# Android 安全架构及权限控制机制剖析
## Android 层次化安全架构

Android 作为一个移动设备的平台，其软件层次结构包括了一个操作系统（OS），中间件（MiddleWare）和应用程序（Application）。根据 Android 的软件框图，其软件层次结构自下而上分为以下几个层次：

* 操作系统层（OS）
* 各种库（Libraries）和 Android 运行环境（RunTime）
* 应用程序框架（Application Framework）
* 应用程序（Application）

以下分别介绍 Andoid 各个层次的软件的重点及其相关技术：

### （1）操作系统层（OS）

Android 使用 Linux2.6 作为操作系统，Linux2.6 是一种标准的技术，Linux 也是一个开放的操作系统。Android 对操作系统的使用包括核心和驱动程序两部分，Android 的 Linux 核心为标准的 Linux2.6 内核，Android 更多的是需要一些与移动设备相关的驱动程序。主要的驱动如下所示：

* 显示驱动（Display Driver）：常用基于 Linux 的帧缓冲（Frame Buffer）驱动
* Flash 内存驱动（Flash Memory Driver）
* 照相机驱动（Camera Driver）：常用基于 Linux 的 v4l（Video for ）驱动。
* 音频驱动（Audio Driver）：常用基于 ALSA（Advanced Linux Sound Architecture，高级 Linux 声音体系）驱动
* WiFi 驱动（Camera Driver）：基于 IEEE 802.11 标准的驱动程序
* 键盘驱动（KeyBoard Driver）
* 蓝牙驱动（Bluetooth Driver）
* Binder IPC 驱动：Andoid 一个特殊的驱动程序，具有单独的设备节点，提供进程间通讯的功能。
* Power Management（能源管理）
### （2）各种库（Libraries）和 Android 运行环境（RunTime）

本层次对应一般嵌入式系统，相当于中间件层次。Android 的本层次分成两个部分一个是各种库，另一个是 Android 运行环境。本层的内容大多是使用 C++ 实现的。 在其中，各种库包括：

* C 库：C 语言的标准库，这也是系统中一个最为底层的库，C 库是通过 Linux 的系统调用来实现。
* 多媒体框架（MediaFrameword）：这部分内容是 Android 多媒体的核心部分，基于 PacketVideo（即 PV）的 OpenCORE，从功能上本库一共分为两大部分，一个部分是音频、视频的回放（PlayBack），另一部分是则是音视频的纪录（Recorder）。
* SGL：2D 图像引擎。
* SSL：即 Secure Socket Layer 位于 TCP/IP 协议与各种应用层协议之间 , 为数据通讯提供安全支持。
* OpenGL ES 1.0 ：本部分提供了对 3D 的支持。
* 界面管理工具（Surface Management）：本部分提供了对管理显示子系统等功能。
* SQLite：一个通用的嵌入式数据库
* WebKit：网络浏览器的核心
* FreeType：位图和矢量字体的功能。

Android 的各种库一般是以系统中间件的形式提供的，它们均有的一个显著特点就是与移动设备的平台的应用密切相关。 Android 运行环境主要指的虚拟机技术—— Dalvik。Dalvik 虚拟机和一般 JAVA 虚拟机（Java VM）不同，它执行的不是 JAVA 标准的字节码（bytecode ）而是 Dalvik 可执行格式（.dex）中执行文件。在执行的过程中，每一个应用程序即一个进程（Linux 的一个 Process）。 二者最大的区别在于 Java VM 是以基于栈的虚拟机（Stack-based），而 Dalvik 是基于寄存器的虚拟机（Register-based）。显然，后者最大的好处在于可以根据硬件实现更大的优化，这更适合移动设备的特点。

### （3）应用程序框架（Application Framework）

Android 的应用程序框架为应用程序层的开发者提供 APIs，它实际上是一个应用程序的框架。由于上层的应用程序是以 JAVA 构建的，因此本层次提供的首先包含了 UI 程序中所需要的各种控件： 例如： Views ( 视图组件 ) 包括 lists( 列表 ), grids( 栅格 ), text boxes( 文本框 ), buttons( 按钮 ) 等，甚至一个嵌入式的 Web 浏览器。一个 Android 的应用程序可以利用应用程序框架中的以下几个部分： Activity （活动）、Broadcast Intent Receiver （广播意图接收者）、Service （服务）、Content Provider （内容提供者）。

