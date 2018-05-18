# ActionScript的规范命名
1. 限制使用缩写词,公认的缩写除外，如comp代码代表组件
2. 变量、方法、实例或Fla文件名用小写骆驼法命名。即多个单词组成的变量或者函数，第一个单词的首字母小写，其余单词的首字母大写，例如getByteTotal()。
3. 类、构造函数或AS类文件用大写骆驼法命名。即每个单词的头一个字母均大写。如DisplayObject。
4. 接口名称以大写字母“I”开头，如IBitmapDrawable。
5. 一定要使用有准确描述性意义的变量名称，使程序望文生义。如getChildByName()。
6. 私有属性的变量以下划线开头，如private var _city:Map。（属性city的返回值）
7. 局部变量（方法里的变量）可用简单与类型有关的字符或单词，如var s:Sprite;。普通方法的形参用$加简单单词，如（$name,$sex）。处理事件的方法形参用e。
8. 尽可能保持所有名称最短。
9. 避免使用保留字和语言构造。
10. 不要使用大小写不同的相同变量名称。例如不要同时使用name和Name。
11. 在循环中将单字符变量作为临时变量（例如 i、j、k、m和n）。
12. 常量应为大写字母，分隔单词应包含下划线。如ENTER_FRAME。
13. 布尔变量应以字母“is”开头，如isRun。
14. 包使用“反向域”命名，例如，用com.adobe命名adobe.com，而且均为小写，不使用大小骆驼法。
15. 处理监听事件的调用的方法用handle或on开头，如handleLoaderComplete和onClick。
16. 私有变量用m加名词开头，如mName、mX（当你在其他地方要调用却忘词的时候，直接输入m然后智能提示，比使用类型简写开头更快，因为也有忘类型的情况）。

### 命名
在编写程序的时候，选择一个易读、方便的好名字是非常关键的。你需要经常考虑一下自己的命名是否恰当，特别是会不会和已有API冲突。
我们的命名规则基本和ECMAScript与Flash Player 9一致。
### 缩写
缩写也不一定就是好事，比如说calculateOptimalValue()，这个方法名就比calcOptVal()要好。通常来说，语义清楚比少敲几次键盘更加重要。如果你滥用缩写，将会使代码的可读性降低。
当然，我们也有一些标准的缩写形式：
* acc 代替 accessibility, 比如 ButtonAccImpl
* auto代替automatic, 比如autoLayout
* auto代替automatic, 比如autoLayout
* eval代替evaluate, 比如EvalBindingResponder
* impl代替implementation, 比如ButtonAccImpl
* info代替information, 比如GridRowInfo
* num代替number of, 比如numChildren
* min代替minimum, 比如minWidth
* max代替maximum, 比如maxHeight
* nav代替navigation, 比如NavBar
* regexp代替regular expression, 比如RegExpValidator
* util代替utility, 比如StringUtil

这里只是列举了一些常用的缩写。如果这里没有你想用到的缩写，请你多加注意大多数人的写法。这样还没有找到的话，那你就要慎重考虑到底要不要使用缩写了。
当然缩写的形式也是多种多样的。举个例子，我们经常使用“horizontal”和“vertical”，如horizontalScrollPolicy、verticalScrollPolicy。但是我们经常把他缩写成普遍认可的H和V。
### 首字母缩略词
变量的首字母缩略写法在Flex中是非常普遍的, 比如 AIR, CSS, HLOC, IME, MX, MXML, RPC, RSL, SWF, UI, UID, URL, WSDL, XML。
首字母缩略词中要么都是大写字母，要么都是小写字母，例如SWF 或者 swf是合法的，但Swf是不行的。只有两种情况缩略词全用小写字母，一种是单独作为标识符，另一种是作为标识符的前缀，而且标识符必须以小写字母开头。
这里有一些缩略词的例子： CSSStyleDeclaration, IUID, uid, IIME, imeMode。

### 单词边界
当标识符是复合词时，即包含了多个单词，我们一般使用两种方法来划分单词的边界。一种用大写字母来划分的骆驼命名法如LayoutManager 、 measuredWidth；一种用下划线来划分如object_proxy。
但是有时候，对于一个本身的复合词，我们无法分清其边界，如dropdown, popUp, pulldown。
当然，也会有两个首字母缩略词不得不连在一块儿的情况，如loadCSSURL()，我们应该尽量避免这样的情况。

### 表明类型的名称
如果你想要将对象的类型包含到名称中，应该将它放在最后。但是不要使用ActionScript1和2的那些惯例：将类型的缩写如“_mc”作为后缀加在后面。举个例子，当我们为一个边框的图形命名，可以命名为“border”、“ borderSkin” 或者 “borderShape”， 千万不要用“border_mc”。

### 包名
开头字母必须小写，以后的单词第一个字母大写，用骆驼命名法如controls, listClasses。
包名必须应该是名词或者动名词，千万不要用动词，形容词或副词。
由于一个包通常包含了很多相似的类，所以名字也通常是复数形式的，如charts, collections, containers, controls, effects, events, formatters, managers, preloaders, resources, skins, states, styles, utils, validators。
我们通常对包命名使用一种表达行为概念的动名词如binding, logging, messaging, printing。或者我们用一些概念的名词，如accessibility, core, graphics, rpc。
一个包如果包含了一些支持同一组件（如FooBar）的类，那么其包名应该是fooBarClasses。

### 文件名
对于大多数类文件，其文件名都要与类名相同，但是对于包含文件不必这样。
名称以小写字母开头，用骆驼命名法并且在后面加上修饰的单词如Style，则名称可以为BorderStyles.as, ModalTransparencyStyles.as。

### 命名空间的命名
用小写字母开头并且用下划线区分单词。如mx_internal、object_proxy。

### 接口的命名
以大写字母“I”开头，并且用骆驼命名法.如IList、 IFocusManager、IUID。
### 类的命名
以大写字母开头，并且用骆驼命名法。如Button、 FocusManager、UIComponent。
命名 Event 的子类如 FooBarEvent。
命名Error 的子类如 FooBarError。
命名Formatter 的子类如 FooBarFormatter。
命名 Validator 的子类如 FooBarValidator。
一些外观类如FooBarBackground, FooBarBorder, FooBarSkin, FooBarIcon, FooBarIndicator, FooBarSeparator, FooBarCursor 等
命名应用类如 FooBarUtil (不要用 FooBarUtils包是复数而类是单数)。
命名基类时通常这样写：FooBarBase: ComboBase, DateBase, DataGridBase, ListBase。
### 事件的命名
用小写字母开头，并且用骆驼命名法。如“move”、“creationComplete”。
### 样式的命名
用小写字母开头，并且用骆驼命名法。如“color”、 “fontSize”。
### 枚举值和字符串
用小写字母开头，并且用骆驼命名法。如“auto”、“filesOnly”。
### 常量的命名
必须全部大写并且以下划线来区分单词。如OFF, DEFAULT_WIDTH。
标志符的单词必须与其对应的常量相符合，如
public static const FOO_BAR:String = "fooBar";
### 属性 (变量) 命名
用小写字母开头，并且用骆驼命名法。如i, width, numChildren。
双重循环的情况下，我们通常用i作为外层的循环变量，n作为其上限。而用j作为内层循环变量，m作为其上限。
如：
```
for (var i:int = 0; i < n; i++)
{
for (var j:int = 0; j < m; j++)
{
...
}
}
```
用p作为for-in循环的循环变量。
如：
```
for (var p:String in o)
{
...
}
```
如果一个类重写了存取器（getter/setter），并且还想调用基类的存取器（getter/setter），这时应该将指定的属性前面加“$”，并且这个存取器（getter/setter）用final来修饰，里面仅仅调用父类的存取器（getter/setter）。
如：
```
mx_internal final function get $numChildren():int
{
return super.numChildren;
}
```
### 存储变量的命名
给拥有存取器getter/setter的存储变量前面加“_”，如将foo命名为_foo。
### 方法的命名
用小写字母开头，并且用骆驼命名法如measure(), updateDisplayList()，方法的名称总是动词或者动宾词。
方法尽量不要命名成getFooBar() 或者 setFooBar()，这些应该是存取器（getter/setters）方法。如果getFooBar()方法是一组复杂的计算，我们可以命名为findFooBar(), calculateFooBar(), determineFooBar()等。
如果一个类重写了父类的方法并且还想调用父类的方法，这时应该在指定的方法前加“$”。将此方法用final修饰，并且仅在其中调用父类的方法。如：
```
mx_internal final function $addChild(child: DisplayObject): DisplayObject
{
return super.addChild(child);
}
```
### 事件响应函数的命名
在事件名称的后面加“Handler”即构成事件响应函数的名称如：mouseDownHandler()。
如果事件的发送者是类中的成员，则在名称前加成员的名称和下划线“_”如textInput_focusInHandler()。
### 参数的命名
把每一个setter的参数都命名为“value”，不用lab，labelValue或者val。
public function set label(value:String):void
将事件响应函数的参数命名为“event”不能用e,evt或者eventObj。
protected function mouseDownHandler(event:Event):void