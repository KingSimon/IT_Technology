# FlashDevelop开发在浏览器上调试Flash
在浏览器上调试flash可以使用火狐浏览器的扩展插件FlashFirebug。具体有什么功能大家可以到官网上查看，只是免费版的功能有限（确实需要的可以购买专业版），只有查看flash的显示对象以及其属性等一些基本功能，但这些功能已经很强大了。
使用FlashFirebug需要安装以下软件：火狐浏览器、Firebug、FlashFirebug和flashplayer_debug。首先下载火狐浏览器，安装好后用火狐浏览器打开Firebug和FlashFirebug的链接，增加应用就可以了。FlashplayerDebug从官网上下载，双击安装完成即可。

FireFox下载地址：http://www.firefox.com.cn/download/

Firebug下载地址：https://addons.mozilla.org/zh-CN/firefox/addon/firebug/

FlashFirebug下载地址：https://addons.mozilla.org/zh-CN/firefox/addon/flashfirebug/

FlashplayerDebug下载地址：Download the Windows Flash Player 11.3 Plugin content debugger

下载安装好后用火狐浏览器打开嵌有要调试的swf文件的网页，点击浏览器右上角的“小甲虫”打开Firebug,出现如下图片

选择工具栏里的Flash，点“点击检测页面上的SWF文件”就把当前网页上的flash加载进来。在右侧可以查看flash元件的基本信息和属性等，如果需要Console、SWFs等功能的话，给火狐开源开发团队捐献$9.99可以获得专业版的FlashFirebug。

如果想使用类似actionscript里的trace功能的话，还可以借助js的console.log方法。这个也是火狐独创的（Chrome和IE在后来好像也添加了类似的功能），同样需要用到Firebug（注意不是FlashFirebug）。参考源文：百度空间——梦想的家园

自己写一个类，代码如下：
```
package
{
import flash.external.ExternalInterface;

public class ExternalDebug
{
 public static function trace(... args):void {
  if (ExternalInterface.available) ExternalInterface.call("console.log",args);
 }
}
}
```
在AS3代码里需要使用trace的地方调用ExternalDebug.trace(args)就可以了,args是任意参数，可以是字符串，数据，其他类型的变量。编译出swf文件，在火狐浏览器上打开，使用Firebug，查看控制台，就可以看到神奇的效果，试试吧！