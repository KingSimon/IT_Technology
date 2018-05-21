# 简单的Button事件响应综合提示控件Toast的应用
Button按钮所触发的事件处理,我们称之为Event Handle,只不过在Android当中,按钮事件是由系统的Button.OnClickListener所控制,熟悉Java程序设计的读者对OnXxxListener应该不陌生.以下的Demo,我们将实现当点击Button时,TextView文字将发生改变,并在屏幕上出现一段时间的Toast提醒.



让我们看一下效果图:



点击按钮前:







点击按钮后:







我们主要在程序里改了两处地方一处是main.xml 另一处是ButtonDemo.java



Main.xml 代码如下:


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" //1.5以后默认的是LinearLayout布局
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView
    android:id="@+id/textview1" //定义Id方便Java类找到它,并且控制它
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="@string/hello"
    />
<Button
    android:id="@+id/button1"
    android:layout_width="60px"
    android:layout_height="wrap_content"

    android:layout_gravity="right" //让Button放在右面
    android:text="确定"

/>
</LinearLayout>
```


Button.java 代码如下:


```
package com.android.test;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class ButtonDemo extends Activity {

    private TextView textview1;
    private Button button1;
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        //通过ID在找到定义在main.xml里的TextView和Button控件
        textview1 = (TextView)findViewById(R.id.textview1);
        button1 = (Button)findViewById(R.id.button1);
        //增加事件响应

        button1.setOnClickListener(new Button.OnClickListener(){
            public void onClick(View v)
            {
                //Toast提示控件
                Toast.makeText(ButtonDemo.this,
                        "TextView里的文字发生了改变,你注意到了吗?",
                        Toast.LENGTH_LONG).show();
                //将TextView的文字发生改变
                textview1.setText("欢迎来到魏祝林的博客!");
            }
        });
    }

}
```