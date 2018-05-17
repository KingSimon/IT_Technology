# 在Flex4.5中检测手机网络连接类型
在应用程序中，您可以访问所有的设备接口，检查它们是否处于活动状态 。你唯一需要知道的是如何寻找某个接口。在下面的代码中，你可以看到我是如何检查“WiFi”和“mobile”接口的。在我找到的基础上，我只检查它们是否处于活动状态。请记住，有些人可能只通过移动的数据链接，但没有注册的话，那么仅找到“mobile”还是不够的。
```xml
<?xml version="1.0" encoding="utf-8"?>  
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
                xmlns:s="library://ns.adobe.com/flex/spark"
                title="Connection Test"
                creationComplete="initView()">  
 
        <fx:Script>  
                <![CDATA[  
                        private function initView():void {  
                                var interfaces:Vector.<NetworkInterface> = NetworkInfo.networkInfo.findInterfaces();  
 
                                for(var i:uint = 0; i < interfaces.length; i++) {  
                                        if(interfaces[i].name.toLowerCase() == "wifi" && interfaces[i].active) {  
                                                lbl.text = "WiFi connection enabled";  
                                                break;  
                                        } else if(interfaces[i].name.toLowerCase() == "mobile" && interfaces[i].active) {  
                                                lbl.text = "Mobile data connection enabled";  
                                                break;  
                                        }  
                                }  
                        }  
                ]]>
        </fx:Script>  
        <s:Label id="lbl" horizontalCenter="0" verticalCenter="0"/>  
</s:View>
```
在这个例子中，我只是设置一个文本属性的标签，但你也可以在实际应用中使用它，比如：确定你的服务器数据的更新率。

在创建这样的应用中，有件事你不能忘记： 你必须在<applicationName>-app.xml文件中，在android这段设置适当的权限。 这部分应包含设置ACCESS_NETWORK_STATE和ACCESS_WIFI_STATE的权限，以便应用程序能正常工作。如果不设置，你将会从findInterfaces方法的返回中得到一个空的向量。
```xml
<android>  
    <manifestAdditions><![CDATA[  
        <manifest>  
                <!-- See the Adobe AIR documentation for more information about setting Google Android permissions -->  
                <uses-permission android:name="android.permission.INTERNET"/>  
                <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>  
                <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>  
        </manifest>  
]]></manifestAdditions>  
</android>
```