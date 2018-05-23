# Android 中的全局变量
当我们需要在整个应用程序中定义全局变量时，可通过扩展 Android 的 Application 类来实现，这里是一个基础的类用来操作全局的应用状态。

下面是创建全局变量的步骤：

1) 创建一个新类扩展自 Application 类：

```
public  class  Global extendsApplication {
    privateBoolean _notification=false;
    publicBoolean get_notification() {
        return_notification;
    }
    publicvoidset_notification(Boolean _notification) {
        this._notification = _notification;
    }
}
```
2) 添加新类到 AndroidManifest 文件作为 application 标签的属性：

```
<application
android:name=".Global"
        .... />
```
3) 你可通过 Context.getApplicationContext() 方法来访问到该全局变量：


```
Global global;
    publicvoidonCreate(Bundle savedInstanceState) {
        global=((Global)getApplicationContext());
        Boolean notification=global.get_notification();}
```