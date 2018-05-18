# AS3中的原型继承介绍
原型继承是早起AS版本所使用的集成方式，在AS3中允许两种方式的集成——类继承和原型继承

### 原型继承的基本原理:
每种类都有一个关联的原型对象，而原型对象的属性由该类的所有实例共享。 在创建一个类实例时，它具有对其类的原型对象的引用，这将作为实例及与其关联的类原型对象间的链接。 运行时，如果在类实例中找不到某属性，则会检查委托（该类的原型对象）中是否有该属性。 如果原型对象中不包含该属性，该过程会继续在层次结构中连续的更高级别上对原型对象进行委托检查，直至 Flash Player 找到该属性为止。


看一段代码：
```
Sprite.prototype.copyright="www.iflashigame.com";

var sp:Sprite=new Sprite;
trace((sp as Object).copyright);    //输出www.iflashigame.com
```
如果不用类型声明也可以写成这样：
```
Sprite.prototype.copyright="www.iflashigame.com";

var sp=new Sprite;
trace(sp.copyright);
```
需要注意的是如果是非动态类那么为类原型添加的属性会成为一个只读属性，例如:
```
Sprite.prototype.copyright="空";

var sp=new Sprite;
var mc=new MovieClip;

trace(sp.copyright);    //输出 "空"
trace(mc.copyright);   //输出 “空",由于MovieClip继承自Sprite,所以也能得到属性

sp.copyright="www.iflashigame.com";      //报错了，无法为 flash.display.Sprite 创建属性 copyright。
mc.copyright="www.iflashigame.com";    //新的值将覆盖继承过来的

trace(sp.copyright);         //无法赋值，还是输出 "空";
trace(mc.copyright);        //输出"www.iflashigame.com";
trace(Sprite.prototype.copyright);     //没有改变，还是输出 ”空"
```
了解了原型继承原理，可以帮助我们在某些特殊场合应付一些特殊的需求。

比如最近在一个游戏设计需求中就可以用这种办法解决，看这篇文章。