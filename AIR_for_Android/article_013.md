# ANEBUILDER 4 ANDROID – 一个自动创建ANDROID ANE的工具
当我们使用Flash和Adobe AIR打包技术开发移动应用的时候，ANE（Adobe AIR本地扩展）也是我们经常会用到的一个技术方案。为什么呢？因为Adobe AIR肯定不会在核心库中满足所有开发者的需求（体积和性能考虑），那么很多目前AIR核心库不具备的功能（比如网络监测，广告显示，内付费等等），我们都要用ANE来实现。

不过ANE对很多一直处于Flash领域的开发者来说困难多多，不仅仅是了解和学习Native Code（Java，Objective C等），ANE繁琐的创建和配置方式也让我们感到无法下手。

语言方面就不多说了，今天这里推荐的一个Windows下可用的，自动创建ANE的工具（for Android）。

原文地址：

http://flashvisions.com/air/anebuilder-4-android-a-tool-to-automate-ane-creation-for-android-on-windows/

大意：

我花了整个一星期的时间来创建Adobe AIR本地扩展，在经历了种种失败之后，我创建了这个工具，可以让您更容易创建出ANE（期待Flash Builder自己实现ANE的打包功能）。

如果你看过ADC的一篇教程： http://www.adobe.com/devnet/air/articles/developing-native-extensions-air.html ，你就会发现这篇教程里面关于创建本机代码和ActionScript库的过程描述的不是很清楚。有一些地方是需要注意的，比如从swc里面抽取library.swf，把它们放置到正确的位置。而且你还要配置你的批处理文件，用ADT命令行去打包（还要注意参数的顺序）。稍有不慎，你就会经历我经历过的各种失败。

不管怎么说，你可以看上面的那篇教程，学习基本的步骤，然后你可以用Java创建本地代码和ActionScript类库，然后你就可以用这个工具来生成ANE文件。它不是一个完善的软件，但它可以把你从繁琐的过程中解放出来。目前只可用于Windows，因为我没有Mac机。但我希望我能尽快推出Mac版。

下载地址：

[ANEBUILDER 4 Android (windows).zip](http://flashvisions.com/wp-content/uploads/2012/06/ANEBuilder-windows.zip)

注意事项：

需要签名，需要.p12证书
记得保存项目，免得下次还要重新输入参数