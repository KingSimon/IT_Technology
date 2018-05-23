# BroadcastReceiver基础总结
Broadcast分为三种：
1.普通广播   无序
  this.sendBroadcast(i)
  this.getContext().sendBroadcast(i, "权限")
2.有序广播  (可以配置有序广播的优先级)
  this.getContext().sendOrderedBroadcast(i, null);
3.粘性广播
  this.sendStickyBroadcast(i);

广播的发送意图有两种：
1.显示意图   仅一方接收
2.隐式意图   可以多方接收
  只需在注册接收者的时候将action设为一样即可
  发布者可以//指明接收方需要具备的权限

广播接收者的注册方式有两种
1.声明式注册 AndroidManifestxml
2.编程式注册  .java
  接收者可以 permission约束 发送方需要具备的权限

广播主要是进程间通信
   当两个或者多个进程发送消息时，广播接收者不需要在线，在三大组件中receiver(广播)的生命周期最短。


广播接受者的响应
   每次广播到来时,会重新创建receiver对象,并且调用onReceive()方法,执行完以后,该对象即被销毁.当onReceive()方法在10秒内没有执行完毕,系统会认为该程序无响应.所以在BroadcastReceiver里不能做一些比较耗时的操作,否侧会弹出ANR(Application No
Response)的对话框。***如果需要完成一项比较耗时的工作,应该通过发送Intent给Service，由Service来完成。这里不能使用子线程来解决，因为BroadcastReceiver的生命周期很短，子线程可能还没有结束BroadcastReceiver就先结束了。***BroadcastReceiver一旦结束，此时BroadcastReceiver的所在进程很容易在系统需要内存时被优先杀死，因为它属于空进程(没有任何活动组件的进程).如果它的宿主进程被杀死,那么正在工作的子线程也会被杀死.所以采用子线程来解决是不可靠的。
```
public void onReceive(Context context, Intent intent) {
//发送Intent启动服务,由服务来完成比较耗时的操作
Intent service = new Intent(context, XxxService.class);
context.startService(service);
}
```
### BroadcastReceiver基础总结

BroadcastReceiver是Android四大组件之一，主要负责接收系统或其他程序发出的广播，在开发中，通常用做事件驱动的起源，比如开机就要开启一个程序，有网络就要开始下载资源，安装或卸载包了，就要跟新UI等等。以下就对这个组件总结我自己的理解：

### BroadcastReceiver的生命周期

      BroadcastReceiver的生命周期很短，当系统或其他程序发出广播的时候，Android系统的包管理对象就会检查所有已安装的包中的配置文件有没有匹配的action，如果有，并且可以接收，那么就调用这个BroadcastReceiver，获取BroadcastReceiver对象，然后执行onReceiver()，这个时候BroadcastReceiver对象就没有了，被摧毁了，所有说BroadcastReceiver的生命周期是非常短的。切记，在onReveiver()中不能做比较耗时的操作，不然会弹出ANR对话框。如果真的要完成耗时的工作的话，可以交给Service去完成。另外，onReceiver()中也不能用子线程来操作，大家都知道，当父进程被杀死后，它的子进程也会被杀死，所以，这是很不安全的做法。



### 按活动模式来分类BroadcastReceiver

* 常驻型广播：不随Activity的生命周期而改变，即使Activity被摧毁了，这个BroadcastReceiver依然能够接受广播，一般在配置文件中定义。

* 非常驻型广播：随着Activity的生命周期而改变，在程序中定义。

到底用哪一种比较好，这里要看程序的需求。比如，需要捕捉开机事件就启动一个包，那么，这个最好是用常驻型广播。比如，系统设置的应用程序管理界面中，监听是否有包被安装，以便跟新UI，这里最好是使用非常驻型广播好。



### 按优先级来分类BroadcastReceiver

* 有序广播：接收广播的时候有优先级，优先级在receiver中的android:property属性中指定，数值越大优先级越高。其优先级范围是[-1000,1000]，这里需要注意两点。第一，有序广播可以中断广播的继续传送，中断之后，排列在这个广播接收器之后的接收器就接收不到该广播了，只需要在这个广播接收器中的onReceiver()中使用abortBroadcast()方法。第二，接收者可以在接收过程中向即将收到广播的接收器传递数据，这里传递数据用Bundle。代码说明：

前面的接收者可以通过setResultExtras(Bundle)存放数据，下一个接收者可以通过

Bundle mBundle =getResultExtras(true) ;来接收数据。

* 普通广播：这些广播接收器在接收广播的时候是无序的，当然，谁也没有权利来中断这个广播，因为它不存在接收队列！



现在来看看代码，首先总结下注册广播的方法：

#### 第一种方法：在配置文件中注册广播（接收apk包被安装了的广播）

首先定义一个类继承BroadcastReceiver，并且覆写onReceiver方法


```
package dxd.android.test;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.util.Log;

public class MyReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        String action = intent.getAction();
        if(action.equals("android.intent.action.PACKAGE_ADDED")){
            Log.e("MyReceiver","安装了包"+intent.getDataString());
        }
    }
}
```

然后，在配置文件AndroidManifest.xml文件中添加receiver节点

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="dxd.android.test"
      android:versionCode="1"
      android:versionName="1.0">
    <uses-sdk android:minSdkVersion="9" />

    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".MyBroadcastReceiverActivity"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <receiver  android:name=".MyReceiver">
            <intent-filter >
                <action android:name="android.intent.action.PACKAGE_ADDED"/>
                <data android:scheme="package" />
                <!-- 这句一定要加  -->
            </intent-filter>
        </receiver>
    </application>
