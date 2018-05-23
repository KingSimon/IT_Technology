# Android 获取ip地址
获取ip地址
1.使用WIFI
首先设置用户权限
```
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE"></uses-permission>
<uses-permission android:name="android.permission.WAKE_LOCK"></uses-permission>
```
```
//获取wifi服务
        WifiManager wifiManager = (WifiManager) getSystemService(Context.WIFI_SERVICE);
        //判断wifi是否开启
        if (!wifiManager.isWifiEnabled()) {
            wifiManager.setWifiEnabled(true);
        }
        WifiInfo wifiInfo = wifiManager.getConnectionInfo();
        int ipAddress = wifiInfo.getIpAddress();
        String ip = intToIp(ipAddress);
        TextView tv=(TextView) findViewById(R.id.ip);
        tv.setText("本机ip:"+ip);

private String intToIp(int i) {
        return (i & 0xFF ) + "." +
                ((i >> 8 ) & 0xFF) + "." +
                ((i >> 16 ) & 0xFF) + "." +
                ( i >> 24 & 0xFF) ;
    }
```
2.使用GPRS
```
//首先，设置用户上网权限
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
public String getLocalIpAddress()    {
        try    {
            for (Enumeration<NetworkInterface> en = NetworkInterface.getNetworkInterfaces();
                    en.hasMoreElements();)            {
                NetworkInterface intf = en.nextElement();
                for (Enumeration<InetAddress> enumIpAddr = intf.getInetAddresses(); enumIpAddr.hasMoreElements();)               {
                    InetAddress inetAddress = enumIpAddr.nextElement();
                    if (!inetAddress.isLoopbackAddress())                   {
                        return inetAddress.getHostAddress().toString();
                    }
                }
            }
        }        catch (SocketException ex)        {
            Log.e("WifiPreference IpAddress", ex.toString());        }
        return null;
    }
```