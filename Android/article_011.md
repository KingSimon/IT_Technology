# 简单拨打电话程序
众所周知,对于一个手机,能拨打电话是其最重要也是最常用的一个功能.而在Android里是怎么样实现拨打电话的程序呢?我在这里写了一个简单的拨打电话的Demo,供大家参考.一共分为5个步骤.



## Step 1:新建一个Android工程,命名为phoneCallDemo.




## Step 2:设计程序的界面,打开main.xml把内容修改如下:


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
    android:text="Please input the phoneNumer:"
    />
<EditText
 android:id="@+id/et1"
 android:layout_width="fill_parent"
 android:layout_height="wrap_content"
 android:phoneNumber="true"
/>
<Button
 android:id="@+id/bt1"
 android:layout_width="wrap_content"
 android:layout_height="wrap_content"
 android:text="Call Phone"
/>
</LinearLayout>
```

## Step 3:增加拨打电话的权限,打开AndroidManifest.xml,修改代码如下:


```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.android.test"
      android:versionCode="1"
      android:versionName="1.0">
    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".PhoneCallDemo"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>
    <uses-sdk android:minSdkVersion="3" />
 <!-- 添加拨出电话的权限 -->
 <uses-permission android:name="android.permission.CALL_PHONE">
 </uses-permission>
</manifest>
```


## Step 4:主程序phoneCallDemo.java代码如下:


```
package com.android.test;

import android.app.Activity;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class PhoneCallDemo extends Activity {

 private Button bt;
 private EditText et;
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        //取得资源
        bt = (Button)findViewById(R.id.bt1);
        et = (EditText)findViewById(R.id.et1);

        //增加事件响应
        bt.setOnClickListener(new Button.OnClickListener(){

   @Override
   public void onClick(View v) {

    //取得输入的电话号码串
    String inputStr = et.getText().toString();
    //如果输入不为空创建打电话的Intent
    if(inputStr.trim().length()!=0)
    {
     Intent phoneIntent = new Intent("android.intent.action.CALL",
       Uri.parse("tel:" + inputStr));
     //启动
     startActivity(phoneIntent);
    }
    //否则Toast提示一下
    else{
     Toast.makeText(PhoneCallDemo.this, "不能输入为空", Toast.LENGTH_LONG).show();
    }
   }

        });
    }
}
```


## Step 5:运行代码,效果如下:

