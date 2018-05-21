# 仿百度谷歌搜索自动提示框-----AutoCompleteTextView的应用
现在我们上网几乎都会用百度或者谷歌搜索信息,当我们在输入框里输入一两个字后,就会自动提示我们想要的信息,这种效果在Android 里是如何实现的呢? 事实上,Android 的AutoCompleteTextView Widget ,只要搭配ArrayAdapter 就能设计同类似Google 搜索提示的效果.



本例子先在Layout 当中布局一个AutoCompleteTextView Widget ,然后通过预先设置好的字符串数组,将此字符串数组放入ArrayAdapter ,最后利用AutoCompleteTextView.setAdapter 方法,就可以让AutoCompleteTextView 具有自动提示的功能.例如,只要输入ab ,就会自动带出包含ab 的所有字符串列表.



让我们看一下效果图:







下面是我们程序所涉及变动的代码(本例子代码写的相对较少):



首先是main.xml:


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
    android:text="Please input:"
    />
<AutoCompleteTextView
    android:id="@+id/actv"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
</LinearLayout>
```
其次是主控制程序AutoCompleteTextViewDemo.java:


```
package com.android.test;

import android.app.Activity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.AutoCompleteTextView;

public class AutoCompleteTextViewDemo extends Activity {

    private AutoCompleteTextView actv;
    private static final String[] autoStrs = new String[]{"a","abc","abcd","abcde","ba"};
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        //通过findViewById()方法取到actv
        actv = (AutoCompleteTextView)findViewById(R.id.actv);
        //new ArrayAdapter对象并将autoStr字符串数组传入actv中
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_dropdown_item_1line,autoStrs);
        actv.setAdapter(adapter);
    }
}
```