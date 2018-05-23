# android.app.Activity 的介绍
发现当前Android的资料不是很多，而且对于Activity的介绍也很少，所以把官方文档的android.app.Activity的介绍翻译了一下，加入了一些自己的理解。各位如果觉得我自己理解的不对，请无视。欢迎邮件讨论。

```
 android.app
public class

android.app.Activity
    java.lang.Object

        android.content.Context

            android.app.ApplicationContext    ViewInflate.Factory

                android.app.Activity      KeyEvent.Callback Window.Callback
```




Activity 是用户唯一可以看得到的东西。几乎所有的activity都与用户进行交互，所以Activity主要负责的就是创建显示窗口，你可以在这些窗口里使用setContentView(View)来显示你自己的UI。activity展现在用户面前的经常是全屏窗口，你也可以将activity作为浮动窗口来使用（使用设置了windowIsFloating的主题），或者嵌入到其他的activity（使用ActivityGroup）中。下面是两个几乎所有Activity的子类都实现了的方法。

* onCreate(Bundle) 这个方法是初始化 activity的地方. 最重要的是，你经常需要在这里使用setContentView(int) 来设置UI布局所使用的layout资源, 当你需要使用程序控制UI中的组件时可以使用 findViewById(int) 来获得对应的视图。

* 当用户离开activity时你可以在onPause() 进行相应的操作. 更重要的是，用户做的任何改变都应该在该点上提交 (经常提交到 ContentProvider 这里保存数据)。

如果要使用 Context.startActivity()来启动activity, activity都必须在启动者应用包的AndroidManifest.xml文件中有对应的 <activity> 定义。



Activity类是 application's overall lifecycle 的一个重要部分。

这里涉及到的主题:

- Activity 生命周期
- 配置改变
- 启动Activity并获得结果
- 保存持久状态
- 许可
- 进程生命周期

## Activity 生命周期
系统中的Activity可以通过一个activity栈来进行管理。当一个新的activity启动的时候，它首先会被放置在activity栈顶部并成为running状态的activity —— 之前的activity也在activity栈中，但总是被保存在它的下边，只有当这个新的activity退出以后之前的activity才能重新回到前景界面。



所有的activity本质上有四种状态：

* activity在屏幕的前景中（activity栈的顶端），它是active或者running状态。
* activity失去了焦点但是仍然可见（这个activity顶上遮挡了一个透明的或者非全屏的activity），它的状态是paused。一个paused状态的activity完全是alive的（它维护自己所有的状态和成员信息，而且仍然在window manager的管理中），但当系统内存极度贫乏时也会将其killed。
* activity由于其他的activity而完全变暗，它就进入了stopped状态。它仍然保持着所有的状态和成员的信息，可是，他对于用户来说不可见，当别的地方需要内存的时候它经常会被killed。
* activity是paused或者stopped，系统需要将其清理出内存的时可以命令其finish或者简单kill其进程。当它重新在用户面前显示的时候，它必须完全重新启动并且将其关闭之前的状态全部恢复回来。

下面的图表是Activity的状态图，直角矩形代表了callback方法，你可以实现这些方法从而使Activity在改变状态的时候执行你制定的操作。带颜色的椭圆形是Activity的主要状态。


activity lifecycle


这里有三个比较关键的生命周期。

* 从最初调用onCreate(Bundle)到最终调用onDestroy()称为完整生命周期。Activity会在onCreate()进行所有“全局”状态的设置，在onDestroy()中释放所有持有的资源。举个例子，如果它有一个从网络上下载数据的后台线程，那他可能就会在onCreate()中创建这个线程并在onDestroy()中停止这个线程。
* 从activity调用onStart()开始，到调用对应的onStop()为止称为可见生命周期。在这段时间内用户可以在屏幕上看到这个activity，尽管并不一定是在前景也不一定可以和用户交互。在这两个方法之间你可以维护那些activity在用户显示时所需的资源。举个例子来说，你可以在onStart()中注册一个IntentReceiver来监控那些可以对你的UI产生影响的环境改变，当你的UI不继续在用户面前显示时你可以在onStop()中注销这个IntentReceiver。每当activity在用户面前显示或者隐藏时都会调用相应的方法，所以onStart()和onStop()方法在整个生命周期中可以多次被调用。
* 从activity调用onResume()开始，到调用对应的onPause()为止称为前景生命周期，这段时间activity处于其他所有activity的前面，且与用户交互。一个activity可以经常在resumed和paused状态之间转换——例如手机进入休眠时、activity的结果返回时、新的intent到来时——所以这两个方法中的代码应该非常的简短。

