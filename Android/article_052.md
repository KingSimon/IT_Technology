# Android Settings
应用能够配置Android系统的各种设置，这些设置的默认值都是由frameworks中的SettingsProvider从数据库中读取的frameworks/base/packages/SettingsProvider/res/values/defaults.xml这个文件就是用来存储
```
<integer name="def_screen_off_timeout">600000</integer>设置关屏超时时间的默认值
<integer name="def_screen_brightness">102</integer> 设置亮度的默认值
<bool name="def_install_non_market_apps">false</bool>设置是否允许安装非Market应用程序的默认值
```
### 1开机图片:

　　android-logo-mask.png
　　android-logo-shine.png
　　这两个图片一个在上一个在下
　　./out/target/common/obj/JAVA_LIBRARIES/android_stubs_current_intermediates/classes/assets/images/android-logo-shine.png
　　./frameworks/base/core/res/assets/images/android-logo-shine.png
　　注意:如果源码没有make可以直接更改frameworks里的的图片就可以了
　　然后直接make否则必须全更改并且不能make只能make firmwar

### 2默认开机墙纸的位置:
　　default_wallpaper.jpg
　　./out/target/common/obj/JAVA_LIBRARIES/android_stubs_current_intermediates/classes/res/drawable/default_wallpaper.jpg
　　./frameworks/base/core/res/res/drawable/default_wallpaper.jpg
　　注意:这个设置和上面的一样这俩个都不能更改文件名
　　3更改PC机器删除硬件的文字kernel中
　　drivers/usb/gadget/f_mass_storage.c
　　fsg->vendor = "XXXXXXXXXXXXX";

### 4更改卷标:
　　bootable/recovery/etc/init.rc
　　setprop UserVolumeLabel "XXXXXXXXXXXXX"
　　直接打包

### 5修改屏幕锁:
　　(1)
　　frameworks/base/packages/SettingsProvider/res/values/defaults.xml
```
　　<integer name="def_screen_off_timeout">60000</integer>
```
　　60000改成想要的时间如果是不锁为-1
　　(2)
　　frameworks/policies/base/phone/com/android/internal/policy/impl/KeyguardViewMediator.java
```
　　private boolean mExternallyEnabled = true;
```
　　将其修改成false
　　这样更改就不会再进入休眠状态了

### 6初始化语言:
　　参考下篇文章

### 7设定初始化主页:
　　package/app/Browser/res/values/String.xml
　　655行
　　后面的应该是书签里的　　

### 8设定亮度0~255:
　　frameworks/base/packages/SettingsProvider/res/values/defaults.xml
　　def_screen_brightness-->这个值初始化好像是100多

### 9音量:
　　frameworks/base/media/java/android/media/AudioManager.java
　　数组DEFAULT_STREAM_VOLUME第4个值(最大我设置到30但是还是差2格才到最大--默认是11
　　建议将数组里的所有的数值都设为最大就OK了)
　　mm frameworks/base
       AudioService.java中定义了每一种音频流的最大音量级别：

```
/** @hide Maximum volume index values for audio streams */
private int[] MAX_STREAM_VOLUME = new int[] {
        5,  // STREAM_VOICE_CALL
     7,  // STREAM_SYSTEM
        7,  // STREAM_RING
        15, // STREAM_MUSIC
        7,  // STREAM_ALARM
        7,  // STREAM_NOTIFICATION
        15, // STREAM_BLUETOOTH_SCO
        7,  // STREAM_SYSTEM_ENFORCED
       15, // STREAM_DTMF
       15  // STREAM_TTS
 };
```



### 10设置Google帐户，左上角提示“正在设置RK2818SDK”，要求改成“正在设置W9”:
　　out\target\product\sdkDemo\root 中default.prop文件第13行
      ro.product.model=rk2818sdk  改为 ro.product.model=W9

