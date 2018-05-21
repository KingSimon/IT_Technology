# 获取手机屏的大小DisplayMetrics的应用
 获取手机屏幕的大小  此时获取的宽高是以像素为单位的，此处的像素是绝对像素而非相对像素



          程序一开始所创建的DisplayMetrics 实例化的时候不需要传递参数但是在调用getWindowManager().之后会取得现有的Activity的Handler 此时调用getDefaultDisplay().方法将取得的宽高存放于DisplayMetrics对象dm中



```

//必须引用android.util.DisplayMetrics

DisplayMetrics dm=new DisplayMetrics();

getWindowManager().getDefaultDisplay().getMetrics(dm);

//strOpt就是屏幕的大小再将其赋值

String strOpt="手机屏幕大小："+dm.widthPixels+"x"+dm.heightPixels;

 ```



大家好，我们这一节要讲的内容是Android如何取得手机屏幕大小的例子.本节主要用了三个对象TextView,Button ,以及DisplayMetrics ,其中Displaymetrics 是取得手机屏幕大小的关键类，这个例子非常的简单，当我们点击按钮，触发事件，在TextView 里显示手机屏幕的宽高分辨率．



看一下效果图:



按钮触发前:







按钮触发后:







其中我们在res->layout->values->string.xml增加了两行如下(黑体)


```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello World, DisplayMetricsDemo!</string>
    <string name="app_name">DisplayMetricsDemo</string>
    <string name="resolution">手机分辨率为:</string>
    <string name="pressme">按我获分辨率</string>
</resources>
```


而布局文件main.xml代码如下:


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView
    android:id="@+id/textview1"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="@string/resolution"
    />
<Button
    android:id="@+id/button1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/pressme"
/>
</LinearLayout>
```


最后是我们主类DisplaymetricsDemo.java，代码如下:


```
package com.android.test;

import android.app.Activity;
import android.os.Bundle;
import android.util.DisplayMetrics;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class DisplayMetricsDemo extends Activity {

    private TextView textview1;
    private Button button1;
    //获取手机屏幕分辨率的类
    private DisplayMetrics dm;
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        //获取布局中TextView,Button对像
        textview1 = (TextView)findViewById(R.id.textview1);
        button1 = (Button)findViewById(R.id.button1);

        //增加button事件响应

        button1.setOnClickListener(new Button.OnClickListener(){
            public void onClick(View v)
            {
                dm = new DisplayMetrics();
                getWindowManager().getDefaultDisplay().getMetrics(dm);
                //获得手机的宽带和高度像素单位为px
                String str = "手机屏幕分辨率为:" + dm.widthPixels
                +" * "+dm.heightPixels;
                textview1.setText(str);
            }
        });

    }
}
```