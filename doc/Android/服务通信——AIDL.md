# AIDL
> Android接口定义语言，全称是`Android Interface Definition Language`
## AIDL使用
### 定义AIDL文件
在Android studio中直接可以new一个studio文件，生成之后的默认文件如下
```java
// IMyAidlInterface.aidl
package cc.abto.demo;

// Declare any non-default types here with import statements

interface IMyAidlInterface {
    /**
     * Demonstrates some basic types that you can use as parameters
     * and return values in AIDL.
     */

     // AIDL中可以使用的基本类型（int, long, boolean, float, double, String）
    void basicTypes(int anInt, long aLong, boolean aBoolean, float aFloat,
            double aDouble, String aString);
}
```
此时可以修改idel文件，写上需要提供给其他进程使用的接口
```java
//step1:创建AIDL对象；
interface IMyAidlInterface {
   String getName();
}
```
编译之后会自动生成一个`IMyAidlInterface`的接口
### 服务端代码
服务端通过binder通信，此时需要实现`onBind`方法返回一个`IBinder`对象，这个对象是通过继承生成的`IMyAidlInterface`中的`Stub`实现的，继承该方法的时候需要实现上面`AIDL`定义的接口，供其他进程调用；
```java
public class MyService extends Service
{

    public MyService()
    {

    }

    @Override
    //step3:需要返回一个IBinder的对象
    public IBinder onBind(Intent intent)
    {
        return new MyBinder();
    }

    //step2:继承IMyAidlInterface接口中的IMyAidlInterface.Stub，并实现其中的方法
    class MyBinder extends IMyAidlInterface.Stub
    {

        @Override
        public String getName() throws RemoteException
        {
            return "test";
        }
    }
}
```
### 客户端代码
客户端通过IMyAidlInterface.Stub.asInterface获得IMyAidlInterface对象，让后通过bindservice实现和服务端绑定，后面的实现机制和bindService一致；最后通过IMyAidlInterface对象跨进程调用其方法；

```java
public class MainActivity extends AppCompatActivity
{


    private IMyAidlInterface iMyAidlInterface;

    @Override
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // step5:通过bindService连接客户端和服务器
        bindService(new Intent("cc.abto.server"), new ServiceConnection()
        {

            @Override
            public void onServiceConnected(ComponentName name, IBinder service)
            {
                // step4:通过IMyAidlInterface.Stub.asInterface获得IMyAidlInterface对象，同时创建一个ServiceConnection对象
                iMyAidlInterface = IMyAidlInterface.Stub.asInterface(service);
            }

            @Override
            public void onServiceDisconnected(ComponentName name)
            {

            }
        }, BIND_AUTO_CREATE);
    }

    public void onClick(View view)
    {
        try
        {
            Toast.makeText(MainActivity.this, iMyAidlInterface.getName(), Toast.LENGTH_SHORT).show();
        }
        catch (RemoteException e)
        {
            e.printStackTrace();
        }
    }
}
```

## 参考文章
- [Android：学习AIDL，这一篇文章就够了(上)](https://www.jianshu.com/p/a8e43ad5d7d2)
- [Android：学习AIDL，这一篇文章就够了(下)](https://www.jianshu.com/p/0cca211df63c)
- [Android中AIDL的使用详解](https://www.jianshu.com/p/d1fac6ccee98)