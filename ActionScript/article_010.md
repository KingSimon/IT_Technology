# flash 读取xml缓存问题
lash读取xml文件之后，flash和xml文件都被保存到了本地的IE缓存里面，这样一来，服务器上xml文件更新了，但是浏览者的读取却不能同步。怎么解决这个问题呢？

这个是缓存的问题，一般都碰到过，有时对新手可能比较困惑，其实解决也很简单，只要你让flash每次访问服务器的URL不一样就行。
那该如何操作呢？一般给服务器的URL后面加一个随机数参数，这样每次访问的URL都是新的，flash就不会读缓存了，而且随机数参数在服务器上不用处理，不会有任何影响。
具体如下：
比如这是你要访问的服务器上XML的URL：as3.xml
你给它后面加一个随机参数：Math.random()
具体代码：
```
var serverUrl:String="as3.xml?R="+Math.random();
var xml:XML=newXML();
System.useCodepage=true;
xml.ignoreWhite=true;
xml.load（serverUrl:String);
```