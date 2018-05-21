# 手机页面的转换setContentView的应用
大家好,我们这一节讲的是手机页面的转换setContentView 的应用.在网页的世界里,想要在两个页面间的转换,只要利用超链接就可以实现,

但是在手机的世界里,要如何实现手机页面的转换呢? 最简单的方法就是改变Activity 的Layout !



    在这个例子中,将布局两个Layout ,分别为Layout1(main.xml) 和Layout2(mylayout.xml), 默认的Layout为main.xml, 我们在Layout1 当中创建一个按钮,当单击按钮时,显示第二个Layout(mylayout.xml) ;同样地,在Layout2 里也设计一个按钮,当单击第二个Layout 的按钮之后,刚显示回原来的Layout1 ,现在就来示范如何在两个页面之间互相切换.



首先看一下效果图(为了区别两个Layout ,我们分别设置了不同的背景色):











下面是我们本程序所涉及的相关代码,首先是主界面布局main.xml


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


其次我们在main.xml 同一目录新建一个为mylayout.xml 文件,代码如下:


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


最后是我们的核心程序setContentViewDemo.java


```
package com.android.setContentViewDemo;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class setContentViewDemo extends Activity {

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 载入main.xml Layout
        setContentView(R.layout.main);

        // 以findViewById()取得Button对象并添加事件onClickLisener
        Button bt1 = (Button) findViewById(R.id.bt1);
        bt1.setOnClickListener(new Button.OnClickListener() {
            public void onClick(View v) {
                goToLayout2();
            }
        });
    }

    // 将layout由main.xml切换成mylayout.xml
    public void goToLayout2() {
        // 将layout改成mylayout
        setContentView(R.layout.mylayout);
        Button b2 = (Button) findViewById(R.id.bt2);
        b2.setOnClickListener(new Button.OnClickListener() {
            public void onClick(View v) {
                goToLayout1();
            }
        });
    }

    // 将layout由mylayout.xml切换成main.xml
    public void goToLayout1() {
        setContentView(R.layout.main);
        Button bt1 = (Button) findViewById(R.id.bt1);
        bt1.setOnClickListener(new Button.OnClickListener() {
            public void onClick(View v) {
                goToLayout2();
            }
        });
    }
}
```