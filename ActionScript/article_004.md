# flash as3 改变鼠标悬停图标
在flash as3制作中的时候,我们常常遇到这样的需求,就是让鼠标变成手形.比如,你希望用户把鼠标移到影片按钮上的时候,鼠标变手形;你希望鼠标一直是一个手形,而不是箭头.下面我们就来研究一下:

1. 首先要了解了一上flash.ui.Mouse,在as3帮助手册里看一下,这个类都有什么方法和属性.我们发现有Mouse.cursor这个属性和Mouse.show() Mouse.hide()两个方法
于是找到了答案,让用户的鼠标一直是手型的代码:
Mouse.cursor=MouseCursor.HAND
另外,我继续深入,还发现可以把鼠标变成各种各样的形状:
Mouse.cursor=MouseCursor.ARROW//用于指定应使用箭头光标。
Mouse.cursor=MouseCursor.AUTO//用于指定应根据鼠标下的对象自动选择光标。
Mouse.cursor=MouseCursor.BUTTON//用于指定应使用按压按钮的手形光标。
Mouse.cursor=MouseCursor.HAND//用于指定应使用拖动手形光标。
Mouse.cursor=MouseCursor.IBEAM//用于指定应使用工字形光标

2. 希望影片剪辑在鼠标上移的时候,鼠标变手型怎么办?我们再查下一flash.display.Sprite这个帮助说明,发现它有个属性是:buttonMode -指定此 sprite 的按钮模式。因此,如果你想让一个影片或sprite当成按钮,你可能要设置一下.