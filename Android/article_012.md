# Linkify的应用
本节要讲的是,当我们在一个EditText输入电话或者网址还是Email的时候,让Android自动判断,当我们输入的是电话,我们点击输入内容将调用打电话程序,当我们输入是网址点击将打开浏览器程序.而Linkify很好的解决了这个问题.我们将分四步来完成这个Demo.



## Step 1:新建一个Android工程,命名为LinkifyDemo.



## Step 2:打开main.xml文件,将其内容修改为如下内容:


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
    android:text="请输入电话或者E-mail或者网址:"
    />
<EditText
 android:id="@+id/et1"
 android:layout_width="340px"
 android:layout_height="wrap_content"
/>
<TextView
 android:id="@+id/tv1"
 android:layout_width="fill_parent"
 android:layout_height="wrap_content"
/>
</LinearLayout>
```
## Step 3:在主应用程序LinkifyDemo.java里代码修改如下:


```
package com.android.test;

import android.app.Activity;
import android.os.Bundle;
import android.text.util.Linkify;
import android.view.KeyEvent;
import android.view.View;
import android.widget.EditText;
import android.widget.TextView;

public class LinkifyDemo extends Activity {

 private EditText et;
 private TextView tv;
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        //获取资源
        et = (EditText)findViewById(R.id.et1);
        tv = (TextView)findViewById(R.id.tv1);
        //增加事件响应
        et.setOnKeyListener(new EditText.OnKeyListener(){
   @Override
   public boolean onKey(View v, int keyCode, KeyEvent event) {
          tv.setText(et.getText());
          //判断输入的类型是哪种,并与系统连接
          Linkify.addLinks(tv, Linkify.WEB_URLS|
            Linkify.EMAIL_ADDRESSES|Linkify.PHONE_NUMBERS);

          return false;
   }
        });
    }
}
```


 ## Step 4:运行之.将出现如下效果:







以输入为电话为例,也就是右上角这张图片,当我们点击这个号码时候,系统将自动调用打电话的应用程序,如下图:







扩展学习:



当然我们还有更简单的方法.就是在main.xml里id为tv的TextView里申明这句话也就是:


```
<TextView
 android:id="@+id/tv1"
 android:layout_width="fill_parent"
 android:layout_height="wrap_content"

 android:autoLink="web|phone|email"
/>
```
也能达到同样的效果