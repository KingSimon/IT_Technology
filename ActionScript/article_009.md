# .Net 实现将汉字转拼音
### 1、AS3的强制类型转换

我之前一直以为是没有这个功能的，而最近在看一些文档，发现这个功能是有的。目前总结下来有两种写法：

1. var loader:URLLoader = URLLoader(event.target);
不知道这种写法是借鉴那种编程语言，反正我没有见过。
补充这个语法说明：http://livedocs.adobe.com/flash/8_cn/main/00001211.html
var loader:URLLoader = event.target as URLLoader;
这种写法是借鉴VB.Net编程语言，唉！AS3语法真得一个大杂烩。
2. URLStream和URLLoader

URLStream在文本文件数据方面可以支持多种字符编码。
URLLoader在进过测试后，发现除UTF-8字符编码外，其他字符编码读过来的数据都是乱码。

可通过ByteArray对象字符编码转换(感谢abc12hjc提供补充代码)：

```java
public class MoxieAS extends Sprite
{
	public function MoxieAS ()
	{
		var stream:URLStream = new URLStream;

		stream.addEventListener(Event.COMPLETE, complete);
		stream.load(new URLRequest("ttt.xml")); // <root><a /><b /><c /></root>
	}

	private function complete (event:Event):void
	{
		var stream:URLStream = event.target as URLStream;
		var xml:XML = XML(stream.readUTFBytes(stream.bytesAvailable));

		trace(xml.*.length()); // output 3
	}
}
```

具体用那个方式就看实际功能需求了。

3. URLRequest

URLStream、URLLoader、Loader等读取外部数据的类，里的load方法参数值是URLRequest对象。我在编程过程总是喜欢直接写地址字符串，原来习惯改过来看样子要花点时间了。

4. Loader

在使用Loader来加载数据时，添加侦听事件时，注重一定要给Loader的 contentLoaderInfo属性增加事件，而不是给Loader对象增加事件。我就是因为加错对象（要害是编译时还没有报错），郁闷得差点要把电脑给扔了。

错误写法：

```
var loader:Loader = new Loader();
loader.addEventListener(Event.COMPLETE, completeHandler);
loader.addEventListener(SecurityErrorEvent.SECURITY_ERROR, securityErrorHandler);
loader.addEventListener(IOErrorEvent.IO_ERROR, ioErrorHandler);
```

正确写法：

```
var loader:Loader = new Loader();
loader.contentLoaderInfo.addEventListener(Event.COMPLETE, completeHandler);
loader.contentLoaderInfo.addEventListener(SecurityErrorEvent.SECURITY_ERROR, securityErrorHandler);
loader.contentLoaderInfo.addEventListener(IOErrorEvent.IO_ERROR, ioErrorHandler);
```


1) Bitmap  swf加载进来SWF转换成一个Bitmap
```
private function complete(evt:Event):void
{
	var loaderInfor:LoaderInfo=evt.currentTarget as LoaderInfo;
	if (loaderInfor != null)
	{
		var loader:Loader=loaderInfor.loader;
		if (loader != null && loader.content != null)
		{
			var bmpData:BitmapData=new BitmapData(loader.width,loader.height,true,0);
			bmpData.draw(loader,null,null,null,null,true);
			if(_roleIcons[loader.name].bitmapData)
				_roleIcons[loader.name].bitmapData.dispose();
			_roleIcons[loader.name].bitmapData=bmpData;
			_roleIcons[loader.name].width = obj.width;
			_roleIcons[loader.name].height = obj.height;
			delete _roleIcons[loader.name];
		}
	}
	beginLoadIcon();
}
```
```
//加载成功，发布成功事件
private function completeFun(e:Event):void
{
	data = loader.content["bitmapData"];
	delEvent();
	dispatchEvent(e);
}
```

2) MovieClip  加载SWF里面的元素【MC、Bitmap】
```
var equip:Class=_loader.contentLoaderInfo.applicationDomain.getDefinition("equipment") as Class;
if(equip)
{
	_equipIcons=new Bitmap(new equip(0,0));
	updateGoodsIcon(_notLoadEquip);
	_loader.contentLoaderInfo.removeEventListener(Event.COMPLETE,loadEquipComplete);
	_notLoadEquip = null;
	_loader.unload();
	next();
}
```
5. Loader加载过来的数据类型

大家知道Loader是用来代替原来 MovieClip的loadMovie功能，用于加载外部的图片文件，SWF文件。
假如加载图片文件（jpg，gif，png等）时，Loader.content得到数据类型是Bitmap对象；
假如加载SWF文件（flash 9 版本）时，Loader.content得到数据类型是MovieClip对象；
假如加载SWF文件（flash 9 以前版本） 时， Loader.content得到数据类型是AVM1Movie对象；

6. stage

在调试flash过程发现，假如把swf文件放到html页面后，stage.stageWidth和stage.stageHeight在第一次加载调用时，他们的值为空值；

7. AVM1Movie

假如是AVM1Movie 对象时，就不能直接调用stop，play，gotoAndStop等原来MovieClip对象的功能了，而且不能将AVM1Movie 对象转换成MovieClip对象。目前解决办法：一种是用flash cs3 重新生成 flash 9的swf文件；另一种是国外网站有说能AVM1和AVM2两个虚拟机相互调用的方式（贴一个地址）；

8. mask

在使用遮罩功能，发现一个问题，假如不把用于遮罩的显示元件通过addChild方法添加到同一级的显示容器里的话，遮罩效果就显示不正常，不知道这个是不是一个bug。我差点因为这个问题而放弃AS3改用AS2了。