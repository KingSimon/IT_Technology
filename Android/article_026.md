# Menu功能菜单设计
大家好,我们今天这一节讲的是Android功能菜单的设计,程序里定义了两个菜单子项,一个是"关于",一个是"退出",当点击"关于"时候,新建一个Toast 提示,当点击"退出"时,我们将结束程序.


程序里除了默认覆盖的onCreate 外之外,还需要另外新建两个类函数:onCreateOptionsmenu ()与onOptionsItemSelected (),前者为创建Menu 菜单的项目,后者则是处理菜单被选择运行后的事件处理.


看一下效果图:






我们只在一个文件里作了改动,也就是MenuDemo.java ,代码如下:


```
package com.android.test;

import android.app.Activity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.Toast;

public class MenuDemo extends Activity {
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
    }

    // 创建菜单
    public boolean onCreateOptionsMenu(Menu menu) {
        menu.add(0, 0, 0, "关于");
        menu.add(0, 1, 1, "退出");
        return super.onCreateOptionsMenu(menu);
    }
    //菜单响应
    public boolean onOptionsItemSelected(MenuItem item) {
        super.onOptionsItemSelected(item);
        switch (item.getItemId()) {
        case 0:
            Toast.makeText(MenuDemo.this, "欢迎来到魏祝林的blog", Toast.LENGTH_LONG).show();
        case 1:
            this.finish();
        }
        return true;
    }
}
```