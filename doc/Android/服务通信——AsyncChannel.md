> AsyncChannel在两个Handler间搭建了通道，可以用于消息传输。
## 1.工作模式
- 单项通道模式：在该模式下，客户端只能向服务端发起请求，服务端给出回应。
- 双向通道模式：在该模式下，客户端和服务端同时连接上AsyncChannel，客户端可以向服务端发送请求，服务端也可以向客户端发送请求。

## 2.使用方法
### 2.1.单向通道模式
单向通道建立需要三个步骤：
- 准备服务端的Messenger对象；
- 创建本地的Handler对象；
- 创建AsyncChannel对象，然后调用connect()方法；

其中客户端创建过程：
- 获取服务端的Messenger对象，该对象其实就是利用服务端的Handler构建的Messenger；
- 创建客户端自己的Handler对象；
- 创建AsyncChannel对象；
- 通过AsyncChannel对象，连接当前的Handler和服务端的Messenger，从而申请连接；

服务端的过程
- 使用handler创建Messenger对象并提供给客户端；

客户端代码
```java
public static AsyncChannel sClientAsyncChannel;
public int onStartCommand(Intent intent, int flags, int startId) {
    //step1:创建本地的Handler对象
    HandlerThread handlerThread = new HandlerThread("ClientThread");
    handlerThread.start();
    Handler clientHandler = new ClientHandler(handlerThread.getLooper());
    //step2:创建客户端的AsyncChannel
    sClientAsyncChannel = new AsyncChannel();
    String act = intent.getAction();

    if ("Normal".equals(act)) {
        // step3:通过AsyncChannel对象，连接当前的Handler和服务端的Messenger，从而申请连接
        Messenger serviceMessenger = AsyncChannelService.getServiceMessenger();
        //发起连接请求
        sClientAsyncChannel.connect(this, clientHandler, serviceMessenger);
    } else if ("Fast".equals(act)) {
    }
    return super.onStartCommand(intent, flags, startId);
}

//step:当客户端接收到AsyncChannel.CMD_CHANNEL_HALF_CONNECTED消息后，说明当前的单项通道建立成功
public void handleMessage(Message message) {
    switch (message.what) {
        case AsyncChannel.CMD_CHANNEL_HALF_CONNECTED: {
            if (msg.arg1 == AsyncChannel.STATUS_SUCCESSFUL) {
                Log.d(tag, "Service half connected");
            }
            break;
        }
    }
}
```

服务端代码
```java
@AsyncChannelService.java
public static Messenger getServiceMessenger() {
    //step5:服务端需要用自己的Handler构建Messenger然后发送给客户端
    return new Messenger(mServiceHandler);
}

```

---
2.2.双向通道建立
双向通道是在单向通道基础上建立的，在单向通道建立的基础桑还需要实现以下几个步骤：
1. 服务端需要初始化Handler，创建自己的AsyncChannel对象；
2. 客户端在收到CMD_CHANNEL_HALF_CONNECTED之后，向AsyncChannel对象发送CMD_CHANNEL_FULL_CONNECTION消息申请建立双向通道；
3. 服务端收到AysncChannel.CMD_CHANNEL_FULL_CONNECTION的请求后，使用自己的AsyncChannel对象将自己的Handler与客户端的Handler连接起来；

服务端代码
```java
@AsyncChannelService.java
public int onStartCommand(Intent intent, int flags, int startId) {
    //step1:初始化自己的Handler
    HandlerThread handlerThread = new HandlerThread("ServiceThread");
    handlerThread.start();
    mServiceHandler = new ServiceHandler(handlerThread.getLooper());
    //step2:创建AsyncChannel对象
    sServiceAsyncChannel = new AsyncChannel();
    return super.onStartCommand(intent, flags, startId);
}
```

客户端代码
```java
@AsyncChannelClient.java
public void handleMessage(Message message) {
    switch (message.what) {
        case AsyncChannel.CMD_CHANNEL_HALF_CONNECTED:
            if (message.arg1 == AsyncChannel.STATUS_SUCCESSFUL) {
                //step3:客户端单项通道建立完成，继续申请双向通道
                sClientAsyncChannel.sendMessage(AsyncChannel.CMD_CHANNEL_FULL_CONNECTION);
            }
            break;
    }
}
```

服务端代码
```java
@AsyncChannelService.java
public void handleMessage(Message msg) {
    switch ((msg.what)) {
    case AsyncChannel.CMD_CHANNEL_FULL_CONNECTION: {
        //step4：服务端接收到双向通道的建立请求,如果同意客户端建立，则需要服务端向AsyncChannel申请连接请求
        sServiceAsyncChannel.connect(AsyncChannelService.this, mServiceHandler, msg.replyTo);
        break;
    }
    }
}
````

---
2.3.fullyConnectSync方式建立双向通道
创建双向通道基本和单向相同，具体步骤如下
客户端：
1. 初始化Handler对象；
2. 创建AsyncChannel对象；
3. 使用fullyConnectSync()方法建立双向通道，此时使用的是服务端的Handler对象(serviceHandler)而不是Messenger对象；

服务端：
1. 初始化Handler对象；
2. 创建AsyncChannel对象；
3. 客户端通过fullyConnectSync()申请双向通道时，服务端可以收到CMD_CHANNEL_FULL_CONNECTION的消息，然后服务端需要连接上AsyncChannel后回复成功的命令即可完成通道的建立；

服务端代码
```java
@AsyncChannelServiceFull.java
public int onStartCommand(Intent intent, int flags, int startId) {
    //step1:初始化Handler对象
    HandlerThread handlerThread = new HandlerThread("ServiceThread");
    handlerThread.start();
    mServiceHandler = new ServiceHandler(handlerThread.getLooper());
    //step2:创建AsyncChannel对象
    sServiceAsyncChannel = new AsyncChannel();
    return super.onStartCommand(intent, flags, startId);
}
```

客户端代码
```java
@AsyncChannelClient.java
public int onStartCommand(Intent intent, int flags, int startId) {
    //step3:初始化Handler对象
    HandlerThread handlerThread = new HandlerThread("ClientThread");
    handlerThread.start();
    Handler clientHandler = new ClientHandler(handlerThread.getLooper());
    //step4:创建AsyncChannel对象
    sClientAsyncChannel = new AsyncChannel();

    Handler serviceHandler = AsyncChannelServiceFull.getServiceHandler();

    //step5:使用fullyConnectSync()方法建立双向通道
    //result返回值，可以明确知道通道是否建立成功
    int result = sClientAsyncChannel.fullyConnectSync(this, clientHandler, serviceHandler);
    if (AsyncChannel.STATUS_SUCCESSFUL == result) {
         Log.d(tag, "client full connected");
    }
    return super.onStartCommand(intent, flags, startId);
}
```

客户端代码
```java
@AsyncChannelServiceFull.java
public void handleMessage(Message msg) {
    switch (msg.what) {
        case AsyncChannel.CMD_CHANNEL_FULL_CONNECTION: {
            //step6:收到双向通道建立请求，如果服务端同意，则需要连接上AsyncChannel，然后恢复STATUS_SUCCESSFUL即可
            sServiceAsyncChannel.connect(AsyncChannelServiceFull.this, mServiceHandler, msg.replyTo);
            //将建立结果传递给客户端
            sServiceAsyncChannel.replyToMessage(msg, AsyncChannel.CMD_CHANNEL_FULLY_CONNECTED, AsyncChannel.STATUS_SUCCESSFUL);
            break;
        }
    }
}
```