下面的Activity方法定义了activity完整的生命周期。他们全都是hook方法，你可以重载这些方法从而使activity在状态改变时执行你所期望的操作。所有activity都应该实现自己的onCreate(Bundle)方法来进行初始化设置；大部分还应该实现onPause()方法提交数据的修改并且准备终止与用户的交互。尽管我们计划在系统中添加更多的工具来管理应用，现在大多activity仍需要实现onFreeze()并且在onCreate(Bundle)中执行对应的状态恢复。其他的方法可以在需要时进行实现，当实现这些方法的时候需要注意的是一定要调用父类中的对应方法。


```
public class Activity extends ApplicationContext {
     protected void onCreate(Bundle icicle);
     protected void onStart();
     protected void onRestart();
     protected void onResume();
     protected void onFreeze(Bundle outIcicle);
     protected void onPause();
     protected void onStop();
     protected void onDestroy();
 }
```

一般来说activity的生命周期变化看起来比较象下面的表格：

|方法|Killable?|下一方法|描述|
|-|-|-|-|
|onCreate()|No|onStart() or onRestart()|Activity初次创建时被调用，你应该在这里进行一般的静态设置：创建view、将数据绑定到list等等。如果activity之前存在冻结状态，那么此状态将在Bundle中提供。如果activity首次创建，本方法后将会调用onStart()，如果activity是停止后重新显示，则将调用onRestart()。|
|-onStart()|NO|onRestart()or onResume()|当activity对用户即将可见的时候调用。其后调用onRestart()或onResume()(框架是否进行选择性调用onResume()仅仅是猜测)|
|-onRestart()|NO|onResume()|当activity从停止状态重新启动时调用。其后调用onResume()。|
|--onResume()|No|onFreeze()|1.当activity将要与用户交互时调用此方法，此时activity在activity栈的栈顶，用户输入已经可以传递给它。2.如果其他的activity在它的上方恢复显示，则将调用onFreeze()。|
|--onFreeze()|No|onPause()|1.当你的activity被暂停而其他的activity恢复与用户交互的时候这个方法会被调用（在其他activity显示之前），你可以使用这个方法保存你当前的用户状态（一般来说是当前实例的用户状态）。暂停之后，为了回收资源供给前景activity，系统会在需要的时间停止（或者kill）你的应用。2.以后如果你的activity启动一个新的实例重新与用户进行交互，你保存在这里的状态都将通过onCreate()方法传递给新的实例。其后总是调用onPause()方法。|
|--onPause()|Yes|onResume() or onStop()|1.当系统要启动一个其他的activity时调用（其他的activity显示之前），这个方法被用来提交那些持久数据的改变、停止动画、和其他占用CPU资源的东西。由于下一个activity在这个方法返回之前不会resumed，所以实现这个方法时代码执行要尽可能快。2.如果activity重新回到前景时将调用onResume()， 如果对用户彻底不可见则会调用onStop()。|
|-onStop()|Yes|onStart() or onDestroy()|1.当另外一个activity恢复并遮盖住此activity,导致其对用户不再可见时调用。一个新activity启动、其它activity被切换至前景、当前activity被销毁时都会发生这种场景。2.当activity重新回到前景与用户交互时调用onRestart()，如果activity将退出则调用onDestory()。|
|onDestroy()|Yes|nothing|在你的activity被销毁前所调用的最后一个方法，当进程终止时会出现这种情况（对activity直接调用finish()方法或者系统为了节省空间而临时销毁此activity的实例，你可以通过isFinishing()的返回值来区分这两种情况）。

（此处译者进行了大块的修改，请参考原文阅读下面表格）





①: 这个表格本人觉得还有些值得商榷的地方，建议作为参考阅读，不管是原文还是译文。



注意上表中“Killable”这一列 —— 对于那些标记killable的方法，当这些方法结束后，activity的进程可能在任何时间被系统kill而不再执行activity中的任何代码。因此你应该利用onFreeze()（保存你当前UI的状态）和onPause()（将所有的修改写回持久存储），这样activity才能在被kill的时候正确的保存当前的状态。如果需要了解一个进程的生命周期与他所执行的activity之间的关系 参见 进程生命周期 部分。



       对于那些标记killable的方法，从这些方法启动开始直到返回之前，activity的进程都不回被系统kill。举个例子，一个activity在onPause()方法返回后处于killable的状态，这种状态会一直持续到onResume()方法开始执行。



## 配置改变
如果设备的配置（在Resources.Configuration中进行了定义）发生改变，那么所有用户界面上的东西都需要进行更新，以适应新的配置。因为Activity是与用户交互的最主要的机制，它包含了处理配置改变的专门支持。



       除非你特殊指定，否则当配置发生改变（比如屏幕方向、语言、输入设备等等的改变）时你当前的activity都将被销毁，这销毁是通过一个正常的activity生命周期过程（onFreeze(Bundle), onPause(), onStop(), 和 onDestroy()）进行的。如果activity之前正在前景画面，当这个实例的onDestroy()调用完成后将会启动这个activity的一个新的实例，并将前面那个实例中onFreeze(Bundle)所保存的内容传递给新的实例。