### （4）应用程序（Application）

Android 的应用程序主要是用户界面（User Interface），通常以 JAVA 程序编写，其中还可以包含各种资源文件（放置在 res 目录中）。JAVA 程序及相关资源经过编译后，将生成一个 APK 包。Android 本身提供了主屏幕（Home），联系人（Contact），电话（Phone），浏览器（Browsers）等众多的核心应用。同时应用程序的开发者还可以使用应用程序框架层的 API 实现自己的程序。


图 1. Android 分层安全架构图

![](img/a061_001.gif)



## Android 的组件模型（Component Model）

Android 系统中包括 4 种组件：

###（1）Activity

Activity 就是一个界面，这个界面里面可以放置各种控件。例如：Task Manager 的界面、Root Explorer 的界面等；Activity 是为用户操作而展示的可视化用户界面。例如说，一个 activity 可以展示一个菜单项列表供用户选择，或者显示一些包含说明的照片。一个短消息应用程序可以包括一个用于显示做为发送对象的联系人的列表的 activity，一个给选定的联系人写短信的 activity 以及翻阅以前的短信和改变设置的 activity。尽管它们一起组成了一个内聚的用户界面，但其中每个 activity 都与其它的保持独立。每个都是以Activity类为基类的子类实现。

一个应用程序可以只有一个 activity，或者，如刚才提到的短信应用程序那样，包含很多个。而每个 activity 的作用以及其数目，自然取决于应用程序及其设计。一般情况下，总有一个应用程序被标记为用户在应用程序启动的时候第一个看到的。从一个 activity 转向另一个的方式是靠当前的 activity 启动下一个。

每个 activity 都被给予一个默认的窗口以进行绘制。一般情况下，这个窗口是满屏的，但它也可以是一个小的位于其它窗口之上的浮动窗口。一个 activity 也可以使用超过一个的窗口。例如，在 activity 运行过程中弹出的一个供用户反应的小对话框，或是当用户选择了屏幕上特定项目后显示的必要信息。

窗口显示的可视内容是由一系列视图构成的，这些视图均继承自 View基类。每个视图均控制着窗口中一块特定的矩形空间。父级视图包含并组织它子视图的布局。叶节点视图（位于视图层次最底端）在它们控制的矩形中进行绘制，并对用户对其直接操作做出响应。所以，视图是 activity 与用户进行交互的界面。例如说，视图可以显示一个小图片，并在用户指点它的时候产生动作。Android 有很多既定的视图供用户直接使用，包括按钮、文本域、卷轴、菜单项、复选框等等。

###（2）Service

服务是运行在后台的功能模块。如文件下载、音乐播放程序等；服务没有可视化的用户界面，而是在一段时间内在后台运行。例如说，一个服务可以在用户做其它事情的时候在后台播放背景音乐、从网络上获取一些数据或者计算一些东西并提供给需要这个运算结果的 activity 使用。每个服务都继承自 Service基类。

一个媒体播放器播放播放列表中的曲目是一个不错的例子。播放器应用程序可能有一个或多个 activity 来给用户选择歌曲并进行播放。然而，音乐播放这个任务本身不应该为任何 activity 所处理，因为用户期望在他们离开播放器应用程序而开始做别的事情时，音乐仍在继续播放。为达到这个目的，媒体播放器 activity 应该启用一个运行于后台的服务。而系统将在这个 activity 不再显示于屏幕之后，仍维持音乐播放服务的运行。

可以连接至（绑定）一个正在运行的服务（如果服务没有运行，则启动之）。连接之后，可以通过那个服务暴露出来的接口与服务进行通讯。对于音乐服务来说，这个接口可以允许用户暂停、回退、停止以及重新开始播放。

###（3）Content Provider

