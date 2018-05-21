# 动态更改屏幕方向的简单例子(LANDSCAPE与PORTRAIT)
大家好，今天要讲的是Android手机如何动态手机屏幕方向的，我们当中有可能手机也会有这种功能，当我们手机方向改变时，屏幕也会跟着改变，在这Android当中是很容易实现的．本节的Demo主要是界面有一个按钮，当点击时，如果屏幕方向是横排(PORTRAIT)刚将屏幕方向更改为竖排(LANDSCAPE),反之依然！我们这里主要是运用了getRequestedOrientation(),和setRequestedorientation()两个方法．但是要利用这两个方法必须先在AndroidManiefst.xml设置一下屏幕方属性，不然程序将不能正常的工作．下面我将分为N个步骤一步一步教你如何实现该Demo.



## Step 1:
我们建立一个Android工程,命名为ChangeOrientationDemo.



## Step 2:
设计UI,打开main.xml,将其代码修改如下，我们这里只是增加了一个按钮，其他什么都没有动．


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="@string/hello"
    />
<Button
 android:id="@+id/bt1"
 android:layout_width="wrap_content"
 android:layout_height="wrap_content"
 android:text="Press me Change Orientation"
/>
</LinearLayout>
```


## Step 3:
设计主程序ChangeOrientationDemo.java,修改其代码如下:


```
package com.android.test;

import android.app.Activity;
import android.content.pm.ActivityInfo;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class ChangeOrientationDemo extends Activity {

 private Button bt1;

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        //获取资源
        bt1 = (Button)findViewById(R.id.bt1);
        //增加按钮事件
        bt1.setOnClickListener(new Button.OnClickListener(){

   @Override
   public void onClick(View v) {
    //如果是竖排,则改为横排
    if(getRequestedOrientation() == ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE)
    {
     setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
    }

    //如果是横排,则改为竖排

    else if(getRequestedOrientation() == ActivityInfo.SCREEN_ORIENTATION_PORTRAIT)
    {
     setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
    }
   }

        });
    }

}
```


## Step 4:
在AndroidManifest.xml文件里设置默认方向，不然程序不能正常工作哦．代码如下：


```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.android.test"
      android:versionCode="1"
      android:versionName="1.0">
    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".ChangeOrientationDemo"
                  android:label="@string/app_name"
                  android:screenOrientation="portrait">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>
    <uses-sdk android:minSdkVersion="3" />

</manifest>
```


## Step 5:
运行程序，效果如下图:







 OK,今天就到这里，谢谢大家！