### 11录音没有小时显示。要求增加:
　　 packages\apps\SoundRecorder\src\com\android\soundrecorder
      （1）SoundRecorder.java中： `private void updateTimerView()``
    　　 把 `String timeStr = String.format(mTimerFormat, time/60, time%60);`
    　　 改为：`long hour=time/3600;`
           `String timeStr = String.format(mTimerFormat, hour, (time-hour*3600)/60, time%60);`
      (2)  \res\values中strings.xml改为：
           ``<string name="timer_format"><xliff:g id="format">%02d:%02d:%02d</xliff:g></string>``

### 12去掉Bluetooth:
　　　　　　(主界面->添加文件夹->Bluetooth received)
　　　　　　 ic_launcher_folder_bluetooth.png（72*72）:
　　　　　　  在\packages\apps\Bluetooth\res\drawable-hdpi
  　　　　　　解决方法：删除\out\target\product\sdkDemo\system\app下的Bluetooth.apk

### 13充电锁屏时图片：
　　　　　　frameworks\base\core\res\res\drawable-hdpi:ic_lock_idle_charging.png

### 14去掉锁屏时显示充电百分比在：
　　　　　　frameworks\base\core\res\res\values-zh-rCN中strings.xml 的lockscreen_plugged_in 括号中的内容及括号 !!

### 15去掉动态桌面背景选项：
　　　　　　packages\wallpapers\Basic 中AndroidManifest.xml的<service>这些，如星系注释掉如下这些：
```
                 <service
                   android:label="@string/wallpaper_galaxy"
                   android:name="com.android.wallpaper.galaxy.GalaxyWallpaper"
                   android:permission="android.permission.BIND_WALLPAPER">
                    <intent-filter>
                       <action android:name="android.service.wallpaper.WallpaperService" />
                    </intent-filter>
                   <meta-data android:name="android.service.wallpaper" android:resource="@xml/galaxy" />
                  </srvice>
```
### 16更改版本号:
　　　　build/core/Makefile
　　　　79行
　　　　RK_VER := xxx
### 17更改界面布局
　　位置:package/app/Launcher2/res/xml/default_workspace.xml
　　注意:adb shell 里当你点击任意的一个apk的时候,在后台会输出
```
　　I/ActivityManager(  728): Starting activity: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 　　 　cmp=com.estrongs.android.pop/.view.FileExplorerActivity bnds=[294,373][393,478] }
I/WindowManager(  728): Setting rotation to 1, animFlags=0
```
      看蓝色的部分就可以找到/前是包名/后面是类名-->这个是在default_workspace.xml里面需要用到的
　　launcher:packageName="com.android.browser"
      launcher:className="com.android.browser.BrowserActivity"
### 18如何将pdf类的文件放到桌面上
　　客户要求将他们自己制作的pdf帮助文档放置到桌面上使客人可以直接点击就浏览
```
　　  String urlString = "/system/app/Nvsbl P4Dv2 English Manual.pdf";
        Intent intent = new Intent();
        intent.setAction(android.content.Intent.ACTION_VIEW);
        intent.setDataAndType(Uri.fromFile(new File(urlString)),"application/pdf");
        startActivity(intent);
        finish();
```
　　　解释下:
　　　首先将pdf文档放到out/target/product/sdkDemo/system/app下
　　　将固定地址给出urlString,使用Intent
```
　　　intent.setAction(android.content.Intent.ACTION_VIEW);启动View
　　　intent.setDataAndType(Uri.fromFile(new File(urlString)),"application/pdf");　　　
```
　　　application/pdf可以指定别的格式包括音频,视频,图片等等但是没有试过有需要可以试试
　　　注意:这个方法很不到如果只放置一些图片还可以但是如果放置文件比较大那么打包出来的
　　　system.img文件会很大也就是占用本身的系统内存----->所以不推荐使用!!!!!!!!!!!
### 9.发现PIN解锁界面出现5秒之后就会自动进入睡眠，之后将无法再次解锁。目前解决方法：
  修改文件 frameworks\policies\base\phone\com\android\internal\policy\impl\KeyguardViewMediator.java

将如下这行：
`protected static final int AWAKE_INTERVAL_DEFAULT_MS = 5000;`
修改为：
`protected static final int AWAKE_INTERVAL_DEFAULT_MS = 1000 * 60 * 5;`