它是 Android 平台应用程序间数据共享的一种标准接口，它以类似于 URI（Universal Resources Identification）的方式来表示数据，如：content://contacts/people/1101；内容提供者将一些特定的应用程序数据供给其它应用程序使用。数据可以存储于文件系统、SQLite 数据库或其它方式。内容提供者继承于 ContentProvider基类，为其它应用程序取用和存储它管理的数据实现了一套标准方法。然而，应用程序并不直接调用这些方法，而是使用一个 ContentResolver对象，调用它的方法作为替代。ContentResolver 可以与任意内容提供者进行会话，与其合作来对所有相关交互通讯进行管理。

###（4）Broadcast Receiver

Broadcast Receiver 是一个专注于接收广播通知信息，并做出对应处理的组件。很多广播是源自于系统代码的。例如，通知时区改变、电池电量低、拍摄了一张照片或者用户改变了语言选项。应用程序也可以进行广播，例如通知其它应用程序一些数据下载完成并处于可用状态。

应用程序可以拥有任意数量的 Broadcast Receiver 以对所有它感兴趣的通知信息予以响应。所有的接收器均继承自BroadcastReceiver基类。

Broadcast Receiver 没有用户界面。然而，它们可以启动一个 activity 来响应它们收到的信息，或者用 NotificationManager来通知用户。通知可以用很多种方式来吸引用户的注意力──闪动背灯、震动、播放声音等等。一般来说是在状态栏上放一个持久的图标，用户可以打开它并获取消息。

与此组件相关的概念是 Intent，Intent 是一个对动作和行为的抽象描述，负责组件之间程序之间进行消息传递。而 Broadcast Receiver 组件则提供了一种把 Intent 作为一个消息广播出去，由所有对其感兴趣的程序对其作出反应的机制。举个简单的例子，为了实现一个系统启动后播放音乐的功能，则可以定义 Intent 为 android.intent.action.BOOT_COMPLETED，由 Broadcast Receiver 组件将其进行广播，而系统中的 Media Player 接收到该信息后则进行播放。

如上所述，4 个组件之间的关系如下图：


图 2. Android 各组件关系图
![](img/a061_002.jpg)



## Android 的权限分类

根据用户的使用过程体验，可以将 Android 涉及的权限大致分为如下三类：

（1）Android 手机所有者权限：自用户购买 Android 手机后，用户不需要输入任何密码，就具有安装一般应用软件、使用应用程序等的权限；

（2）Android root 权限：该权限为 Android 系统的最高权限，可以对所有系统中文件、数据进行任意操作。出厂时默认没有该权限，需要使用 z4Root 等软件进行获取，然而，并不鼓励进行此操作，因为可能由此使用户失去手机原厂保修的权益。同样，如果将 Android 手机进行 root 权限提升，则此后用户不需要输入任何密码，都将能以 Android root 权限来使用手机。

（3）Android 应用程序权限：Android 提供了丰富的 SDK（Software development kit），开发人员可以根据其开发 Android 中的应用程序。而应用程序对 Android 系统资源的访问需要有相应的访问权限，这个权限就称为 Android 应用程序权限，它在应用程序设计时设定，在 Android 系统中初次安装时即生效。值得注意的是：如果应用程序设计的权限大于 Android 手机所有者权限，则该应用程序无法运行。如：没有获取 Android root 权限的手机无法运行 Root Explorer，因为运行该应用程序需要 Android root 权限。

回页首

Android 系统权限定义

