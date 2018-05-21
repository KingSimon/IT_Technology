# 用Android访问本地站点---(localhost,10.0.2.2)要区别
不知道大家是否想试一下用Android模拟器如何访问自己建的Web站点呢? 下面我将讲一下如何实现.



## Step 1:我用的Tomcat作为本地服务器,在Webapps这个目录里,把我的名为kankong的Web工程放进来,启动Tomcat服务器.

在浏览器里输入:http://localhost(或者127.0.0.1):8080/kankong/index.html将出现如下界面:







## Step 2:启动Android 模拟器:



如果你在Eclipse里已经启动了Android模拟器就跳过此步.我们如何手动启动Android模拟器呢?



首先运行cmd出现我们习惯的黑屏,输入Android list avd(列出所有AVD模拟器)







然后我们选择其中一个启动以Android 1.5为例子:emulator -debug avd_config -avd android 1.5:

出现我们熟悉的画面如下:







## Step 3:打开浏览器输入http://localhost:8080/kankong/index.html?



我们的第一想法是输入http://localhost:8080/kankong/index.html,可是这将不会成功,为什么呢?问题是这样的，android模拟器（simulator）把它自己作为了localhost,也就是说，代码中使用localhost或者127.0.0.1来访问，都是访问模拟器自己！这是不行的！


如果你想在模拟器simulator上面访问你的电脑，那么就使用android内置的IP 10.0.2.2 吧，  10.0.2.2 是模拟器设置的特定ip，是你的电脑的别名alias记住，在模拟器上用10.0.2.2访问你的电脑本机.

也就是输入http://10.0.2.2:8080/kankong/index.html将出现如下界面:







OK~这样就大功告成了!