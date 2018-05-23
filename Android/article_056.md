# Android反编译操作说明
1,工具介绍：
apktool:使用该工具可以反编译apk，得到所有资源文件
dex2jar:使用该工具可以反编译apk，得到源代码
jd-gui:查看jar包源代码工具。

2,操作步骤：
a,进入到apktool目录下执行如下命令
apktool d  G:\andriod\dycr.apk
命令执行完成后，会在apktool目录下生成dycr文件夹，资源文件就文件夹中。

b,首先将要反编译的.apk文件更改成.zip文件,并解压该zip文件得到classes.dex文件
再将classes.dex复制到dex2jar目录下，执行如下命令
dex2jar.bat classes.dex
命令执行完成后，会在dex2jar目录下生成classes.dex.jar文件，该文件是源代码文件。

c,使用jd-gui工具即可查看classes.dex.jar文件，就看到源代码啦。