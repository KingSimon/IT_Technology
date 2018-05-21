# 多选项CheckBox的综合应用
大家好,我们这一节将讲多选项CheckBox 的综合应用,我们的程序主要构造两个CheckBox 的对象,以及一个TextView对象,并通过setOnCheckedChangeLisener 实现onCheckedChanged ()方法来更新TextView 文字.



首先我们看一下效果图:















下面是主程序的代码:



string.xml:


```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello World, CheckboxDemo!</string>
    <string name="app_name">CheckboxDemo</string>
    <string name="hobby">你的爱好是:</string>
    <string name="basketball">篮球</string>
    <string name="football">足球</string>
</resources>
```


主程序界面代码main.xml


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
    android:text="@string/hobby"
    />
<CheckBox
    android:id="@+id/checkbox1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/basketball"
/>
<CheckBox
    android:id="@+id/checkbox2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/football"
/>
</LinearLayout>
```


最后是程序的核心代码CheckBoxDemo:


```
package com.android.test;

import android.app.Activity;
import android.os.Bundle;
import android.widget.CheckBox;
import android.widget.CompoundButton;
import android.widget.TextView;

public class CheckboxDemo extends Activity {

    private TextView tv;
    private CheckBox cb1;
    private CheckBox cb2;
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        tv = (TextView)findViewById(R.id.textview1);
        cb1 = (CheckBox)findViewById(R.id.checkbox1);
        cb2 = (CheckBox)findViewById(R.id.checkbox2);

        cb1.setOnCheckedChangeListener(cbListener);
        cb2.setOnCheckedChangeListener(cbListener);


    }

    private CheckBox.OnCheckedChangeListener cbListener =
        new CheckBox.OnCheckedChangeListener(){

        public void onCheckedChanged(CompoundButton buttonView,boolean isChecked)
        {
            String stv = getString(R.string.hobby);
            String scb1 = getString(R.string.basketball);
            String scb2 = getString(R.string.football);
            //判断一共有四种情况
            if(cb1.isChecked()== true && cb2.isChecked()== true)
            {
                tv.setText(stv + scb1 + "," + scb2);
            }
            else if(cb1.isChecked()== true && cb2.isChecked()== false)
            {
                tv.setText(stv+scb1);
            }
            else if(cb1.isChecked() == false && cb2.isChecked() == true)
            {
                tv.setText(stv+scb2);
            }
            else{
                tv.setText(stv);
            }
        }
    };

}
```