# 不同Activity之间的数据传递---Bundle对象的使用
在上一节例子中,介绍了如何在Activity 中调用另一个Activity ,但若需要在调用 另外一个Activity 的同时传递数据,那么就需要利用Android.os.Bundle 对象封装数据的能力,将欲传递的数据或参数通过Bundle 来传递不同Intent 之间的数据.



本范例将设计一个简单的个人信息表单,有姓名(EditText )和性别(RadioButton )还有一个提交按钮(Button ),当点击提交按钮时,另外一个页面将显示个人信息.



首先我们看一下效果图:









下面是我们所涉及有变动的代码:



首先是主页面布局main.xml 和第二个页面的布局mylayout.xml



main.xml 这里我们用了AbsoluteLayout布局大家 要注意哦


```
<?xml version="1.0" encoding="utf-8"?>
<AbsoluteLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="Person information"
    />
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="Name:"
    android:layout_y="30px"
/>
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="Gender:"
    android:layout_y="80px"
/>
<EditText
    android:id="@+id/ed1"
    android:layout_width="200px"
    android:layout_height="wrap_content"
    android:layout_x="50px"
    android:layout_y="20px"
/>
<RadioButton
    android:id="@+id/rb1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="man"
    android:layout_x="60px"
    android:layout_y="65px"
    android:checked="true"//默认它是被选中的
/>
<RadioButton
    android:id="@+id/rb2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="woman"
    android:layout_x="160px"
    android:layout_y="65px"
/>
<Button
    android:id="@+id/bt1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="confirm"
    android:layout_y="120px"
/>
</AbsoluteLayout>
```


在main.xml 同一目录下建另外一个页面布局mylayou.xml


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView
    android:id="@+id/mytv"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="@string/hello"
    />
</LinearLayout>
```


下面是程序的核心代码BundleDemo.java 和BundleDemo1.java



BundleDemo.java:


```
package com.android.test;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.RadioButton;

public class BundleDemo extends Activity {

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        //以findViewById()取得Button对象,并添加响应
        Button bt1 = (Button)findViewById(R.id.bt1);
        bt1.setOnClickListener(new Button.OnClickListener(){
            public void onClick(View v){
                //取得name
                EditText et = (EditText)findViewById(R.id.ed1);
                String name = et.getText().toString();
                //取得Gender
                String sex="";
                RadioButton rb1 = (RadioButton)findViewById(R.id.rb1);
                if(rb1.isChecked())
                {
                    sex="man";
                }else{
                    sex="woman";
                }
                //new一个Intent对象,用法上节已经讲过了
                Intent intent = new Intent();
                intent.setClass(BundleDemo.this, BundleDemo1.class);
                //new一个Bundle对象,并将要传递的数据导入,这里是重点Map<String,String>结构哦
                Bundle bundle = new Bundle();
                bundle.putString("name",name);
                bundle.putString("sex", sex);
                //将Bundle对象assign给Intent
                intent.putExtras(bundle);
                //调用Activity BundleDemo1
                startActivity(intent);
            }
        });
    }
}
```




在BundleDemo.java 同一目录建立BundleDemo1.java :


```
package com.android.test;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;


public class BundleDemo1 extends Activity {

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.mylayout);
      //取得Intent中的Bundle对象
        Bundle bundle = this.getIntent().getExtras();
        //取得Bundle对象中的数据
        String name = bundle.getString("name");
        String sex = bundle.getString("sex");
        //设置输出文字
        TextView mytv = (TextView)findViewById(R.id.mytv);
        mytv.setText("you name is:" + name + "/n you gender is:"+sex);
    }
}
```




最后我们还是要在AndroidManifest.xml 中加入BundleDemo1 这个Activity ,一定要加,不然后果自负呵呵~


```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.android.test"
      android:versionCode="1"
      android:versionName="1.0">
    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".BundleDemo"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    <activity android:name="BundleDemo1"></activity>
    </application>
    <uses-sdk android:minSdkVersion="3" />

</manifest>
```