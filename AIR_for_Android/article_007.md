# Mobile Flex 调用GPS
利用flash air技术可跨平台实现GPS调用功能，可查看实时经纬度信息。（基于flex4.6开发）

Mxml文件代码如下：
```
<?xml version="1.0" encoding="GBK"?>
<s:Application xmlns:fx="http://ns.adobe.com/mxml/2009" 
   xmlns:s="library://ns.adobe.com/flex/spark" applicationDPI="240" width="480" height="800" initialize="GetGeoLocation()">
<fx:Declarations>
<!-- 将非可视元素（例如服务、值对象）放在此处 -->
</fx:Declarations>
<fx:Script>
<![CDATA[
import flash.events.GeolocationEvent;
import flash.sensors.Geolocation;
import com.yapiodesign.components.android.alert.AlertDialog;
private var alert:AlertDialog;

private function GetGeoLocation():void{
if (Geolocation.isSupported){
//Initialize the location sensor.
var geo:Geolocation = new Geolocation();
if(!geo.muted){
geo.setRequestedUpdateInterval(6000);
//Register to receive location updates.
geo.addEventListener(GeolocationEvent.UPDATE, geolocationUpdateHandler);
}
}else{
//Alert.show("未检测到GPS设备！","提示");
alert = new AlertDialog();
alert.title = "提示";
alert.message = "未检测到GPS设备！";
alert.open(this, true);
}
}


private function geolocationUpdateHandler(event:GeolocationEvent):void{
LabLatitude.text = event.latitude.toString();
LabLongitude.text = event.longitude.toString();
LabAltitude.text = event.altitude.toString();
LabSpeed.text = event.speed.toString();
}


private function Close():void{
NativeApplication.nativeApplication.exit();
}

]]>

</fx:Script>

<!--<android>
<manifestAdditions>
<![CDATA[<manifest>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>]]>
</manifestAdditions>
</android>
-->

<s:Label x="59" y="63" text="经度" width="58" height="22"/>
<s:Label x="58" y="139" text="纬度" width="52" height="22"/>
<s:Label x="55" y="215" text="高程" width="58" height="22"/>
<s:Label x="58" y="297" text="速度" width="67" height="22"/>
<s:Label x="148" y="63" text="0.0" width="195" height="22" id="LabLongitude"/>
<s:Label x="149" y="139" text="0.0" width="195" height="22" id="LabLatitude"/>
<s:Label x="149" y="215" text="0.0" width="195" height="22" id="LabAltitude"/>
<s:Label x="150" y="297" text="0.0" width="195" height="22" id="LabSpeed"/>
<s:Button x="133" y="397" width="152" label="退出" click="Close()"/>

</s:Application>
```
App配置xml文件内容如下
```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<application xmlns="http://ns.adobe.com/air/application/3.1">

<!-- Adobe AIR Application Descriptor File Template.

Specifies parameters for identifying, installing, and launching AIR applications.

xmlns - The Adobe AIR namespace: http://ns.adobe.com/air/application/3.1
The last segment of the namespace specifies the version 
of the AIR runtime required for this application to run.

minimumPatchLevel - The minimum patch level of the AIR runtime required to run 
the application. Optional.
-->

<!-- A universally unique application identifier. Must be unique across all AIR applications.
Using a reverse DNS-style name as the id is recommended. (Eg. com.example.ExampleApplication.) Required. -->
<id>GPS1</id>

<!-- Used as the filename for the application. Required. -->
<filename>GPS1</filename>

<!-- The name that is displayed in the AIR application installer. 
May have multiple values for each language. See samples or xsd schema file. Optional. -->
<name>GPS1</name>

<!-- A string value of the format <0-999>.<0-999>.<0-999> that represents application version which can be used to check for application upgrade. 
Values can also be 1-part or 2-part. It is not necessary to have a 3-part value.

An updated version of application must have a versionNumber value higher than the previous version. Required for namespace >= 2.5 . -->

<versionNumber>0.0.0</versionNumber>

<!-- A string value (such as "v1", "2.5", or "Alpha 1") that represents the version of the application, as it should be shown to users. Optional. -->

<!-- <versionLabel></versionLabel> -->

<!-- Description, displayed in the AIR application installer.

May have multiple values for each language. See samples or xsd schema file. Optional. -->

<!-- <description></description> -->
<!-- Copyright information. Optional -->
<!-- <copyright></copyright> -->
<!-- Publisher ID. Used if you're updating an application created prior to 1.5.3 -->
<!-- <publisherID></publisherID> -->
<!-- Settings for the application's initial window. Required. -->

<initialWindow>
<!-- The main SWF or HTML file of the application. Required. -->
<!-- Note: In Flash Builder, the SWF reference is set automatically. -->
<content>[此值将由 Flash Builder 在输出 app.xml 中覆盖]</content>

<!-- The title of the main window. Optional. -->
<!-- <title></title> -->
<!-- The type of system chrome to use (either "standard" or "none"). Optional. Default standard. -->
<!-- <systemChrome></systemChrome> -->
<!-- Whether the window is transparent. Only applicable when systemChrome is none. Optional. Default false. -->
<!-- <transparent></transparent> -->
<!-- Whether the window is initially visible. Optional. Default false. -->
<!-- <visible></visible> -->

<!-- Whether the user can minimize the window. Optional. Default true. -->
<!-- <minimizable></minimizable> -->
<!-- Whether the user can maximize the window. Optional. Default true. -->
<!-- <maximizable></maximizable> -->
<!-- Whether the user can resize the window. Optional. Default true. -->
<!-- <resizable></resizable> -->

<!-- The window's initial width in pixels. Optional. -->
<!-- <width></width> -->
<!-- The window's initial height in pixels. Optional. -->
<!-- <height></height> -->
<!-- The window's initial x position. Optional. -->
<!-- <x></x> -->

<!-- The window's initial y position. Optional. -->
<!-- <y></y> -->

<!-- The window's minimum size, specified as a width/height pair in pixels, such as "400 200". Optional. -->
<!-- <minSize></minSize> -->

<!-- The window's initial maximum size, specified as a width/height pair in pixels, such as "1600 1200". Optional. -->
<!-- <maxSize></maxSize> -->

        <!-- The initial aspect ratio of the app when launched (either "portrait" or "landscape"). Optional. Mobile only. Default is the natural orientation of the device -->

        <!-- <aspectRatio></aspectRatio> -->
        <!-- Whether the app will begin auto-orienting on launch. Optional. Mobile only. Default false -->
        <!-- <autoOrients></autoOrients> -->
        <!-- Whether the app launches in full screen. Optional. Mobile only. Default false -->

        <!-- <fullScreen></fullScreen> -->
        <!-- The render mode for the app (either auto, cpu, gpu, or direct). Optional. Default auto -->
        <!-- <renderMode></renderMode> -->


<!-- Whether or not to pan when a soft keyboard is raised or lowered (either "pan" or "none").  Optional.  Defaults "pan." -->

<!-- <softKeyboardBehavior></softKeyboardBehavior> -->
<autoOrients>true</autoOrients>
        <fullScreen>false</fullScreen>
        <visible>false</visible>
        <softKeyboardBehavior>none</softKeyboardBehavior>
    </initialWindow>

<!-- We recommend omitting the supportedProfiles element, -->
<!-- which in turn permits your application to be deployed to all -->
<!-- devices supported by AIR. If you wish to restrict deployment -->
<!-- (i.e., to only mobile devices) then add this element and list -->
<!-- only the profiles which your application does support. -->
<!-- <supportedProfiles>desktop extendedDesktop mobileDevice extendedMobileDevice</supportedProfiles> -->

<!-- The subpath of the standard default installation location to use. Optional. -->
<!-- <installFolder></installFolder> -->

<!-- The subpath of the Programs menu to use. (Ignored on operating systems without a Programs menu.) Optional. -->
<!-- <programMenuFolder></programMenuFolder> -->

<!-- The icon the system uses for the application. For at least one resolution,
specify the path to a PNG file included in the AIR package. Optional. -->
<!-- <icon>
<image16x16></image16x16>
<image32x32></image32x32>
<image36x36></image36x36>
<image48x48></image48x48>
<image57x57></image57x57>
<image72x72></image72x72>
<image114x114></image114x114>
<image128x128></image128x128>
</icon> -->


<!-- Whether the application handles the update when a user double-clicks an update version
of the AIR file (true), or the default AIR application installer handles the update (false).
Optional. Default false. -->
<!-- <customUpdateUI></customUpdateUI> -->

<!-- Whether the application can be launched when the user clicks a link in a web browser.
Optional. Default false. -->

<!-- <allowBrowserInvocation></allowBrowserInvocation> -->


<!-- Listing of file types for which the application can register. Optional. -->
<!-- <fileTypes> -->


<!-- Defines one file type. Optional. -->
<!-- <fileType> -->


<!-- The name that the system displays for the registered file type. Required. -->
<!-- <name></name> -->


<!-- The extension to register. Required. -->
<!-- <extension></extension> -->


<!-- The description of the file type. Optional. -->
<!-- <description></description> -->


<!-- The MIME content type. -->
<!-- <contentType></contentType> -->


<!-- The icon to display for the file type. Optional. -->
<!-- <icon>
<image16x16></image16x16>
<image32x32></image32x32>
<image48x48></image48x48>
<image128x128></image128x128>
</icon> -->


<!-- </fileType> -->
<!-- </fileTypes> -->
    <!-- iOS specific capabilities -->
<!-- <iPhone> -->
<!-- A list of plist key/value pairs to be added to the application Info.plist -->
<!-- <InfoAdditions>
            <![CDATA[
                <key>UIDeviceFamily</key>
                <array>
                    <string>1</string>
                    <string>2</string>
                </array>
                <key>UIStatusBarStyle</key>
                <string>UIStatusBarStyleBlackOpaque</string>
                <key>UIRequiresPersistentWiFi</key>
                <string>YES</string>
            ]]>

        </InfoAdditions> -->

        <!-- A list of plist key/value pairs to be added to the application Entitlements.plist -->
<!-- <Entitlements>
            <![CDATA[
                <key>keychain-access-groups</key>
                <array>
                    <string></string>
                    <string></string>
                </array>
            ]]>
        </Entitlements> -->
<!-- Display Resolution for the app (either "standard" or "high"). Optional. Default "standard" -->
<!-- <requestedDisplayResolution></requestedDisplayResolution> -->
<!-- </iPhone> -->


<!-- Specify Android specific tags that get passed to AndroidManifest.xml file. -->
    <!--<android> -->
    <!--<manifestAdditions>
<![CDATA[
<manifest android:installLocation="auto">
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-feature android:required="true" android:name="android.hardware.touchscreen.multitouch"/>
<application android:enabled="true">
<activity android:excludeFromRecents="false">
<intent-filter>
<action android:name="android.intent.action.MAIN"/>
<category android:name="android.intent.category.LAUNCHER"/>
</intent-filter>
</activity>
</application>
            </manifest>
]]>
        </manifestAdditions> -->
    <!-- Color depth for the app (either "32bit" or "16bit"). Optional. Default 16bit before namespace 3.0, 32bit after -->
        <!-- <colorDepth></colorDepth> -->
    <!-- </android> -->
<!-- End of the schema for adding the android specific tags in AndroidManifest.xml file -->

<android>
        <colorDepth>16bit</colorDepth>
        <manifestAdditions><![CDATA[
<manifest android:installLocation="auto">
    <!--See the Adobe AIR documentation for more information about setting Google Android permissions-->
    <!--删除 android.permission.INTERNET 权限将导致无法调试设备上的应用程序-->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <!--应同时切换 DISABLE_KEYGUARD 和 WAKE_LOCK 权限，才能访问 AIR
的 SystemIdleMode API-->
    <!--<uses-permission android:name="android.permission.DISABLE_KEYGUARD"/>-->
    <!--<uses-permission android:name="android.permission.WAKE_LOCK"/>-->
    <!--<uses-permission android:name="android.permission.CAMERA"/>-->
    <!--<uses-permission android:name="android.permission.RECORD_AUDIO"/>-->
    <!--应同时切换 ACCESS_NETWORK_STATE 和 ACCESS_WIFI_STATE 权限，才能使用 AIR
的 NetworkInfo API-->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
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
]]></InfoAdditions>
        <requestedDisplayResolution>high</requestedDisplayResolution>
    </iPhone>
</application>
```