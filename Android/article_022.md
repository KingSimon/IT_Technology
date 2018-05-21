# 调用另一个Activity---Intent对象的使用
前一个教程介绍了如何运用切换Layout 的方式进行手机页面间的转换，如果要转换的页面不只是背景，颜色或文字内容的不同，而是Activity 的置换，那，那就不是单单改变Layout 就能完成的，尤其是需要传递的变量不像网页可以通过Cookie 或Session ，在程序里要移交主动权到另外一个Activity ，光靠先前技巧是办不到的．



而下面我们要讲的Intent 对象就是为解决这问题而生的，Intent 就如同其英文字义，是"想要＂或"意图",之意，在主Activity 当中，告诉程序自己是什么，并想要前往哪里，这就是Intent 对象所处理的事了，本例子和前一个例子我们将实现同一个效果．



看一下效果图：







下面是所涉及的代码：



首先是布局main.xml 及mylayout.xml



main.xml:


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
    android:text="欢迎来到魏祝林的博客"
    />
<Button
    android:id="@+id/bt1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="点击进入Layout2"
/>
</LinearLayout>
```


mylayout.xml


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#ffffffff"
    >
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="Welcome to Mr Wei's blog"
    />
<Button
    android:id="@+id/bt2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="点击进入Layout1"
/>

</LinearLayout>
```


然后是控制程序IntentDemo.java 及IntentDemo1.java 代码：



IntentDemo.java:


```
package com.android.test;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class IntentDemo extends Activity {

    private Button bt1;
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        bt1 = (Button)findViewById(R.id.bt1);

        bt1.setOnClickListener(new Button.OnClickListener(){
            public void onClick(View v){
                //new 一个Intent对象，并指定要启动的Class
                Intent intent = new Intent();
                intent.setClass(IntentDemo.this, IntentDemo1.class);
                //调用一个新的Activity
                startActivity(intent);
                //关闭原本的Activity
                IntentDemo.this.finish();
            }
        });
    }
}
```


在IntentDemo.java 同一目录内新建一个IntentDemo1.java 类



IntentDemo1.java:


```
package com.android.test;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class IntentDemo1 extends Activity {

    private Button bt2;

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 载入mylayout.xml
        setContentView(R.layout.mylayout);
        bt2 = (Button) findViewById(R.id.bt2);
        bt2.setOnClickListener(new Button.OnClickListener() {
            public void onClick(View v) {
                // new 一个Intent对象，并指定要启动的Class
                Intent intent = new Intent();
                intent.setClass(IntentDemo1.this, IntentDemo.class);
                // 调用一个新的Activity
                startActivity(intent);
                // 关闭原本的Activity
                IntentDemo1.this.finish();
            }
        });
    }
}
```


最后是本例子的重点，添加另外一个Activity 所以必须在AndroidManifest.xml 中定义一个新的activty ，并给予名称name ,或则程序无法编译运行．新手很容易遇到这个问题．


```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.android.test"
      android:versionCode="1"
      android:versionName="1.0">
    <application android:icon="@drawable/icon" android:label="@string/app_name">
        <activity android:name=".IntentDemo"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name="IntentDemo1"></activity>
    </application>
    <uses-sdk android:minSdkVersion="3" />

</manifest>
```