因为任何的应用资源（包括layout文件）都有可能由于任何配置值而改变。因此处理配置改变的唯一安全的方法就是重新获取所有的资源，包括layout、绘图资源（原文drawables）、字符串资源。由于activity已经如何保存自己的状态并从这些状态中重建自身，所以activity重新启动自身来获得新的配置将是一个非常便利的途径。



       在一些特殊的情况中，你可能希望当一种或者多种配置改变时避免重新启动你的activity。你可以通过在manifest中设置android:configChanges属性来实现这点。你可以在这里声明activity可以处理的任何配置改变，当这些配置改变时不会重新启动activity，而会调用activity的onConfigurationChanged(Resources.Configuration)方法。如果改变的配置中包含了你所无法处理的配置（在android:configChanges并未声明），你的activity仍然要被重新启动，而onConfigurationChanged(Resources.Configuration)将不会被调用。



## 启动Activity并获得结果
startActivity(Intent)方法可以用来启动一个新的activity，这个activity将被放置在activity栈的栈顶。这个方法只有一个参数Intent，这个参数描述了将被执行的activity。



       有时候你希望在一个activity结束时得到它返回的结果。举个例子，你可能启动一个activity来让用户从通讯簿中选择一个人；当它结束的时候将会返回这个所选择的人。为了得到这个返回的信息，你可以使用startSubActivity(Intent, int)这个方法来启动新的activity，第二个整形参数将会作为这次调用的识别标记。这个activity返回的结果你可以通过onActivityResult(int, int, String, Bundle)方法来获得，此方法的第一个参数就是之前调用所使用的识别标记。



       当activity退出的时候，它可以调用setResult(int)来将数据返回给他的父进程。这个方法必须提供一个结果码，这个结果码可以使标准结果RESULT_CANCELED, RESULT_OK，也可以是其他任何从RESULT_FIRST_USER开始的自定义值。此外，它还可以返回一段字符串（经常是一段数据的URL地址），一个包含它所有希望值的Bundle。这些信息都会在父activity的回调函数Activity.onActivityResult()中出现，并连同最初提供的识别标记一起（此处有些拗口，意思其实就是子activity返回的内容、返回码、识别标记都将作为参数，按照不同的返回情况来调用父activity的Activity.onActivityResult()方法，以实现出现各种返回时父activity做出响应的处理）。



       如果子activity由于某种情况发生失败（例如crashing），父activity将会收到RESULT_CANCELED结果码。



       这里是一个例子，说明了如何启动一个新的activity并处理结果。


```
public class MyActivity extends Activity {
     ...
     static final int PICK_CONTACT_REQUEST = 0;
     public boolean onKeyDown(int keyCode, KeyEvent event) {
         if (keyCode == KeyEvent.KEYCODE_DPAD_CENTER) {
             // When the user center presses, let them pick a contact.
             startSubActivity(
                 new Intent(Intent.PICK_ACTION,
                 Uri.parse("content://contacts")),
                 PICK_CONTACT_REQUEST);
            return true;
         }
         return false;
     }
     protected void onActivityResult(int requestCode, int resultCode,
             String data, Bundle extras) {
         if (requestCode == PICK_CONTACT_REQUEST) {
             if (resultCode == RESULT_OK) {
                 // A contact was picked.  Here we will just display it
                 // to the user.
                 startActivity(new Intent(Intent.VIEW_ACTION,
                     Uri.parse(data)));
             }
         }
     }
 }
```


当一个按键被按下时onKeyDown()启动一个新的activity。当新的activity结束后onActivityResult()会被调用。

## 保存持久状态
一般来说有两类持久状态需要activity来处理：类似于文档的共享数据（一般使用content provider存储在SQLite数据库中）和就像用户参数设定一样的内部状态。



       对于content provider的数据，我们建议activity使用“编辑即发生”用户模型。就是说，用户所做的任何编辑都将立即生效而不要求任何进一步的确认。下面两条规则可以使支持这种模型成为一件简单的事情。

* 创建一个新文档的时候，立即为它创建文件或者后台数据库条目。举例来说，如果用户要写一封新邮件，当他们开始输入的时候马上创建一个新的数据库条目，所以即使他们输入以后进入别的activity，这封邮件也会在草稿箱中出现。