Android 系统在 /system/core/private/android_filesystem_config.h 头文件中对 Android 用户 / 用户组作了如下定义，且权限均基于该用户 / 用户组设置。
```
 #define AID_ROOT             0  /* traditional unix root user */
 #define AID_SYSTEM        1000  /* system server */
 #define AID_RADIO         1001  /* telephony subsystem, RIL */
 #define AID_BLUETOOTH     1002  /* bluetooth subsystem */
 #define AID_GRAPHICS      1003  /* graphics devices */
 #define AID_INPUT         1004  /* input devices */
 #define AID_AUDIO         1005  /* audio devices */
 #define AID_CAMERA        1006  /* camera devices */
 #define AID_LOG           1007  /* log devices */
 #define AID_COMPASS       1008  /* compass device */
 #define AID_MOUNT         1009  /* mountd socket */
 #define AID_WIFI          1010  /* wifi subsystem */
 #define AID_ADB           1011  /* android debug bridge (adbd) */
 #define AID_INSTALL       1012  /* group for installing packages */
 #define AID_MEDIA         1013  /* mediaserver process */
 #define AID_DHCP          1014  /* dhcp client */
 #define AID_SDCARD_RW     1015  /* external storage write access */
 #define AID_VPN           1016  /* vpn system */
 #define AID_KEYSTORE      1017  /* keystore subsystem */
 #define AID_USB           1018  /* USB devices */
 #define AID_DRM           1019  /* DRM server */
 #define AID_DRMIO         1020  /* DRM IO server */
 #define AID_GPS           1021  /* GPS daemon */
 #define AID_NFC           1022  /* nfc subsystem */

 #define AID_SHELL         2000  /* adb and debug shell user */
 #define AID_CACHE         2001  /* cache access */
 #define AID_DIAG          2002  /* access to diagnostic resources */

 /* The 3000 series are intended for use as supplemental group id's only.
  * They indicate special Android capabilities that the kernel is aware of. */
 #define AID_NET_BT_ADMIN  3001  /* bluetooth: create any socket */
 #define AID_NET_BT        3002  /* bluetooth: create sco, rfcomm or l2cap sockets */
 #define AID_INET          3003  /* can create AF_INET and AF_INET6 sockets */
 #define AID_NET_RAW       3004  /* can create raw INET sockets */
 #define AID_NET_ADMIN     3005  /* can configure interfaces and routing tables. */

 #define AID_MISC          9998  /* access to misc storage */
 #define AID_NOBODY        9999

 #define AID_APP          10000 /* first app user */
```
值得注意的是：每个应用程序在安装到 Android 系统后，系统都会为其分配一个用户 ID，如 app_4、app_11 等。以下是 Calendar 和 Terminal 软件在 Android 系统中进程浏览的结果（其中，黑色字体标明的即为应用分配的用户 ID）：
```
 USER     PID   PPID  VSIZE  RSS     WCHAN    PC         NAME
 app_16    2855  2363  216196 20960 ffffffff afd0ee48 S com.android.providers.calendar
 app_91    4178  2363  218872 25076 ffffffff afd0ee48 S jackpal.androidterm
```
在 Android 系统中，上述用户 / 用户组对文件的访问遵循 Linux 系统的访问控制原则，即根据长度为 10 个字符的权限控制符来决定用户 / 用户组对文件的访问权限。该控制符的格式遵循下列规则：

* 第 1 个字符：表示一种特殊的文件类型。其中字符可为 d( 表示该文件是一个目录 )、b( 表示该文件是一个系统设备，使用块输入 / 输出与外界交互，通常为一个磁盘 )、c( 表示该文件是一个系统设备，使用连续的字符输入 / 输出与外界交互，如串口和声音设备 )，“.”表示该文件是一个普通文件，没有特殊属性。
* 2 ～ 4 个字符：用来确定文件的用户 (user) 权限；
* 5 ～ 7 个字符：用来确定文件的组 (group) 权限；
* 8 ～ 10 个字符：用来确定文件的其它用户 (other user，既不是文件所有者，也不是组成员的用户 ) 的权限。
* 第 2、5、8 个字符是用来控制文件的读权限的，该位字符为 r 表示允许用户、组成员或其它人可从该文件中读取数据。短线“-”则表示不允许该成员读取数据。
* 第 3、6、9 位的字符控制文件的写权限，该位若为 w 表示允许写，若为“-”表示不允许写。
* 第 4、7、10 位的字符用来控制文件的制造权限，该位若为 x 表示允许执行，若为“-”表示不允许执行。

举个例子，“drwxrwxr--  2 root   root    4096  2 月 11 10:36 lu”表示的访问控制权限（黑色字体标明）为：因为 lu 的第 1 个位置的字符是 d，所以由此知道 lu 是一个目录。第 2 至 4 位置上的属性是 rwx，表示用户 root 拥有权限列表显示 lu 中所有的文件、创建新文件或者删除 lu 中现有的文件，或者将 lu 作为当前工作目录。第 5 至 7 个位置上的权限是 rwx，表示 root 组的成员拥有和 root 一样的权限。第 8 至 10 位上的权限仅是 r--，表示不是 root 的用户及不属于 root 组的成员只有对 lu 目录列表的权限。这些用户不能创建或者删除 lu 中的文件、执行 junk 中的可执行文件，或者将 junk 作为他们的当前工作目录。



