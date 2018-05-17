# 实现AIR FOR ANDROID应用的随系统自启动
如果您是一位Java Android开发人员，那么实现一个随系统自启动的应用对您来说应该非常Easy，但对于一位使用Adobe Flash技术开发应用，然后用AIR打包机制制作.APK的开发者来说，实现这个功能却不是很轻松的。Flash的优势就是跨平台，一位Flash开发者可以使用自身积累的知识体系，以最小的学习成本进入Android开发的世界。AIR在打包的时候对我们隐藏了很多细节，这样一方面可以减少我们的学习阻力，一方面却也因为这个不透明的过程造成一些困扰（后面详述）。如果我们要实现一个功能，AIR核心API却没有提供实现，就成了非常麻烦的事情，不过现在好在AIR已经提供了一种扩展自己功能的机制，就是ANE。对于Android开发来说，我们可以使用Java代码来完成AIR本身不提供的功能。
关于ANE的基本知识，您可以参阅这里（中文）：http://help.adobe.com/zh_CN/air/extensions/index.html
很棒的ANE for Android实例教程 http://t.cn/SbsI5j 跟这个过一遍就明白ANE的原理，创建过程和使用方式了。
下面我们来看看如何让一个AIR打包的APK实现随系统自己启动的功能（当然也要借助ANE了）。

## APK的AndroidManifest.xml分析
在动手之前，您最好先把AIR打包产生的APK文件做一下分析，了解它的特性，后面就可以少走一些弯路。将.apk文件直接改扩展名为.zip，解压即可看到它的结构。注意AndroidManifest.xml，这是Android应用非常核心的一个配置文件。这个文件是AIR打包自动产生的，但是和AIR应用本身的XML配置文件也是有管理的（AIR应用的XML配置中的android节点部分会被合并到AndroidManifest.xml，这样方便我们做一些权限设定等等）。
解压得到的AndroidManifest.xml是个二进制的XML文档，无法用文本工具查看，您可以先使用AXMLPrinter2.jar将它转换为普通文本格式即可阅读。
这个文件中我们要注意几个细节：
1. manifest节点的package属性不能由我们设定，这是AIR打包的时候自动设定的，规则是“air.应用ID”，比如我们的应用ID是TestAppANEs，那么这里的设置就是package=”air.TestAppANEs”
2. 在application部分会自动产生一个activity，名称是.AppEntry。activity相当于Android应用的视图，AIR会自动产生一个视图，用来承载我们的Flash内容。
了解这些细节之后，我们就可以继续实施ANE部分的开发了。

## ANE实现
创建ANE项目的过程就不细述了，您可以参阅Adobe的文档。这里只说和随系统启动相关的部分。您首先要创建一个包，命名和manifest节点的package属性保持一致，比如这里应该是air.TestAppANEs。这个地方要非常注意，包名必须遵循这样的结构，否则运行时会找不到类。
然后在这个包中创建一个Java类：BootBroadCastReceiver，继承BroadcastReceiver，完整代码如下：

```java
package air.TestAppANEs;
import android.app.Activity;
import android.app.AlarmManager;
import android.app.PendingIntent;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.net.NetworkInfo.State;
import android.os.SystemClock;
import android.util.Log;
public class BootBroadCastReceiver extends BroadcastReceiver {
    public static final String ACTION = "android.intent.action.BOOT_COMPLETED";
    private PendingIntent mAlarmSender;
    @Override
    public void onReceive(Context context, Intent intent) {
        if (intent.getAction().equals(ACTION)) {
            Intent it =new Intent();
            it.setClass("air.TestAppANEs","air.TestAppANEs.AppEntry");
            it.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            context.startActivity(it);
        }
    }
}
```
然后将Java项目编译为JAR包，然后建立一个ActionScript库项目，最终和JAR包打包为一个ANE文件（略过N多细节，请参阅Adobe文档）。
这里再补充两个细节问题，首先是ADT打包，ANE打包的参数确实很容易弄错，估计第一次打包的同学很难能一次性通过，最后一个参数的点前面还有一个空格，提醒您千万注意了 -platform Android-ARM -C .\Android-ARM\ .
其次是您应该给extension.xml设置一个
，并使用ActionScript实现一个模拟功能实施，并打包到ANE中，这样方便您在PC测试，否则您会得到不支持调试的提示。


## 和主项目的整合
ANE制作完毕后，您可以用Flash Builder，在您的主项目上点击右键，属性，库构建路径，在ANE面板上，加入刚才制作的ANE文件（Flash Builder会自动在AIR应用的XML配置文件中加入这个ANE的ID，确保这个ID必须有）。然后在发布的时候，ANE的部分还有一个对勾（确定是否包含），一定记得点上，不然就会找不到类。

先别急着打包，我们还需要修改一下配置文件，打开AIR应用的XML配置文件，找到android部分，加入.BootBroadCastReceiver的定义，完整结构如下：
```xml
<android>
    <colorDepth>16bit</colorDepth>
    <manifestAdditions><![CDATA[
        <manifest android:installLocation="auto">
            <application>
                <receiver android:name=".BootBroadCastReceiver" >
                    <intent-filter>
                        <action android:name="android.intent.action.BOOT_COMPLETED" />
                    </intent-filter>
                </receiver>
            </application>
            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
            <uses-permission android:name="android.permission.INTERNET"/>
            <!--其它权限-->
        </manifest>
    ]]></manifestAdditions>
</android>
```
注意.BootBroadCastReceiver这个定义很关键，以.开头才能实现随系统启动的功能。

然后…就没有然后了。您可以测试您的应用，安装后让手机重启，不出意外的话，您可以看到自己的应用在系统启动完毕后，就会自己启动并显示主界面。

您也可以不显示主界面，而是注册一个Service，实现后台的通知和提醒。
