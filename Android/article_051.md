# Android开发之编程中15个很有用的代码片段
## 1：查看是否有存储卡插入

```
String status=Environment.getExternalStorageState();
if(status.equals(Enviroment.MEDIA_MOUNTED))
{
说明有SD卡插入
}
```

## 2：让某个Activity透明
OnCreate中不设Layout

```
this.setTheme(R.style.Theme_Transparent);
```
以下是Theme_Transparent的定义（注意transparent_bg是一副透明的图片）

## 3：在屏幕元素中设置句柄
使用Activity.findViewById来取得屏幕上的元素的句柄. 使用该句柄您可以设置或获取任何该对象外露的值.

```
TextView msgTextView = (TextView)findViewById(R.id.msg);
msgTextView.setText(R.string.push_me);
```

## 4:发送短信

```
String body=”this is mms demo”;
Intent mmsintent = new Intent(Intent.ACTION_SENDTO, Uri.fromParts(”smsto”, number, null));
mmsintent.putExtra(Messaging.KEY_ACTION_SENDTO_MESSAGE_BODY, body);
mmsintent.putExtra(Messaging.KEY_ACTION_SENDTO_COMPOSE_MODE, true);
mmsintent.putExtra(Messaging.KEY_ACTION_SENDTO_EXIT_ON_SENT, true);
startActivity(mmsintent);
```
## 5:发送彩信

```
StringBuilder sb = new StringBuilder();
sb.append(”file://”);
sb.append(fd.getAbsoluteFile());
Intent intent = new Intent(Intent.ACTION_SENDTO, Uri.fromParts(”mmsto”, number, null));
// Below extra datas are all optional.
intent.putExtra(Messaging.KEY_ACTION_SENDTO_MESSAGE_SUBJECT, subject);
intent.putExtra(Messaging.KEY_ACTION_SENDTO_MESSAGE_BODY, body);
intent.putExtra(Messaging.KEY_ACTION_SENDTO_CONTENT_URI, sb.toString());
intent.putExtra(Messaging.KEY_ACTION_SENDTO_COMPOSE_MODE, composeMode);
intent.putExtra(Messaging.KEY_ACTION_SENDTO_EXIT_ON_SENT, exitOnSent);
startActivity(intent);
```
## 7：发送Mail

```
mime = “img/jpg”;
shareIntent.setDataAndType(Uri.fromFile(fd), mime);
shareIntent.putExtra(Intent.EXTRA_STREAM, Uri.fromFile(fd));
shareIntent.putExtra(Intent.EXTRA_SUBJECT, subject);
shareIntent.putExtra(Intent.EXTRA_TEXT, body);
```
## 8:注册一个BroadcastReceiver

```
registerReceiver(mMasterResetReciever, new IntentFilter(”OMS.action.MASTERRESET”));
private BroadcastReceiver mMasterResetReciever = new BroadcastReceiver() {
public void onReceive(Context context, Intent intent){
String action = intent.getAction();
if(”oms.action.MASTERRESET”.equals(action)){
RecoverDefaultConfig();
}
}

};
```
## 9:定义ContentObserver，监听某个数据表

```
private ContentObserver mDownloadsObserver = new DownloadsChangeObserver(Downloads.CONTENT_URI);
private class DownloadsChangeObserver extends ContentObserver {
public DownloadsChangeObserver(Uri uri) {
super(new Handler());
}
@Override
public void onChange(boolean selfChange) {｝
}
```
## 10:获得 手机UA

```
public String getUserAgent()
{
String user_agent = ProductProperties.get(ProductProperties.USER_AGENT_KEY, null);
return user_agent;
}
```
## 11：清空手机上cookie

```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager.getInstance().removeAllCookie();
```
## 12：建立GPRS连接

```
//Dial the GPRS link.
private boolean openDataConnection() {
// Set up data connection.
DataConnection conn = DataConnection.getInstance();

if (connectMode == 0) {
ret = conn.openConnection(mContext, “cmwap”, “cmwap”, “cmwap”);
} else {
ret = conn.openConnection(mContext, “cmnet”, “”, “”);
}

}
```
## 13：PreferenceActivity 用法

```
public class Setting extends PreferenceActivity

｛

public void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
addPreferencesFromResource(R.xml.settings)；
}

｝
```
Setting.xml:

```
Android:key=”seting2″
android:title=”@string/seting2″
android:summary=”@string/seting2″/>

android:key=”seting1″
android:title=”@string/seting1″
android:summaryOff=”@string/seting1summaryOff”
android:summaryOn=”@stringseting1summaryOff”/>
```
## 14:通过HttpClient从指定server获取数据

```
DefaultHttpClient httpClient = new DefaultHttpClient();
HttpGet method = new HttpGet(“http://www.baidu.com/1.html”);
HttpResponse resp;
Reader reader = null;
try {
// AllClientPNames.TIMEOUT
HttpParams params = new BasicHttpParams();
params.setIntParameter(AllClientPNames.CONNECTION_TIMEOUT, 10000);
httpClient.setParams(params);
resp = httpClient.execute(method);
int status = resp.getStatusLine().getStatusCode();
if (status != HttpStatus.SC_OK) return false;
// HttpStatus.SC_OK;
return true;
} catch (ClientProtocolException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (IOException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} finally {
if (reader != null) try {
reader.close();
} catch (IOException e) {
// TODO Auto-generated catch block
e.printStackTrace();
}
}
```
## 15:显示toast

```
Toast.makeText(this._getApplicationContext(), R.string._item, Toast.LENGTH_SHORT).show();
```