## Android 应用程序权限申请

每个应用程序的 APK 包里面都包含有一个 AndroidMainifest.xml 文件，该文件除了罗列应用程序运行时库、运行依赖关系等之外，还会详细地罗列出该应用程序所需的系统访问。程序员在进行应用软件开发时，需要通过设置该文件的 uses-permission 字段来显式地向 Android 系统申请访问权限。

### AndroidMainifest.xml 文件用途

AndroidManifest.xml 主要包含以下功能：

- 说明 application 的 java 数据包，数据包名是 application 的唯一标识；
- 描述 application 的 component；
- 说明 application 的 component 运行在哪个 process 下；
- 声明 application 所必须具备的权限，用以访问受保护的部分 API，以及与其他 application 的交互；
- 声明 application 其他的必备权限，用以 component 之间的交互；
- 列举 application 运行时需要的环境配置信息，这些声明信息只在程序开发和测试时存在，发布前将被删除；
- 声明 application 所需要的 Android API 的最低版本级别，例如 1.0，1.1，1.5；
- 列举 application 所需要链接的库；

### AndroidManifest.xml 文件的结构及元素

AndroidManifest.xml 文件的结构、元素，以及元素的属性，可以在 Android SDK 文档中查看详细说明。而在看这些众多的元素以及元素的属性前，需要先了解一下这些元素在命名、结构等方面的规则：

- 元素：在所有的元素中只有 <manifest> 和 <application> 是必需的，且只能出现一次。如果一个元素包含有其他子元素，必须通过子元素的属性来设置其值。处于同一层次的元素，这些元素的说明是没有顺序的。
- 属性：按照常理，所有的属性都是可选的，但是有些属性是必须设置的。那些真正可选的属性，即使不存在，其也有默认的数值项说明。除了根元素 <manifest> 的属性，所有其他元素属性的名字都是以 android: 前缀的；
- 定义类名：所有的元素名都对应其在 SDK 中的类名，如果你自己定义类名，必须包含类的数据包名，如果类与 application 处于同一数据包中，可以直接简写为“.”；
- 多数值项：如果某个元素有超过一个数值，这个元素必须通过重复的方式来说明其某个属性具有多个数值项，且不能将多个数值项一次性说明在一个属性中；
- 资源项说明：当需要引用某个资源时，其采用如下格式：@[package:]type:name。例如 <activity android:icon=”@drawable/icon ” . . . >
- 字符串值：类似于其他语言，如果字符中包含有字符“\”，则必须使用转义字符“\\”；

下面结合 cookie 实例中的 AndroidManifest.xml 文件来说明一下，原 XML 文件如下：
```
 <?xml version=”1.0 ″ encoding=”utf-8 ″ ?>
 <manifest xmlns:android=”http://schemas.android.com/apk/res/android”
 package=”moandroid.cookie”
 android:versionCode=”1 ″
 android:versionName=”1.0 ″ >
 <application android:icon=”@drawable/icon” android:label=”@string/app_name”>
 <activity android:name=”.cookie” android:label=”@string/app_name”>
 <intent-filter>
 <action android:name=”android.intent.action.MAIN” />
 <category android:name=”android.intent.category.LAUNCHER” />
 </intent-filter>
 </activity>
 </application>
 <uses-sdk android:minSdkVersion=”3 ″ />
 </manifest>
```
除了头部的 XML 信息说明外，首先是 manifest 项（也就是根节点），其属性包括：schemas URL 地址、包名（moandroid.cookie），以及程序的版本说明。其次是 manifest 的子节点 application，其属性包括：程序图标、程序名称。前面带有 @ 表示引用资源，例如：@drawable/icon 表示引用的是 drawable 资源中的 icon，可以在其源工程的 res/drawable 中找到。然后就是 application 的子节点 activity，其属性包括：activity 的名称、activity 的标签名，其子节点 intent-filter 则是对 activity 的说明。