* 当activity的onPause()方法被调用时，它应该将任何用户所做修改向后台的content provider 或者文件提交。这可以确保即将运行的其他activity可以看到这些修改。你可能会使用更激进的方式来在你的activity生命周期中关键的时间点上提交数据：例如在启动一个新的activity之前、终止你的activity之前、当用户在输入域之间切换时，等等。(译者注：根据文档的其他部份，个人觉得启动新activity之前及终止当前activity之前进行保存的说法都值得商榷，具体位置仍可能是在onPause()中)

这个模型是为了避免用户在activity之间切换时数据丢失而设计的，同时允许系统在activity处于paused状态后任何时间都可以安全的kill这个activity（因为其它地方可能需要系统资源）。注意这隐含着一个含义，即用户点击你activity的BACK键并不意味着“取消”——它意味着保存他当前的内容并离开你的activity。Activity中取消编辑必须通过其他机制来提供，例如显式的提供"revert" 或 "undo"选项。



如果需要了解更多关于content providers的信息，参见content package。这是不同的activity之间如何调用和彼此之间传递数据的关键。



Activity同时提供了API来管理与activity相关联的内部持久状态。这可以用来记忆用户日历的首选初始显示（日期视图或者周视图）或者用户web浏览器的缺省主页这类信息。



       Activity持久状态使用getPreferences(int)方法来管理，允许你取得和修改一系列与activity相关联的名称/取值对。如果要在应用的多个组件（activities,recivers,services,providers）之间共享参数，你可以使用底层方法Context.getSharedPreferences()来获得存储在一个特定名称下的参数对象。（注意：在application packages之间不能共享设置数据 —— 如果要这么做的话你需要使用content provider）。



这里摘录了一段日历activity中的代码，用来在永久设定中保存用户首选的视图模式。


```
public class CalendarActivity extends Activity {
     ...
     static final int DAY_VIEW_MODE = 0;
     static final int WEEK_VIEW_MODE = 1;
     private SharedPreferences mPrefs;
     private int mCurViewMode;

     protected void onCreate(Bundle icicle) {
         super.onCreate(icicle);
         SharedPreferences mPrefs = getSharedPreferences();
         mCurViewMode = mPrefs.getInt("view_mode" DAY_VIEW_MODE);
     }

     protected void onPause() {
         super.onPause();
         SharedPreferences.Editor ed = mPrefs.edit();
         ed.putInt("view_mode", mCurViewMode);
         ed.commit();
     }
 }
```
## 许可
你可以通过在Activity所属应用的manifest文件中对应的<activity>标签中进行声明来限制哪些应用可以启动此Activity。如果你进行了声明，其它应用需要在他们自己的manifest文件中声明对应的<uses-permission>元素（而且需要在安装时被授予许可：译者注）才可以启动这个activity。



如果需要了解更多有关于一般安全机制和许可方面的信息可以参考Security Model。

## 进程生命周期
Android系统会尽量久的保留应用进程。但是当内存降低时最终还是要移除旧的进程。就像Activity 生命周期 中描述的一样，移除哪个进程还是要取决于关联的用户与之交互的程度。一般来说，进程可以基于其中运行的activity所处的生命周期而分成四种状态，下面将这些状态根据重要程度排列。系统会在kill更重要的进程（第一个）之前首先kill那些不那么重要的进程（最后一个）。



1.     前景activity（处于屏幕最上方，用户与之交互的activity）被认为是最重要的。如果设备上的内存无法满足它的使用，Kill此进程只能作为最后的手段。一般来说这个时候设备处于内存paging状态，为了使用户界面保持响应才会发出这个kill请求。

2.     可见activity（一个对用户可见，但是不在前景的activity，比如处在浮动对话框后）被认为是非常重要的，除非为了保证前景activity运行，否则不会被kill。

3.     后台activity（一个对用户不可见，并处于paused状态的activity）就不再重要了，所以当需要为其它前景的或者可见的activity运行而回收内存时系统可以很安全的kill它们。

4.     空进程是一个没有运行activity或者其他应用组件（比如Service或者IntentReceiver类）的进程。当内存开始降低时系统很快就会kill掉这些进程。因此当你要在activity外运行任何的后台操作时，必须在IntentReceiver或Service的上下文环境中运行，这样系统才知道需要将你的进程保留而不是kill。

有些时候Activity可能需要长时间运行一个操作，且它并不依赖于activity的生命周期而存在。例如一个照相机应用可能允许你将照片上传到web站点。上传可能需要很长时间，在上传过程中应该允许用户离开这个应用。为了做到这一点，你的activity应该在上传时启动一个Service来执行此工作。这将使系统在你的进程上传数据的过程中能够恰当的区分它的优先级（认为此进程比其他不可见应用更重要），不管原来的activity的状态是paused、stopped还是finished。