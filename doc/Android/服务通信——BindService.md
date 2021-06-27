# BindService
> 如果要创建一个支持绑定的service，我们必须要重写它的onBind()方法。这个方法会返回一个IBinder对象，它是客户端用来和服务器进行交互的接口。而要得到IBinder接口，我们通常有三种方式：继承Binder类，使用Messenger类，使用AIDL。

以一个例子介绍，将Activity作为客户端，Service作为服务端，实现两者之间的通信
Service侧代码
```java
public class MyService extends Service {
    private DownloadBinder mBinder = new DownloadBinder();

    class DownloadBinder extends Binder {
        public void startDownload() {
            Log.d("MyService", "startDownload executed");
        }
        public int getProgress() {
            Log.d("MyService", "getProgress executed");
            return 0;
        }
    }

    @Override
    // 绑定服务的时候会会回调，返回binder的实例
    public IBinder onBind(Intent intent) {
        // TODO: Return the communication channel to the service.
        return mBinder;
    }
    //此处省略服务创建代码
}
```


Activity侧代码
构建出了一个Intent 对象，然后调用bindService() 方法将MainActivity和MyService进行绑定。bindService() 方法接收3个参数：
- 第一个参数就是刚刚构建出的Intent 对象；
- 第二个参数是前面创建出的ServiceConnection的实例；
- 第三个参数则是一个标志位，这里传入BIND_AUTO_CREATE，表示在活动和服务进行绑定后自动创建服务；（如果这个服务之前还没有创建过，onCreate() 方法会先于onBind() 方法执行。之后，调用方可以获取到onBind() 方法里返回的IBinder 对象的实例，这样就能自由地和服务进行通信了）
```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    private MyService.DownloadBinder downloadBinder;

    private ServiceConnection connection = new ServiceConnection() {
        
        @Override
        public void onServiceDisconnected(ComponentName name) {
        }

        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            downloadBinder = (MyService.DownloadBinder) service;
            downloadBinder.startDownload();
            downloadBinder.getProgress();
        }
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button bindService = (Button) findViewById(R.id.bind_service);
        Button unbindService = (Button) findViewById(R.id.unbind_service);
        bindService.setOnClickListener(this);
        unbindService.setOnClickListener(this);
    }
    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            ...
            case R.id.bind_service:
                Intent bindIntent = new Intent(this, MyService.class);

                // 绑定服务，绑定之后会调用ServiceConnection的onServiceConnected()方法
                bindService(bindIntent, connection, BIND_AUTO_CREATE); 
                
                break;
            case R.id.unbind_service:
                // 解绑服务，解绑之后会调用ServiceConnection的onServiceDisconnected()方法
                unbindService(connection); 
                break;
            default:
                break;
        }
    }
}
```