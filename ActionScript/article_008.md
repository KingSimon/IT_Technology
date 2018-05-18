# ActionScript3 一些琐碎的笔记
1. Loader、URLLoader 与 URLStream 的区别
* (1) Loader 于加载SWF 文件或图像（JPG、PNG 或 GIF）文件。给contentLoaderInfo属性添加侦听事件，来监测数据是否已下载完成。
* (2) URLLoader 以文本、二进制数据或 URL 编码变量的形式从 URL 下载数据(XML、JSP、PHP等)。 数据要先全部下载完才能使用，可以监视下载进度。
* (3) URLStream 提供对下载 URL 的低级访问。数据一下载，便可随即为应用程序使用，并且 URLStream 类还允许在完成下载前关闭流。已下载文件的内容将作为原始二进制数据提供，Big-endian 格式编码。

2. 自定义类分派事件后侦听失败

    要记住“谁分派谁侦听”这个原则。

3. ArgumentError: Error #2180: 如果 AVM1 内容（AS1 或 AS2）已加载到 AVM2 (AS3) 内容中，则不可将其移动到 displayList 的其他部分。
这个是虚拟机版本的问题，加载的swf文件可能是Player9或更前的版本。解决方法有两个：

* （1）把调试播放器的版本改为Player9（不实际）
* （2）直接使用loader，而不是loader.content。

4. Error #1007: 尝试实例化的函数不是构造函数

Embed是flex把资源嵌入到编译文件里的一种方式，出现如上的错误可能是写错参数导致的，如下例
```
[Embed(source="../media/graphics/bgWelcome.jpg")]
public static const BgWelcoe:Class;

......


  var bitmap:Bitmap = new Assets[name]();
```
要检查 new 后面的`Assets[name]`的值与BgWelcoe是否相同。我这里的BgWelcoe写错了，但使用时传入了BgWelcome，就出现这个错误提示。



5. 深入理解Flash的沙箱-Security Domains

http://kevincao.com/2010/11/security-domains/



6. Vector、Array、Object和Dictionary的简单区别

附上初始化和访问方法。

* （1）Vector是通过索引访问的密集数组，存放元素类型均相同的多种数据类型。效率比Array高。
```
var vec:Vector.<int> = new Vector.<int>();
var vec:Vector.<int> = new Vector.<int>[1,2,3];
```
* （2）Array是通过索引访问的稀疏数组，可存放数字、字符串、对象其他数组等多种数据类型。（也可以做为关联数组，但不提倡）
```
var b:Object = new Object();
var arr:Array = new Array("a",b,3);
var arr:Array = ["a",b,3];
```
* （3）Object是关联数组，键为字符串，值可多种数据类型,可动态赋予属性。
```
var obj:Object = {name:"John",age:10};
obj.sex = "男";
trace(obj.name);//John
trace(obj["sex"]);//男
```
* （4）Dictionary也是关联数组，但可将对象作为键，值也可多种数据类型,可动态赋予属性。用delete删除键，引用有“强”“弱”之分。
```
var obj:Object = new Object();
var dic:Dictionary = new Dictionary();
dic[obj] = "abc";
dic.abc = "ABC";
trace(dic.abc);//ABC
trace(dic["abc"]);//ABC
```


7. for...in 与 for each...in的区别

* （1）for...in 读取的是键，在Dictionary中读取对象键，Object中读取键名，Array和XML等读取索引。

* （2）for each...in 读取的是值，不管是Dictionary、Object、Array中都读取对应的值。



8. localX、mouseX和stageX的区别

  localX 和stageX 是 MouseEvent 类的属性；mouseX 是 DisplayObject 类的属性

  localX 和mouseX 都可以获得鼠标在显示对象上的局部坐标。

* （1）localX是在点击事件触发时通过MouseEvent的实例获得的，获得的是事件流中鼠标相对在目标阶段中的对象的坐标。

* （2）mouseX是随时都可以通过DisplayObject类或其子类的实例获得，获得的是鼠标在显示对象本身上的坐标。

* （3）stageX用来获取鼠标在舞台上的坐标。



9. target 和 currentTarget的区别

  * （1）target指向的是事件流中事件经过的目标对象

  * （2）currentTarget指向的是使用侦听器的那个对象

两者在显示对象的事件流中才有区别，非显示对象和事件经过使用侦听的那个对象时指向的是同一个对象。