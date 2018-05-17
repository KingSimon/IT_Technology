# ADOBE AIR移动APP的互相调用实现方式
使用Adobe AIR进行移动应用开发的时候，我们或许会有这样的需求：假如我们开发的是两个应用（A和B），同时安装到手机上，那么能否在A中呼叫B并传递参数呢（或者反过来B操作后，再把参数返回给A）。
目前AIR还没有直接呼叫某个APP的API，但我们可以使用手机特有的特性来实现。
## 第一种方式：使用ANE
假如我们的目标平台是Android，那么可以确定的是，Java是可以呼叫一个APP并传递参数的，所以我们可以借助Java实现的ANE来调用另一个APP（只要知道另一个APP的ID就可以了）。
首先打开Eclipse，创建一个Android项目，引入FlashRuntimeExtensions.jar，编写Extension,Context和Function（具体过程不再细述了，可以[参阅创建ANE的初级教程](http://sswilliam.blog.163.com/blog/static/189696383201191094227313/)），也可以在稍后的链接中下载Java部分的代码。主要是实现callApp这个Function，代码如下：
```java
package com.techmx.extensions;
import android.app.Activity;
import android.content.Intent;
import android.net.Uri;
import com.adobe.fre.FREContext;
import com.adobe.fre.FREFunction;
import com.adobe.fre.FREObject;
 
public class CallAppFunction implements FREFunction {
 
    @Override
    public FREObject call(FREContext arg0, FREObject[] arg1) {
        FREObject result = null;
        String appPackage;
        String appID;
        Intent myIntent = new Intent();
        try {
            Activity currentActivity = arg0.getActivity();
            appPackage = arg1[0].getAsString();
            appID = arg1[1].getAsString();
            myIntent.addCategory(Intent.CATEGORY_LAUNCHER);
            myIntent.setAction(Intent.ACTION_MAIN);
            myIntent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            myIntent.setData(Uri.parse(arg1[2].getAsString()));
            myIntent.setClassName(appPackage, appID);
            currentActivity.startActivity(myIntent);
        } catch (Exception e) {
            // TODO: handle exception
        }
        return result;
    }
}
```
然后打开Flash Builder，创建一个库项目，和Java部分的接口相对应：
```
package com.techmx.extensions
{
    import flash.events.EventDispatcher;
    import flash.events.IEventDispatcher;
    import flash.external.ExtensionContext;
 
    /**
     * 通过一个传递的ID，启动另外一个应用
     */
    public class CallAppExtension extends EventDispatcher
    {
        public static const CALL_APP:String = "callApp";
        public static const EXTENSION_ID:String = "com.techmx.extensions.CallAppExtension";
        private var extContext:ExtensionContext;
 
        public function CallAppExtension(target:IEventDispatcher=null)
        {
            extContext = ExtensionContext.createExtensionContext(EXTENSION_ID,"");
        }
 
        public function callApp(appPackage:String,appID:String,customURI:String):void
        {
            if(extContext)
            {
                extContext.call(CALL_APP,appPackage,appID,customURI);
            }
        }
    }
}
```
然后就可以拿出SWC，和Java项目导出的JAR一起，打包为ANE文件。整个项目工程(包括ANE文件)可以点击这里下载: [ANEPack2](http://www.todoair.com/wp-content/uploads/2012/09/ANEPack2.zip)
然后我们就可以创建两个测试项目：MobileA和MobileB，类型都是ActionScript手机项目。在MobileA中，引入刚才创建的ANE文件，并调用扩展的方法来呼叫另一个应用，也就是MobileB。
MobileA的主体代码：

```java
package
{
    import com.techmx.extensions.CallAppExtension;
 
    import flash.display.Sprite;
    import flash.events.MouseEvent;
    import flash.net.*;
 
    public class MobileA extends Sprite
    {
        public function MobileA()
        {
            super();
            var btn:Sprite = new Sprite();
            btn.graphics.beginFill(0x000000,1);
            btn.graphics.drawCircle(50,50,50);
            btn.graphics.endFill();
            addChild(btn);
            btn.addEventListener(MouseEvent.CLICK,clickHandler);
        }
 
        protected function clickHandler(event:MouseEvent):void
        {
            var extension:CallAppExtension = new CallAppExtension();
            //注意ID的规则，AIR会自动补上"air."和".AppEntry"
            extension.callApp("air.MobileB","air.MobileB.AppEntry","myarguments://me=neo&you=jack");
        }
    }
}
```
注意扩展方法的第三个参数，我们可以用自定义URI的方式传递参数，这里写的是myarguments，实际上你写成其它的值也没有关系，都可以传递过去。

在MobileB中，则可以生成一个文本对象，来显示收到的参数：
```java
package
{
    import flash.desktop.NativeApplication;
    import flash.display.Sprite;
    import flash.events.InvokeEvent;
    import flash.text.TextField;
    import flash.text.TextFormat;
 
    [SWF(width="320",height="480")]
    public class MobileB extends Sprite
    {
        private var label:TextField;
 
        public function MobileB()
        {
            super();
            label = new TextField();
            var format:TextFormat = new TextFormat(null,28);
            label.width = 320;
            label.height = 480;
            label.defaultTextFormat = format;
            addChild(label);
            NativeApplication.nativeApplication.addEventListener(InvokeEvent.INVOKE, onInvoke);
        }
 
        private function onInvoke(event:InvokeEvent):void
        {
            label.text = "Reason: "+event.reason;
            label.appendText("\n"+"Arguments: " + event.arguments);
        }
    }
}
```
将两个应用全部安装到手机上，然后打开MobileA，就可以点击黑色的圆圈呼叫MobileB。

## 第二种方式：使用自定义URI
使用ANE的方式，可以最大程度的使用底层API的便利，但对于项目还是不太方便。如果只是呼叫另一个APP的话，所幸我们还有另一个选择，就是使用自定义的URI。

这种方式就要求我们对那个需要被调用的应用（在这个例子中就是MobileB），在配置文件中注册一个自定义的URI，比如我们要注册一个“todoair”的URI，需要在配置文件中同时更改Android部分的定义和iOS部分的定义（如果您还要部署到iOS的话）。
```xml
<android>
        <manifestAdditions><![CDATA[
            <manifest android:installLocation="auto">
                <application>
                 <activity>
                     <intent-filter>
                           <action android:name="android.intent.action.MAIN"/>
                           <category android:name="android.intent.category.LAUNCHER"/>
                     </intent-filter>
                     <intent-filter>
                           <action android:name="android.intent.action.VIEW"/>
                           <category android:name="android.intent.category.BROWSABLE"/>
                           <category android:name="android.intent.category.DEFAULT"/>
                           <data android:scheme="todoair"/>
                     </intent-filter>
                 </activity>
              </application>
              <uses-permission android:name="android.permission.INTERNET"/>
            </manifest>
 
        ]]></manifestAdditions>
    </android>
    <iPhone>
        <InfoAdditions><![CDATA[
            <key>UIDeviceFamily</key>
            <array>
                <string>1</string>
                <string>2</string>
            </array>
            <key>CFBundleURLTypes</key>
            <array>
                <dict>
                    <key>CFBundleURLSchemes</key>
                    <array>
                        <string>todoair</string>
                    </array>
                    <key>CFBundleURLName</key>
                    <string>MobileB</string>
                </dict>
            </array>
        ]]></InfoAdditions>
        <requestedDisplayResolution>high</requestedDisplayResolution>
    </iPhone>
```
然后先把MobileB打包安装到手机，接着来修改MobileA的代码，将调用方式修改为：
```java
protected function clickHandler(event:MouseEvent):void
{
    //URL方式
    //navigateToURL(new URLRequest("todoair://me=neo&you=jack"));
}
```
这就可以了，打包MobileA到手机，运行效果和刚才基于ANE的方式是类似的：
这个方式不需要编写原生代码来配合，使用简单，但是一定要在配置文件中注册URI才会生效。另外您应该也注意到了，它使用navigateToURL来调用，说明这个方式用网页也是可行的。您可以在您的WEB网站上，用HTML中的A链接，结合注册的自定义URI，来启动您的应用。
参考资源：

[InvokeEvent的说明](http://www.todoair.com/adobe-air%E7%A7%BB%E5%8A%A8app%E7%9A%84%E4%BA%92%E7%9B%B8%E8%B0%83%E7%94%A8%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F-2012-09-28/help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/events/InvokeEvent.html)

[自定义URI的相关说明](http://www.flex-tutorial.fr/2010/09/11/air-android-ouvrir-une-application-air-sur-android-depuis-le-navigateur-web-avec-une-custom-uri/)

[Adobe AIR 3.5 SDK下载](http://labs.adobe.com/downloads/air3-5.html)

[Adobe AIR 3.5更新日志](http://labsdownload.adobe.com/pub/labs/flashplatformruntimes/shared/air3-5_flashplayer11-5_releasenotes.pdf)