</manifest>
```

 还有一个主Activity类就没有写出来了，在该类中就根本没有调用任何接收器的方法。



#### 第二种：在程序中注册广播接收器（接收apk被安装了的广播）

         在刚才的代码之上，只需要在主Activity中添加注册和解除广播接收器的代码。

```
package dxd.android.test;

import android.app.Activity;
import android.content.IntentFilter;
import android.os.Bundle;

public class MyBroadcastReceiverActivity extends Activity {
    MyReceiver receiver = new MyReceiver();
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        IntentFilter filter = new IntentFilter();
        filter.addAction("android.intent.action.PACKAGE_ADDED") ;
        registerReceiver(receiver, filter) ;
    }

    @Override
    protected void onDestroy() {
        unregisterReceiver(receiver) ;
        super.onDestroy();
    }
}
```
凡是广播，就逃不出这两种注册方式！可以看到，第一种在配置文件注册的广播就是常驻型广播，它并不受Activity的生命周期的控制，第二种在程序中注册的广播是非常驻型广播，当Activity被摧毁时，这个广播就注销了。这里，说明一下，当有这两种方式共同存在的时候，会以第二种程序中注册的广播为准。 好，之前举例都是接收的系统的广播，那么如何接收自己定义的广播呢？

首先，要明天自己需要发送什么广播，这里就以收到广播之后，去跳转到一个activity为例，广播为：“dxd.android.test.START_ACTIVITY”，名字是自己定义的！随意修改，但是要保持一致。首先还是要写一个类继承自BroadcastReceiver:


```
package dxd.android.test;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.util.Log;

public class DIYReceiver extends BroadcastReceiver {
<span style="white-space:pre">  </span>@Override
<span style="white-space:pre">  </span>public void onReceive(Context context, Intent intent) {
<span style="white-space:pre">      </span>String action = intent.getAction() ;
<span style="white-space:pre">      </span>Log.e("DIYReceiver", "action = " + action) ;
<span style="white-space:pre">      </span>if(action.equals("dxd.android.test.START_ACTIVITY")){
<span style="white-space:pre">          </span>Intent intentTo = new Intent(context,NextActivity.class) ;
<span style="white-space:pre">          </span>intentTo.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);// 没有在activity中启动，必须加这句。
<span style="white-space:pre">          </span>context.startActivity(intentTo) ;
<span style="white-space:pre">      </span>}
<span style="white-space:pre">  </span>}
}
```

然后，在配置文件中书写receiver标签


```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="dxd.android.test"
      android:versionCode="1"
      android:versionName="1.0">
    <uses-sdk android:minSdkVersion="9" />

    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".DIYBroadcastReceiverActivity"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".NextActivity"></activity>

        <receiver android:name=".DIYReceiver"

            <intent-filter>
                              <action android:name="dxd.android.test.START_ACTIVITY"/>
            </intent-filter>
        </receiver>
    </application>
</manifest>
```

现在在程序中发送自己定义的广播：


```
package dxd.android.test;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class DIYBroadcastReceiverActivity extends Activity {
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        Button but = (Button)findViewById(R.id.but);

        but.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(DIYBroadcastReceiverActivity.this ,DIYReceiver.class);
                intent.setAction("dxd.android.test.START_ACTIVITY");
                sendBroadcast(intent);
            }
        });
    }
}
```

可以看到，在DIYBroadcastReceiverActivity页面中有一个按钮，当点击按钮之后就会发出广播，当广播接收器接收到广播之后，就会开始跳转页面。



### 有权限的广播

有时发送者要求广播接收器需要有权限才能接收广播，那么这时该怎么定义呢，其实就只需要添加一个权限即可，这里跟之前发送广播的方法有点小小的不通。看代码


```
package dxd.android.test;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class DIYBroadcastReceiverActivity extends Activity {
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        Button but = (Button)findViewById(R.id.but);

        but.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(DIYBroadcastReceiverActivity.this ,DIYReceiver.class);
                intent.setAction("dxd.android.test.START_ACTIVITY");
                sendBroadcast(intent,"dxd.android.test.permission.receiver");// 这里发送广播的方式不一样
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
    }


}
```
自己写的Receiver几乎跟之前的是一样的

```
public class DIYReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        String action = intent.getAction() ;
        Log.e("DIYReceiver", "action = " + action) ;
        if(action.equals("dxd.android.test.START_ACTIVITY")){
            Intent intentTo = new Intent(context,NextActivity.class) ;
            intentTo.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);// 没有在activity中启动，必须加这句。
            context.startActivity(intentTo) ;
        }
    }
}
```
 主要看配置文件

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="dxd.android.test"
      android:versionCode="1"
      android:versionName="1.0">
    <uses-sdk android:minSdkVersion="9" />

    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".DIYBroadcastReceiverActivity"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".NextActivity"></activity>

        <receiver android:name=".DIYReceiver" >
            <intent-filter>
                <action android:name="dxd.android.test.START_ACTIVITY"/>
            </intent-filter>

        </receiver>

    </application>

    <permission android:name="dxd.android.test.permission.receiver"></permission>
    <uses-permission android:name="dxd.android.test.permission.receiver"/>
</manifest>
```
 综合判断起来，就是sendBroadcast(intent,permission)，和在配置文件中增加了一个权限的声明，和添加这个权限！

到此为止，就只有粘性广播没有总结。之后搞清楚了才写。