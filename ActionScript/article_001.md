# ActionScript性能优化总结

[TOC]

## 1：内存释放优化原则

1. 被删除对象在外部的所有引用一定要被删除干净才能被系统当成垃圾回收处理掉；
2. 父对象内部的子对象被外部其他对象引用了，会导致此子对象不会被删除，子对象不会被删除又会导致了父对象不会被删除；
3. 如果一个对象中引用了外部对象，当自己被删除或者不需要使用此引用对象时，一定要记得把此对象的引用设置为 null;
4. 本对象删除不了的原因不一定是自己被引用了，也有可能是自己的孩子被外部引用了，孩子删不掉导致父亲也删不掉；
5. 除了引用需要删除外，系统组件或者全局工具、管理类如果提供了卸载方法的就一定要调用删除内部对象，否则有可能会造成内存泄露和性能损失；
6. 父对象立刻被删除了不代表子对象就会被删除或立刻被删除，可能会在后期被系统自动删除或第二次移除操作时被删除；
7. 如果父对象 remove 了子对象后没有清除对子对象的引用，子对象一样是不能被删除的，父对象也不能被删除；
8. 注册的事件 如果没有被移除不影响自定义的强行回收机制，但有可能会影响正常的回收机制，所以最好是做到注册的事件监听器都要记得移除干净。
9. 父对象被删除了不代表其余子对象都删除了，找到一种状态的泄露代码不等于其他状态就没有泄露了，要各模块各状态逐个进行测试分析，直到测试任何状态下都能删除整个对象为止。


## 2：内存泄露举例

1. 引用泄露：对子对象的引用，外部对本对象或子对象的引用都需要置 null ；
2. 系统类泄露：使用了系统类而忘记做删除操作了，如 BindingUtils.bindSetter() ， ChangeWatcher.watch() 函数 时候完毕后需要调用 ChangeWatcher.unwatch() 函数来清除引用 ，否则使用此函数的对象将不会被删除；
类似的还有 MUSIC ， VIDEO ， IMAGE ， TIMER ， EVENT ， BINDING 等。
3. 效果 泄露：当对组件应用效果 Effect 的时候，当本对象本删除时需要把本对象和子对象上的 Effect 动画 停止掉，然后把 Effect 的 target 对象置 null; 如果不停止掉动画直接把 Effect 置 null 将不能正常移除对象。
4. SWF 泄露：要完全删除一个 SWF 要调用它的 unload() 方法并且把对象置 null;
5. 图片泄露：当 Image 对象使用完毕后要把 source 置 null;( 为测试 ) ；
6. 声音、视频 泄露 : 当不需要一个音乐或视频是需要停止音乐，删除对象，引用置 null;

内存泄露解决方法：

1. 在组件的 REMOVED_FROM_STAGE 事件回掉中做垃圾处理操作（移除所有对外引用（不管是 VO 还是组件的都需要删除），删除监听器，调用系统类的清除方法）
先 remove 再置 null, 确保被 remove 或者 removeAll 后的对象在外部的引用全部释放干净 ;
2. 利用 Flex 的性能优化工具 Profile 来对项目进程进行监控，可知道历史创建过哪些对象，目前有哪些对象没有被删除，创建的数量，占用的内存比例和用量，创建过程等信息。

## 3：代码写法优化性能

1. 定义局部变量
定义局部变量的时候，一定要用关键字var来定义，因为在Flash播放器中，局部变量的运行速度更快，而且在他们的作用域外是不耗占系统资源的.当一个函数调用结束的时候，相应的局部变量都会被销毁，并且释放出他们占有的系统资源
2. 申明变量时强制类型
  var a:int; 而非 var a;
3. 访问静态属性比非静态属性慢
4. getter setter比直接访问属性慢
5. final变量和类效率更高
6. 申明多个变量时单行效率比多行申明高
```
for(var i:int=0; i<100000; i++)
{
    var v1:Number=10, v2:Number=10, v3:Number=10, v4:Number=10;
}
```
7. 不要在循环内定义变量
8. 删除不必要的导入类
9. as比type(value)转型要快
10. 短变量名比长变量名高效
11. 尽量将代码内嵌，避免过多的函数调用。（比如用 value > 0 ? value : -value; 来代替 Math.abs()）

调用函数成本很高。尝试通过移动内联代码来减少函数的调用次数

优化代码如下:运行速度提高四倍以上。
```
for (i = 0; i< MAX_NUM; i++)
{
	currentValue = arrayValues;
	arrayValues = Math.abs ( currentValue );
}
```
12. if(a.a) 比 if(a.a==null) 快
13. for..in 效率大于for ，需求证
14. 当用到复杂的条件表达式时。把他们打散成为嵌套的独立判断结构是最佳方案。比如if(a&&b&&c)的效率就低于if(a){if(b){if(c){}}}。
15. 对 while 循环使用相反的顺序 （ while (--i > -1) { } ） 比for更高效
16. 低阶运算效率高于高阶运算
unit()--->Math.floor()     更快的算法:var u:uint=1.5>>0;
int()--->Math.ceil()
位运算>乘法>除法 ，但要注意是的有些运算是不能转换的 13/2 不能转换成 13<<0;
17. 方法闭包作为监听器更方便和高效，因为他不需要活动的对象作为依赖体.
```
class Form
{
    function setupEvents()
    {
        var f = function(event:Event) 
        {
            trace("my handler");
        }
        grid.addEventListener("click", f);
    }
}
```
优于
```
class Form
{
    function setupEvents()
    {
        grid.addEventListener("click", f);
    }
    function f(event:Event)
    {
        trace("my handler");
    }
}
```
## 4：数组等容器优化
1. 初始化 var a:Array=[]; 效率高于 var a:Array = new Array();
2. 一维数组来代替二维数组
3. 关于Vector和Array
     1) 在基本数据类型 int number string 时Vector 效率优于Array
     2) 在Object中，还是选择Array，不管是排序还是大量应用Array,特别在用sortOn时Array比Vector要快.
     3) 固定长度的 Vector 更快。( Vector 可以这样初始化：`var coords:Vector.<Number> = Vector.<Number>([132, 20, 46, 254, 244, 100, 20, 98, 218, 254]);`)



