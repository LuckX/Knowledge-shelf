> AsyncChannel在两个Handler间搭建了通道，可以用于消息传输。
## 工作模式
- 单项通道模式：在该模式下，客户端只能向服务端发起请求，服务端给出回应。
- 双向通道模式：在该模式下，客户端和服务端同时连接上AsyncChannel，客户端可以向服务端发送请求，服务端也可以向客户端发送请求。

## 使用方法
### 单向通道模式
客户端代码
```java
        public static AsyncChannel sClientAsyncChannel;
        public int onStartCommand(Intent intent, int flags, int startId) {
            //step1:创建本地的Handler对象
            HandlerThread handlerThread = new HandlerThread("ClientThread");
            handlerThread.start();
            Handler clientHandler = new ClientHandler(handlerThread.getLooper());
            //创建客户端的AsyncChannel
            sClientAsyncChannel = new AsyncChannel();
            String act = intent.getAction();
            //我们发送的Action就是"Normal"
            if ("Normal".equals(act)) {
                // 建立单向通道，获取服务端的Messenger对象
                Messenger serviceMessenger = AsyncChannelService.getServiceMessenger();
                //发起连接请求
                sClientAsyncChannel.connect(this, clientHandler, serviceMessenger);
            } else if ("Fast".equals(act)) {
            }
            return super.onStartCommand(intent, flags, startId);
        }

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
            //服务端需要用自己的Handler构建Messenger然后发送给客户端
            return new Messenger(mServiceHandler);
        }

```

服务端代码
```java
```


## 参考资料
[AsyncChannel的使用和内部原理](https://blog.csdn.net/u010961631/article/details/48179305?spm=1001.2014.3001.5501)