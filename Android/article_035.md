# Android的虚拟机Dalvik 介绍
Dalvik和标准Java虚拟机（JVM）之间的首要差别之一，就是Dalvik基于寄存器，而JVM基于栈。一直以来都有人在猜测，选择基于寄存器的方式是因为它对提前优化（ahead-of-time optimization）提供了更好的支持，而这对类似于移动电话这样的受限环境是颇有裨益的。另一份针对基于寄存器虚拟机和基于栈虚拟机更深入的比较分析指出，基于寄存器的虚拟机对于更大的程序来说，在它们编译的时候，花费的时间更短。

　　Dalvik和Java之间的另外一大区别就是运行环境——Dalvik经过优化，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个 Dalvik应用作为一个独立的Linux进程执行。Neil Bartlett指出，给每一个应用赋予独立的进程可以允许动态安装、激活和去激活，但是他对Dalvik为什么要选择这种方式而没有使用OSGi在单一进程中实现表示疑问——Radoslav Gerganov回复说，独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。Carl Rosenberger也指出OSGi也可以被移植到Android平台，而Jilles van Gurp对Google为何选择重新实现若干组件，如跨进程通信，表示疑问。

　　此外，Java也已经不再是人们在Dalvik上开发所选择的唯一语言了——已经有人在Dalvik上运行Scala取得了成功，并且Hecl也已经被成功移植了。另外更有人对运行Groovy做了一次尝试，不过目前为止还不怎么成功。Mono项目的创始人Miguel de Icaza也对在Dalvik源码公开之后将Mono整合到Dalvik上表示了兴趣，而且也已经有人猜测如何用多种方式来实现整合了，包括与随Android SDK提供的Java到Dalvik重编译器类似的CIL（Common Intermediate Language，通用中间语言）到Dalvik重编译器。

　　Dalvik的诞生也导致人们开始忧虑Java平台的第一次大规模的分道扬镳或许已经是进行时了——有人已经把Davlik和微软的JVM以及Sun 对微软的诉讼联系起来，等着看Google身上是否也会发生类似事情；另外一些人则指出，Google并没有宣称Dalvik是一个Java实现，而微软却是这样做的。Sun也对可能带来的阵营分裂表达了忧虑情绪，并提出和Google合作来保证Dalvik和JVM之间的兼容性——Google对此的解释是，Dalvik是对解决目前Java ME平台上分裂的一次尝试，也是为了提供一个拥有较少限制许可证的平台。甚至还有人怀疑这是否是Sun和Google两大阵营对Java之未来的一次大规模较量。Ian Skerret认为，Dalvik的诞生是对Sun尝试控制和保护来自Java ME收入来源的一次反应，以及对建立OpenJDK统辖理事会迟迟未果的回答。这也导致Dalibor Topic怀疑Google是否要重履Sun走过的路：

　　当然，一个很有意思的问题是，为什么没人有勇气拿Google关于OpenJDK的问题反过来问Google呢？

　　虽然Android号称开源，但它仍是专有产品。Android做过兼容性保证，是在秘密会议室中签署和保管的。Android不具备任何治理模型，也没有证据指出将来会出现治理模型。Android没有规范，并且它的许可证禁止任何替代实现的开发，因为这并非Google在SDK许可证中授权许可的使用权。Android完全在Google的掌控之下，一旦有竞争性应用在财政上损害了Google的利益，Google是保有一刀抹杀这些应用的权利的。从设计伊始，Android就收到限制，只能在Google的财务利益允许的条件内开放。专有的Java也不是什么好货色，旧瓶装新酒而已。

　　这就好像我们在见证JCP的重生一样，人们排着队把开源社区的“街头信誉”在一个单一的、专有的实现的基础上借给另外一个封闭的厂商垄断集团。只不过这次的大头改姓Google，而不是Sun了。
Stefano Mazzocchi发布了一篇分析报告，深切入里地探讨了围绕Java ME和Dalvik的许可证问题，他得出结论说，Dalvik的市场定位良好，足以给移动电话市场带来冲击。尽管Google一直都很小心避免引起诉讼的几个关键点，但Mazzocchi相信Sun还是会起草知识产权案的状告书（IBM也有可能）。他还指出，由于在JCP之外操作，Google可以非常快地对Android进行更改，而且可以避开Sun对任何JCP更动的否决权——这样他们也可以为诸如USB和蓝牙这样的组件加入接口，而这些组件在基础Java ME实现中是不可用的。最后，通过在Apache许可证下授权许可Dalvik的源码，移动电话运营商更有可能采用Dalvik，因为运营商可以在不花费许可费用的情况下使用和修改它。