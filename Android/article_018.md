# ListView的应用
我们今天要讲的内容是Android中ListView中的实现.一共分为四个步骤,我将一一讲解:



## Step one:创建一个新的Android工程,命名为ListViewDemo.

## Step two:找到ListViewDemo.java,把我们习惯的继承Activity,改成ListActivity,如下:
```
public class ListViewDemo extends ListActivity
```
## Step three:修改ListViewDemo.java代码方法如下:


```
package com.android.test;

import android.app.ListActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;

public class ListViewDemo extends ListActivity {
 static final String[] COUNTRIES = new String[]{
  "Americian","Belize","China","Japan","Korean","Russian"
 };
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
       setListAdapter(new ArrayAdapter<String>(this,android.R.layout.simple_list_item_1
         ,COUNTRIES));
    }
}
```
## Step four:Run it(运行之)效果如下:

