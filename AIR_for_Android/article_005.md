# ADOBE AIR移动应用中的文本输入
*   语言：ActionScript 3.0
*   环境：Adobe AIR for Mobile
*   作者：[@flashache](http://weibo.com/ias3 "@flashache")
*   简介： flash.text.TextField设置type为TextFieldType.INPUT作为flash的文本输入，是大部分flash开发者们都非常熟悉的方法了。如果是一个flash开发者老人，可能还会知道一些关于文本输入的稀奇古怪，积累了很多年，想到就头疼的bug，例如web应用设置嵌入模式(wmode)为透明(transparent)的情况下在火狐浏览器下无法正确输入中文这种让人想拍桌子骂*的问题。所以每次我碰到关于flash的文本输总是战战兢兢的，哈哈。言归正传，其实最近笔者在开发一款基于Adobe AIR技术的iOS客户端，所以免不了略复杂的文本输入这个环节。而且由于客户端的文本输入一般都不是简单输入单行文字，所以笔者在这块上面也是花了一些功夫，下面给大家总结一些积累的经验和收集到的资源，以供参考。

### 使用flash.text.TextField

在AIR3发布之前，我们可以使用TextField作为在移动设备上的文本输入，设置TextFieldType.INPUT,这个和普通的flash开发没什么区别。这块的代码我也就不在这写了，大家可以参考线上关于TextField的帮助：[http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/TextField.html](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/TextField.html "flash.text.TextField")。

### 移动端TextField的输入原理

在移动设备上例如iOS，使用TextField作为文本输入，Adobe AIR处理方式是：在用户输入的时候，调用系统原生的输入代替flash的文本输入，当结束输入即移出输入框的时候将输入的内容在flash中渲染出来。

### 移动端TextField使用常见问题

虽然总说flash处理文本很弱，但是在这么多弱的文本处理里面，TextField其实还是可以做很多事情的，例如作为动态文本，静态文本、输入文本、支持html渲染，支持密码的方式渲染（displayAsPassword）。在样式方面除了通过属性设置例如文本颜色（textColor）也可以使用flash.text.TextFormat对样式进行设置，例如行间距（要命的行间距啊！，一会儿会提到这个[:D]）、文字间隔、是否粗体等等。在了解了上述关于TextFied在移动端的输入原理之后，你就明白，因为你的输入和显示的并不是同一个文本输入框，如果原生的并不支持这个特性的话，就会出现这种问题：输入的时候和显示的对不上。最最明显的就是刚才提到的那个行间距，例如我们设置TextFormat的leading为50，这时候你会发现，输入的时候，这个至没有生效，整个段落变矮了，输入完成之后渲染的时候生效了。知道原理之后就不会觉得奇怪了。由于线上帮助也并没有标识出来，TextField的哪些特性是原生输入支持的，哪些不支持，所以在使用的时候相对就有点麻烦，而且，如果那个特性不支持，又设计到你的产品里面了（例如我这次碰到的，哈哈），那就只有修改下设计，做一些妥协了。 内容参考：[<span class="Apple-converted-space"> </span>http://forums.adobe.com/message/3977807](http://forums.adobe.com/message/3977807 "[AIR for iOS] TextField + emoji problems")

### 推荐使用flash.text.StageText

都听过Stage3D，StageVideo，说道这个在AIR3引入的StageText可不是每个人都有了解和使用过。其实StageText就是为了解决文本渲染的一些问题。

### StageText的输入原理

StageText的原理实际上是Adobe AIR对原生的文本进行一次封装，将原生的文本渲染提供给AIR应用使用。所以文本输入的各种特性将和原生的文本输入框一模一样（因为只是包了一层壳）。

### StageText的特性与优点

关于特性方面，大家可以仔细参考下线上的帮助：
[http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/StageText.html](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/StageText.html "flash.text.StageText")
比较实用的特性例如：

*   控制软键盘的外观，常见的可以设置为SoftKeyboardType.NUMBER 在软键盘上仅显示数字等等。

*   使用到原生输入的自动校正

*   设置软键盘的“return”为“DEFAULT,DONE,GO,NEXT以及SEARCH”（设置属性ReturnKeyLabel）

### StageText的使用注意点

看文档会发现StageText不是继承于DisplayObject，继承关系是：StageText -> EventDispatcher -> Object，所以不能被添加到传统的显示列表上，而且总是显示在最上面。所以也不推荐多个StageText相互叠加显示。属性设置上面也稍稍和传统的显示对象不太一样，所以可能使用上有些不太习惯。

### Adobe提供的针对StageText二次封装NativeText

其实代码很少，只是包装一层使得使用起来更方便写，这块具体也不再多言了，git地址：[https://github.com/cantrell/StageTextExample](https://github.com/cantrell/StageTextExample "StageTextExample")
上述部分内容参考（包括NativeText的使用）：
[http://blogs.adobe.com/cantrell/archives/2011/09/native-text-input-with-stagetext.html](http://blogs.adobe.com/cantrell/archives/2011/09/native-text-input-with-stagetext.html "Native Text Input with StageText")

### 移动软键盘的自动聚焦（softKeyboardBehavior）

移动应用输入框有一个自动的特性，就是自动会聚焦到显示区域的中间，当然键盘弹出的时候。如下图所示（基于Starling框架的UI框架Feather，提供的Demo:ComponentsExplorer中的文本输入）PS:如果大家去看一下代码，Feather使用的文本输入是基于flash.text.TextField的。

在移动应用的配置文件***-app.xml里面有个节点：softKeyboardBehavior，就可以控制这个行为，是否在文本输入的时候聚焦到显示区域的中间，默认是开启的。

### 自制输入面板和软键盘的定位

很多情况下，我们会自己开发一些输入，例如客户端常用到的“表情输入面板”，还有“图片选择”等等，如下图所示：

这些输入面板需要依赖于自身开发的UI，并且和软键盘有一定的关系，需要相互切换，并且位置方面也需要精确定位软键盘的位置。我们需要做的是：

1.  设置softKeyboardBehavior为none

2.  获得软键盘的高度

第一个在配置的xml里面设置即可，第二个实际上就是对文本监听SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATE，读取stage的softKeyboardRect即可。

`注：`

1. 在主程序入口添加属性`resizeForSoftKeyboard="false"`防止软键盘导致界面变形

2. 在移动应用的配置文件`***-app.xml`里面有个节点：`softKeyboardBehavior`，就可以控制这个行为，是否在文本输入的时候聚焦到显示区域的中间，默认是开启的。设置`softKeyboardBehavior`为`none`，在配置的`xml`里面设置即可，对文本监听`SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATE`，读取`stage`的`softKeyboardRect`即可。
软键盘对应不同手机其高度为
`this.stage.softKeyboardRect.height*PersonMobile.getInstance().applicationDPI/Capabilities.screenDPI`
`PersonMobile.getInstance()`为项目入口的单例

3.焦点处理

把要获取焦点的其他组件名赋予`stage.focus`，或者设置`stage.focus = null`, 会导致软件盘关闭;但要注意设置为`null`时，会导致键盘返回键失效，需重新设置焦点。