而在 intent-filter 中，action android:name=”android.intent.action.MAIN”和 category android:name=”android.intent.category.LAUNCHER”用以说明程序启动时的入口 activity 是哪个。如果这两个属性值中分别含有 MAIN 和 LAUNCHER，则说明它就是启动程序时的入口活动。uses-sdk android:minSdkVersion=”3 ″说明程序使用的 Android SDK 的最低版本，其中 1 表示 Android 1.0，2 表示 Android 1.1，而 3 则表示 Android 1.5。

### 如何进行应用程序权限申请

如下所示，文中黑体标记的部分为应用程序权限申请内容：
```
 <?xml version="1.0" encoding="utf-8"?>
 <manifest xmlns:android="http://schemas.android.com/apk/res/android"
     package="cn.com.fetion.android"
     android:versionCode="1"
     android:versionName="1.0.0">
   <application android:icon="@drawable/icon" android:label="@string/app_name">
       <activity android:name=".welcomActivity"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
   </application>
 <uses-permission android:name="android.permission.SEND_SMS"></uses-permission>
 </manifest>
```
如上述文件描述中加下划线的斜体部分，该文件的作用是说明该软件需要发送短信的功能。

Android 定义了百余种 permission，可供开发人员使用，具体详见网址：http://developers.androidcn.com/reference/android/Manifest.permission.html。

在文件中，用户还可以自定义权限。permission 就是自定义权限的声明，可以用来限制 app 中特殊组件，特性与 app 内部或者和其他 app 之间访问。写了一个引用自定义权限的例子，在安装 app 的时候，提示权限：

定义权限如下：
```
 <permission android:label="”自定义权限”"

				 android:description=”@string/test”

				 android:name=”com.example.project.TEST”

				 android:protectionLevel=”normal”

				 android:icon=”@drawable/ic_launcher”>
```
声明的含义如下；

android:label：权限名字，显示给用户的，值可是一个 string 数据，例如这里的“自定义权限”。

android:description：比 label 更长的对权限的描述。值是通过 resource 文件中获取的，不能直接写 string 值，例如这里的”@string/test”。

android:name：权限名字，如果其他 app 引用该权限需要填写这个名字。

android:protectionLevel：权限级别，分为 4 个级别：

* normal：低风险权限，在安装的时候，系统会自动授予权限给 application。
* dangerous：高风险权限，系统不会自动授予权限给 app，在用到的时候，会给用户提示。
* signature：签名权限，在其他 app 引用声明的权限的时候，需要保证两个 app 的签名一致。这样系统就会自动授予权限给第三方 app，而不提示给用户。
* signatureOrSystem：这个权限是引用该权限的 app 需要有和系统同样的签名才能授予的权限，一般不推荐使用。

值得注意的是：通过测试发现一种特殊的情况，应用程序可以在程序运行时申请 root 权限，如下图，在使用 Android Terminal Emulator 时尝试使用 su 命令切换到 root 用户。若用户已通过 hacking 的方式使得 Android 系统获得了 root 权限，则可以允许该程序以 root 用户权限执行；反之即算用户选择“允许”，也不能使程序以 root 用户权限执行。


图 3. Android 用户权限赋予示意图
![](img/a061_003.gif)

Android 系统对应用程序权限申请的处理方式分析

对 Android 源代码中的如下文件进行分析：

* InstallAppProgress.java：其路径为 \packages\apps\PackageInstaller\src\com\android\packageinstaller\InstallAppProgress.java；
* PackageInstallerActivity.java：其路径为 \packages\apps\PackageInstaller\src\com\android\packageinstaller\PackageInstallerActivity.java；
* AppSecurityPermissions.java：其路径为 \frameworks\base\core\java\android\widget\AppSecurityPermissions.java

总结得出如下图所示的 Android 系统对应用程序授权申请的处理流程：

* 进入处理应用程序授权申请的入口函数；
* 系统从被安装应用程序的 AndroidManifest.xml 文件中获取该应用正常运行需申请的权限列表；
* 显示对话框，请求用户确认是否满足这些权限需求；
* 若同意，则应用程序正常安装，并被赋予相应的权限；若否定，则应用程序不被安装。系统仅提供给用户选择“是”或者“否”的权利，没有选择其中某些权限进行授权的权利。

图 4. Android 用户权限赋予示意图
![](img/a061_004.jpg)