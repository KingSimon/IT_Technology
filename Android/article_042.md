# 判断当前网络是否可用和调用系统设置wifi界面
需要用到的权限

```
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.INTERNET"/>
```
```
//判断当前网络是否可用
    public boolean note_Intent(Context context) {
        ConnectivityManager con = (ConnectivityManager) context
            .getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo networkinfo = con.getActiveNetworkInfo();
        if (networkinfo == null || !networkinfo.isAvailable()) {
        // 当前网络不可用
            Toast.makeText(context.getApplicationContext(), "当前网络不可用",
            Toast.LENGTH_SHORT).show();
            return false;
        }
        boolean wifi = con.getNetworkInfo(ConnectivityManager.TYPE_WIFI)
            .isConnectedOrConnecting();
        if (!wifi) { // 提示使用wifi
            Toast.makeText(context.getApplicationContext(), "建议您使用WIFI以减少流量！",
            Toast.LENGTH_SHORT).show();
        }
        return true;

    }
```
跳转到系统设置WIFI界面中

```
Intent localIntent = new Intent();
        localIntent.setComponent(new ComponentName("com.android.settings",
                "com.android.settings.wifi.WifiPickerActivity"));
        startActivityForResult(localIntent, 100);
```