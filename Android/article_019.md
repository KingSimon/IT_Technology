# 自定义下拉菜单模式----Spinner与setDropDownViewResource的应用
大家好,我们这一节讲一下Android下的下拉菜单Spinner,就像是Swing的Combobox,html的<select>,由于手机画面有限,要在有限的范围选择项目,下拉菜单是比较好的选择.



Android提供的Spinner Widget的下拉菜单已经非常好用了,样式也还适用.但我们本节的Demo的重点在于自定义下拉菜单里的样式,其关键在于调用setDropDownViewResource方法,以XML的方式定义下拉菜单要显示的模样.



## Step 1:创建一个新的Android工程,我们命名为SpinnerDemo.

## Step 2:打开layout文件夹,找到main.xml将其内容改为:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"   android:padding="10dip"android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="10dip"
    android:text="Please select a planet:"
/>
   <Spinner
       android:id="@+id/spinner"
       android:layout_width="fill_parent"
       android:layout_height="wrap_content"
       android:drawSelectorOnTop="true"
       android:prompt="@string/planet_prompt"
   />
</LinearLayout>
```
注意:
the Spinner'sandroid:prompt
is a string resource. In this case, Android does not allow it to be a string, it must
be a reference to a resource.  So...

## Step 3
:找到在res/values/string.xml,
在里面加入如下(黑体
)一行:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello World, SpinnerDemo!</string>
    <string name="app_name">SpinnerDemo</string>
    <string name="planet_prompt">Choose a planet</string>
</resources>
```
## Step 4
:在res/values/
文件夹下创建一个xml
文件,命名为arrays.xml:
```
<resources>
<string-array name="planets">
<item>Mercury</item>
<item>Venus</item>
<item>Earth</item>
<item>Mars</item>
<item>Jupiter</item>
<item>Saturn</item>
<item>Uranus</item>
<item>Neptune</item>
</string-array>
</resources>
```
这是用户可以从Spinner Widget选择list 的选择项.

## Step5
:打开SpinnerDemo.java
,编辑内容如下:
```
package com.android.test;
import android.app.Activity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.Spinner;

public classSpinnerDemo
extends Activity{
public void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.main);
Spinner s = (Spinner) findViewById(R.id.spinner);
   ArrayAdapter adapter = ArrayAdapter.createFromResource(
           this, R.array.planets, android.R.layout.simple_spinner_item)
   adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
   s.setAdapter(adapter);
}
}
```
## step 6:
最后run it
(运行之)效果如下:
