# Android开发中在一个Activity中关闭另一个Activity
比如有ActivityA， ActivityB，在ActivityB中关闭ActivityA


解决方案：
1.
在 ActivityA 里面设置一个静态的变量instance,初始化为this
在 ActivityB 里面, ActivityA.instance.finish();

2.
也可以通过ActivityManager
```
ActivityManager manager = (ActivityManager)getSystemService(ACTIVITY_SERVICE);
manager.restartPackage(packageName);
```

```
ActivityA
package com.activity.yuzhenbei;
import android.os.Bundle;
import android.app.Activity;
import android.content.Intent;
import android.view.KeyEvent;
import android.view.Menu;
import android.view.MenuItem;
public class MainActivity extends Activity {
    public static ActivityA instance = null;
@Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activitya);
        instance = this;
        Intent intent = new Intent();
intent.setClass(ActivityA.this, ActivityB.class);
ActivityA.this.startActivity(intent);
    }
        // Menu
  // 当点击Menu按钮时，调用该方法
  @Override
  public boolean onCreateOptionsMenu(Menu menu) {
  menu.add(0, 1, 1, R.string.help).setIcon(
  android.R.drawable.ic_menu_close_clear_cancel);
  return super.onCreateOptionsMenu(menu);
  }


  // 选中某个菜
  @Override
  public boolean onOptionsItemSelected(MenuItem item) {
  if (item.getItemId() == 1) {
  Intent intent = new Intent();
intent.setClass(ActivityA.this, ActivityB.class);
ActivityA.this.startActivity(intent);
  }
  return super.onOptionsItemSelected(item);
  }

       // 返回键
  @Override
  public boolean onKeyDown(int keyCode, KeyEvent event) {
  if (keyCode == KeyEvent.KEYCODE_BACK) { // 如果是手机上的返回键
  ActivityA.this.finish();
  }
  return super.onKeyDown(keyCode, event);
  }
}


ActivityB
package com.activity.yuzhenbei;
import android.app.Activity;
import android.os.Bundle;
import android.view.KeyEvent;
import android.view.View;
public class  ActivityB extends Activity {
@Override
public void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activityb);
}
// 返回键
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
if (keyCode == KeyEvent.KEYCODE_BACK) { // 如果是手机上的返回键
ActivityB.this.finish();
ActivityA.instance.finish();
}
return super.onKeyDown(keyCode, event);
}

}
```