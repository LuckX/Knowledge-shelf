# Messenger实现IPC
> Messenger的核心其实就是Message以及Handler来进行线程间的通信

还是以Service作为服务端，Activity作为客户端
## 服务端代码
服务端主要是返给客户端一个IBinder实例，以供服务端构造Messenger，并且处理客户端发送过来的Message。
```java
public class MessengerServiceDemo extends Service {

    static final int MSG_SAY_HELLO = 1;

    //step1:服务端实现一个Handler,接收客户端消息并处理
    class ServiceHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case MSG_SAY_HELLO:
                    //当收到客户端的message时，显示hello
                    Toast.makeText(getApplicationContext(), "hello!", Toast.LENGTH_SHORT).show();
                    break;
                default:
                    super.handleMessage(msg);
            }
        }
    }

    //step2:使用实现的Handler创建Messenger对象
    final Messenger mMessenger = new Messenger(new ServiceHandler());

    @Nullable
    @Override
    //step3:通过Messenger得到一个IBinder对象，并将其通过onBind()返回给客户端
    public IBinder onBind(Intent intent) {
        Toast.makeText(getApplicationContext(), "binding", Toast.LENGTH_SHORT).show();
        //返回给客户端一个IBinder实例
        return mMessenger.getBinder();
    }
}
```

## 客户端代码
客户端bindService时候会首先回调服务端的onBind函数返回一个IBinder对象，然后调用ServiceConnection的onServiceConnected实例化一个Messenger对象
```java
//客户端
public class ActivityMessenger extends Activity {

    static final int MSG_SAY_HELLO = 1;

    Messenger mService = null;
    boolean mBound;

    // step4:创建ServiceConnection类
    private ServiceConnection mConnection = new ServiceConnection() {
        public void onServiceConnected(ComponentName className, IBinder service) {
            //step5:使用 IBinder 将 Messenger（引用服务的 Handler）实例化
            //接收onBind()传回来的IBinder，并用它构造Messenger
            mService = new Messenger(service);
            mBound = true;
        }

        public void onServiceDisconnected(ComponentName className) {
            mService = null;
            mBound = false;
        }
    };

    //调用此方法时会发送信息给服务端
    public void sayHello(View v) {
        if (!mBound) return;
        //step6:使用实例化的Messenger对象发送消息
        //发送一条信息给服务端
        Message msg = Message.obtain(null, MSG_SAY_HELLO, 0, 0);
        try {
            mService.send(msg);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    protected void onStart() {
        super.onStart();
        //绑定服务端的服务，此处的action是service在Manifests文件里面声明的
        Intent intent = new Intent();
        intent.setAction("com.lypeer.messenger");
        //不要忘记了包名，不写会报错
        intent.setPackage("com.lypeer.ipcserver");
        bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
    }

    @Override
    protected void onStop() {
        super.onStop();
        // Unbind from the service
        if (mBound) {
            unbindService(mConnection);
            mBound = false;
        }
    }
}
```