4. 数组循环:

    用局部变量缓存数组长度 ,用uint代替int
```
        for(var i:uint=0;i<len;i++)
        {
              Var a:Object = arr;
        }
```
1. 重置数组用
   Arr.length=0 效率最快
2. 初始化数组
   a[]比push方法效率高600%
3. 尽可能少使用方括号操作符访问 Vector 或 Array 元素，可以利用一个临时变量来操作。
4. Dictionary 弱引用可用来自动销毁资源.

## 5：显示对象api相关优化
1. 新的 drawing API 更快，它们是：drawPath, drawGraphicsData, drawTriangles。
2. 使用 setVector() 方法来处理像素。
3. 使用 setPixel() 和 setPixel32 方法时，要配合使用 lock() 和 unlock() 方法。
4. 尽可能在避免在循环内更新 TextField。
5. 位图还是矢量图:
  矢量:同屏元素较少, 需要放大不失真，位图:需要大量元件，游戏特效，图像特效
  矢量平滑处理:减少控制点。可使帧频稳定。
选中矢量图,Ctrl+Alt+Shift+C，减少线条
  位图压缩方式 Fireworks 可以用PNG8格式高效的压缩带Alpha通道的图片
6. 运动物体针对每个子元件使用 cacheAsBitmap 而不是针对父元件使用。
7. 使用 cacheAsBitmap 和 opaqueBackground 参数可以改进渲染性能（包含 TextField ）。
8. stage3d在旋转、缩放和alpha混合方面很有优势，这块是位图的软肋，位图只有在原始状态下渲染是最快的，想要不影响效率就得预先缓存出各种情况，会耗费掉大量内存，因此在这方面stage3d的性能更加强大 ,可以用3d来做2d加速
9. 使用 TextField 时，appendText() 方法比 += 操作符要快。
10. 使用 TextLine 处理静态文本比 TextField 快而且使用更少的内存
11. 对文本基本搜索和提取时，使用 String 类方法而非正则表达式
12. ，也会对播放速率造成影响。低透明度的可设置visable为flase
13. 不要用masks 应该用 scrollRect来做遮罩效果
14. 使用适当的DisplayObject
      * Shape – 没有交互（占内存少）
      * Sprite – 有交互（占内存多）
      * MovieClip – 有时间线（占内存更多）
15. 减少重绘区域
      显示重绘区域:flash.profiler.showRedrawRegions (true,0x33ff00);
16. 尽可能禁用鼠标交互。obj.mouseEnabled = false; obj.mouseChildren = false;
17. 激活和停用事件
   使用 Event.ACTIVATE 和 Event.DEACTIVATE 事件检测后台是否处于非活动状态，并相应地优化应用程序。对于移动设备很重要，例如后台运行air时，可将其帧频减小。
18. 回调方法 要比 event 快而且消耗更少的内存

## 6：资源优化

对象缓存池
* 减少文件大小 （嵌入字体少用，用滤镜而非位图）
* 合理加载，要用时加载资源
* 列表和列表数据分开加载
* 数据分页
* 分次请求

资源销毁
1. 释放内存 – 将所有对象的引用设置为 null
2. bitmap bitmapData 不用时  dispose();并将引用设置为null
3. 卸载从外部装载的内容时，使用loader.unloadAndStop()，而不要使用 loader.unload()。
4. xml对象会比字符串占用内存增大20倍，可用System.disposeXML()来断开程个xml对象连接销毁xml对象.
5. URLLoader.close();URLStream.close();加载素材时可用这两个加载再用Loader.loadbytes。因为  Loader.load会展开swf文件，内存比压缩的流数据占用内存大.
6. 适当使用事件冒泡，如有大量子对象需要添加事件监听器时，可以添加在父窗口中，冒泡实现 。
,大量事件冒泡是不必需的，可停止冒泡.事件记得移除监听
7. 不可见mc ，先stop,再移除监听，再从舞台上remove.
8. 如果之前使用了ExternalInface.callBack("APIName", functionName)声明了一个API，则可以使用ExternalInface.callBack("APIName", null)取消该API

总结:
* 移除监听
* 管理intervals timeout timer
* 卸载swf
* 停止声音视频
* 移除显示对象
* 停止时间帧
* 关闭连接 LocalConnections NetConnection NetStream
* 没有摄像头和麦克风处理的处理
* 释放位图数据