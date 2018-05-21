# Intent在Android中的几种用法
如果是从BroadcastReceiver 启动一个新的Activity , 不要忘记`i.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK)``;

```
public class MyReceiver extends BroadcastReceiver{

public static final String action="acc";
 public void onReceive(Context context, Intent intent) {
  Intent i=new Intent(context,Receivered.class);
  i.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
  context.startActivity(i);
 }


}
```
1. 指定action 和type
```
// SIM import
        Intent importIntent = new Intent(Intent.ACTION_VIEW);
        importIntent.setType("vnd.android.cursor.item/sim-contact");
        importIntent.setClassName("com.android.phone", "com.android.phone.SimContacts");
        menu.add(0, 0, 0, R.string.importFromSim)
                .setIcon(R.drawable.ic_menu_import_contact)
                .setIntent(importIntent);
```

2. 指定action, data和type
(1)隐式查找type
示例代码：
```
uri: content://simcontacts/simPeople/(id)
intent = new Intent("android.intent.action.SIMEDIT",uri);
            startActivity(intent);
```
程序会很据data中的uri去查找匹配的type（必须的）
provider中的getType()
```
case SIM_PEOPLE_ID:
            return "vnd.android.cursor.item/sim-contact";
```
配置文件中的filter设定
```
AndroidManifest.xml
    <intent-filter>
                <action android:name="android.intent.action.SIMEDIT" />
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="vnd.android.cursor.item/sim-contact" />
      </intent-filter>
```
也可以自己设定type，但只能使用 setDataAndType()

3. 其他设定intent的属性方式
```
   Intent setComponent(ComponentName component)
   Intent setClassName(Context packageContext, String className)
   Intent setClassName(String packageName, String className)
   Intent setClass(Context packageContext, Class<?> cls)
```

Intent 应该算是Android中特有的东西。你可以在Intent中指定程序 要执行的动作（比如：view,edit,dial），以及程序执行到该动作时所需要的资料 。都指定好后，只要调用startActivity()，Android系统 会自动寻找最符合你指定要求的应用 程序，并执行该程序。

下面列出几种Intent 的用法
显示网页:
```
Uri uri = Uri.parse("http://www.google.com");
Intent it  = new Intent(Intent.ACTION_VIEW,uri);
startActivity(it);
```
显示地图:
```
Uri uri = Uri.parse("geo:38.899533,-77.036476");
Intent it = new Intent(Intent.Action_VIEW,uri);
startActivity(it);
```
路径规划:
```
Uri uri = Uri.parse("http://maps.google.com/maps?f=dsaddr=startLat%20startLng&daddr=endLat%20endLng&hl=en");
Intent it = new Intent(Intent.ACTION_VIEW,URI);
startActivity(it);
```
拨打电话:
调用拨号程序
```
Uri uri = Uri.parse("tel:xxxxxx");
Intent it = new Intent(Intent.ACTION_DIAL, uri);
startActivity(it);
Uri uri = Uri.parse("tel.xxxxxx");
Intent it =new Intent(Intent.ACTION_CALL,uri);
```
要使用这个必须在配置文件 中加入`<uses-permission id="android .permission.CALL_PHONE" />``
发送SMS/MMS
调用发送短信 的程序
```
Intent it = new Intent(Intent.ACTION_VIEW);
it.putExtra("sms_body", "The SMS text");
it.setType("vnd.android-dir/mms-sms");
startActivity(it);
```
发送短信
```
Uri uri = Uri.parse("smsto:0800000123");
Intent it = new Intent(Intent.ACTION_SENDTO, uri);
it.putExtra("sms_body", "The SMS text");
startActivity(it);
```
发送彩信
```
Uri uri = Uri.parse("content://media/external/images/media/23");
Intent it = new Intent(Intent.ACTION_SEND);
it.putExtra("sms_body", "some text");
it.putExtra(Intent.EXTRA_STREAM, uri);
it.setType("image/png");
startActivity(it);
```
发送Email
```
Uri uri = Uri.parse("mailto:xxx@abc.com");
Intent it = new Intent(Intent.ACTION_SENDTO, uri);
startActivity(it);
Intent it = new Intent(Intent.ACTION_SEND);
it.putExtra(Intent.EXTRA_EMAIL, "me@abc.com");
it.putExtra(Intent.EXTRA_TEXT, "The email body text");
it.setType("text/plain");
startActivity(Intent.createChooser(it, "Choose Email Client"));
Intent it=new Intent(Intent.ACTION_SEND);
String[] tos={"me@abc.com"};
String[] ccs={"you@abc.com"};
it.putExtra(Intent.EXTRA_EMAIL, tos);
it.putExtra(Intent.EXTRA_CC, ccs);
it.putExtra(Intent.EXTRA_TEXT, "The email body text");
it.putExtra(Intent.EXTRA_SUBJECT, "The email subject text");
it.setType("message/rfc822");
startActivity(Intent.createChooser(it, "Choose Email Client"));
```
添加附件
```
Intent it = new Intent(Intent.ACTION_SEND);
it.putExtra(Intent.EXTRA_SUBJECT, "The email subject text");
it.putExtra(Intent.EXTRA_STREAM, "file:///sdcard/mysong.mp3");
sendIntent.setType("audio/mp3");
startActivity(Intent.createChooser(it, "Choose Email Client"));
```
播放多媒体

```
Intent it = new Intent(Intent.ACTION_VIEW);
Uri uri = Uri.parse("file:///sdcard/song.mp3");
it.setDataAndType(uri, "audio/mp3");
startActivity(it);
Uri uri = Uri.withAppendedPath(MediaStore.Audio.Media.INTERNAL_CONTENT_URI, "1");
Intent it = new Intent(Intent.ACTION_VIEW, uri);
startActivity(it);
```
Uninstall 程序
```
Uri uri = Uri.fromParts("package", strPackageName, null);
Intent it = new Intent(Intent.ACTION_DELETE, uri);
startActivity(it);
```
uninstall apk
```
Uri uninstallUri = Uri.fromParts("package", "xxx", null);
returnIt = new Intent(Intent.ACTION_DELETE, uninstallUri);
```
install apk
```
Uri installUri = Uri.fromParts("package", "xxx", null);
returnIt = new Intent(Intent.ACTION_PACKAGE_ADDED, installUri);
```
play audio
```
Uri playUri = Uri.parse("file:///sdcard/download/everything.mp3");
returnIt = new Intent(Intent.ACTION_VIEW, playUri);
```
//发送附件
```
Intent it = new Intent(Intent.ACTION_SEND);
it.putExtra(Intent.EXTRA_SUBJECT, "The email subject text");
it.putExtra(Intent.EXTRA_STREAM, "file:///sdcard/eoe.mp3");
sendIntent.setType("audio/mp3");
startActivity(Intent.createChooser(it, "Choose Email Client"));
```
//搜索应用
```
Uri uri = Uri.parse("market://search?q=pname:pkg_name");
Intent it = new Intent(Intent.ACTION_VIEW, uri);
startActivity(it);
//where pkg_name is the full package path for an application
```
//显示指定应用的详细页面（这个好像不支持了，找不到app_id）
```
Uri uri = Uri.parse("market://details?id=app_id");
Intent it = new Intent(Intent.ACTION_VIEW, uri);
startActivity(it);
//where app_id is the application ID, find the ID
//by clicking on your application on Market home
//page, and notice the ID from the address bar
```