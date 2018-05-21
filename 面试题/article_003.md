# .NET 面试题总结 第3部分
#### Global.asax 文件中配置自定义的错误？
问：您要创建ASP.NET应用程序用于运行公司内部的Web站点，这个应用程序包含了50个页面。您想要配置这个

应用程序以便当发生一个HTTP代码错误时它可以显示一个自定义的错误页面给用户。您想要花最小的代价完成这

些目标，您应该怎么做？
答：在这个应用程序的 Global.asax 文件中创建一个Application_Error过程去处理ASP.NET代码错误；

#### 在DataGrid控件的Footer显示员工合计数？
问：您要创建一个显示公司员工列表的应用程序。您使用一个DataGrid控件显示员工的列表。您打算修改这个控件

以便在这个Grid的Footer显示员工合计数。请问您应该怎么做？
答：重写OnItemCreated事件，当Grid的Footer行被创建时显示合计数；

#### 如何在这个程序中使用Web Service？
问：您为公司创建了一个ASP.NET应用程序。这个应用程序调用一个 XML Web Service，这个 XML Web Service

将返回一个包含了公司雇员列表的DataSet对象。请问您该如何在这个程序中使用这个 XML Web Service？
答：在“Web Reference”对话框中输入这个 XML Web Service的地址；

#### 修改程序？
问：您要创建一个ASP.NET应用程序在 DataGrid 控件中显示一个经过排序的列表。产品数据被存放于一个名为

PubBase的 Microsoft SQL Server 数据库。每个产品的主键是 ProductID，Numeric型并且每个产品有一个字母描

述字段，名为ProductName。您使用一个SqlDataAdapter对象和一个SqlCommand对象通过调用一个存储过程从

数据库中获取产品数据。您将SqlCommand对象的CommandType属性设置为CommandType。

StoredProcedure，并将它的CommandText属性设置为procProductList。您成功的获取了一个DataTable对象，

其中是已经按ProductID降序排列的产品列表。您打算显示以相反的字母顺序排列的ProductName，请问该怎么做

？
答：将SqlCommand对象的CommandType属性修改为CommandType.Text，将CommandText属性修改

为”SELECT * FROM procProductList ORDER BY ProductName DESC”。然后将这个DataTable对象绑定到

DataGrid控件；

#### error 和 exception 有什么区别？

error 表示恢复不是不可能但很困难的情况下的一种严重问题。比如说内存溢出，不可能指望程序能处理这样的情

况；

exception 表示一种设计或实现问题。也就是说，它表示如果程序运行正常，从不会发生的情况；

#### 要用新浪网上的天气预报，怎么办？
用 Web Service，请求国家气象局的接口。用AJAX把新浪网上的网页请求过来，然后解析一下；

#### 母板页与内容页中怎么传参(除Session、Cookie、Application、QuerySting、Server.Transfer之外)？
获取母版页上文本框的值赋给内容页的文本框 `TextBox1.Text = (Master.FindControl("textBox1") as TextBox).Text;`
获取内容页上文本框的值赋给母版页的文本框
`TextBox1.Text = (ContentPlaceHolder1.FindControl("textBox1") as TextBox).Text;`

#### 两根分布不均匀的香，全部烧完需1个小时，如何确定一段15分钟的时间？
同时点燃第一根的两端和第二根的一端，第一根烧完是半个小时。这时点燃第二根的另一端并开始计时，全部烧完

就是15分钟；

#### XML文档定义的形式？它们的本质区别？解析XML文档的方式？
1、 两种形式 DTD、Schema；
2、 本质区别：Schema本身是XML的，可以被XML解析器解析(这也是从DTD上发展schema的根本目的)；
3、 有DOM、SAX、STAX等；

DOM：处理大型文件时其性能下降的非常厉害。这个问题是由DOM的树结构所造成的，这种结构占用的内存较多

，而且DOM必须在解析文件之前把整个文档装入内存，适合对XML的随机访问；

SAX：不限于DOM，SAX是事件驱动型的XML解析方式。它顺序读取XML文件，不需要一次全部装载整个文件。当

遇到像文件开头、文档结束或者标签开头与标签结束时，它会触发一个事件，用户通过在其回调事件中写入处理代

码来处理XML文件，适合对XML的顺序访问；

STAX：Streaming API for XML(流的API的XML)；

#### 在WinForm中如何取消一个窗体的关闭？
```
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{e.Cancel = true;}
```
#### 在WinForm中，Appplication.Exit()和Form.Close()有什么不同？
一个是退出整个应用程序，一个是关闭其中一个form；

#### “62-63=1”等式不成立，请移动一个数字(不可以移动减号和等于号)，使得等式成立，如何移动？
62移动成2的6次方；

#### Anonymous Inner Class (匿名内部类) 是否可以 extends (继承)其它类，是否可以 implements (实现) interface

(接口)？
不能，可以实现接口；

#### Static Nested Class 和 Inner Class的不同？
Static Nested Class是被声明为静态(static)的内部类，它可以不依赖于外部类实例被实例化。而通常的内部类需

要在外部类实例化后才能实例化；

#### HashMap和Hashtable的区别？
HashMap是Hashtable的轻量级实现(非线程安全的实现)，他们都完成了Map接口，主要区别在于HashMap允许空

(null)键值(key)，由于非线程安全，效率上可能高于Hashtable；

#### 什么是WSE？目前最新的版本是多少？
WSE (Web Service Extension) 包来提供最新的WEB服务安全保证，目前最新版本2.0；

#### 请谈谈对正则表达式？
能将一些比较复杂的验证逻辑以简单的验证表达式验证；

#### 请解释 Web.config 文件中的重要节点？
动态调试编译 —— `<compilation/>`；
自定义错误信息 —— `<customErrors/>`；
身份验证 —— `<authentication/>`；
授权 —— `<authorization/>`；
应用程序级别跟踪记录 —— `<trace/>`；
会话状态设置 —— `<sessionState/>`；
全球化 —— `<globalization/>`；

#### 当发现不能读取页面上的输入的数据时很有可能是什么原因造成的？怎么解决？
很有可能是在Page_Load中数据处理时没有进行Page的IsPostBack属性判断；

#### 请解释什么是上下文对象，在什么情况下要使用上下文对象？
上下文对象是指 HttpContext 类的 Current 属性，当我们在一个普通类中要访问内置对象(Response、Request、

#### Session、Server、Appliction等)时就要以使用此对象；

#### 请解释流与文件有什么不同？
文件是一些具有永久存储及特定顺序的字节组成的一个有序的、具有名称的集合。因此，对于文件，人们常会想到

目录路径、磁盘存储、文件和目录名等方面。相反，流提供一种向后备存储器写入字节和从后备存储器读取字节的

方式，后备存储器可以为多种存储媒介之一。正如除磁盘外存在多种后备存储器一样，除文件流之外也存在多种流

。例如，还存在网络流、内存流和磁带流等；

从概念上阐述前期绑定(Early Binding)和后期绑定(Late Binding)的区别？
如果方法在编译时就确定就是前期绑定，如果在运行时确定的叫后期绑定。这个就像是强弱类型的比较相似，前期

绑定是在编译的时候就确定了要绑定的数据，而后期绑定是在运行的时候才填充数据。所以前期绑定如果失败，会

在编译时报编译错误，而后期绑定失败只有在运行时的时候才发生；

#### 调用Assembly.Load算静态引用还是动态引用？
个人理解其实应该是一个反射，System.Reflection.Assembly.Load()，肯定是动态引用，因为静态引用在编译时

就已经引用，并使用；

何时使用Assembly.LoadFrom()？何时使用Assembly.LoadFile()？
1、Assembly.LoadFile()只载入相应的dll文件，不载入相应dll内引用的其他dll，比如`Assembly.LoadFile("a.dll")`，

则载入a.dll，假如a.dll中引用了b.dll的话，b.dll并不会被载入；
`Assembly.LoadFrom()`则不一样，它会载入相应的dll文件及其引用的其他dll，比如上面的例子，b.dll也会被载入；
2、用`Assembly.LoadFrom()`载入一个Assembly时，会先检查前面是否已经载入过相同名字的Assembly，比如

a.dll有两个版本(版本1在目录1下，版本2放在目录2下)，程序一开始时载入了版本1，当使用

`Assembly.LoadFrom("2//a。dll")`载入版本2时，不能载入，而是返回版本1。
`Assembly.LoadFile()`的话则不会做这样的检查，比如上面的例子换成Assembly.LoadFile的话，则能正确载入版本

2；

#### 什么叫Assembly Qualified Name？它是一个文件名吗？它有什么不同？
它不是一个文件名，相比文件名，Assembly Qualified Name(程序集限定名称)，更能确定一个程序集，它包含文

件名，但同时包含版本，公钥，和区域。因为同样一个名称的文件可能有不同的版本和区域，此时单独靠文件名称

，可能会造成不能确定程序集的正确性；

做强签名的Assembly与不做强签名的Assembly有什么不同？
强签名的程序集可以确认Assembly Nname是唯一的(因为使用了public key token)；
强签名的程序集可以做COM；
强签名程序集可以安装到GAC中;

#### DateTime是否可以为null?
不能为null，包括int什么的都不能等于null。当然2.0可以里加上了可空类型，但是在编译后你会发现可空类型其实

是假的；

#### 什么叫JIT？什么是NGEN？它们分别有什么限制和好处？
.NET 采用中间语言(IL)机制。JIT(Just In Time)即时编译是指程序第一次运行的时候才进行把中间语言(IL)编译成

机器代码，JIT增加了执行效率；
本机映像生成器 (Ngen.exe) 是一个提高托管应用程序性能的工具；
Ngen.exe 创建本机映像(包含经编译的特定于处理器的机器代码的文件)，并将它们安装到本地计算机上的本机映

像缓存中。运行库可从缓存中使用本机映像，而不是使用实时 (JIT) 编译器编译原始程序集。这是为什么ASP.NET

程序第一次会比较慢，因为他是JIT；

#### Finalize() 和 Dispose() 之间的区别？
Finalize自动释放资源；Dispose()用于手动释放资源；

#### using() 语法有用吗？什么是IDisposable？
1、using()能自动调用Dispose方法;
2、IDisposiable是显示释放对象的接口，实现IDisposiable接口的类，可以显示的释放对象。通过编写Dispose方法

来实现显式释放资源；

#### tasklist /m "mscor*" 这句命令是干嘛的？
列出所有使用了以“mscor”作为开头的dll或者exe的进程和模块信息；

#### in-proc 和 out-of-proc 的区别？
in-proc是进程内，进程内能共享代码和数据块，out-of-proc是进程外，进程外的互操作需要用进程间通讯来实现

；

#### .Net里的哪一项技术能够实现out-of-proc通讯？
.NET Remoting技术或者WCF技术；

#### 当你在ASP.NET中运行一个组件时，它在Windows XP， Windows 2000， Windows 2003上分别跑在哪个进程

里面？
Windows XP：aspnet_wp.exe
Windows 2000：inetinfo.exe
Windows 2003：w3wp.exe

#### 在.Net中所有可序列化的类都被标记为什么？
`[serializable]`

#### DateTime.Parse(myString); 这行代码有什么问题？
有问题，当myString不能满足时间格式要求的时候，会引发异常，建议使用DateTime.TryParse()；

#### PDB是什么东西？在调试中它应该放在哪里？
PDB是用于保存调试和项目状态信息的文件，在Debug的时候将产生pdb文件，调试的时候应该放在和对应应用程

序集相同目录；

#### 什么叫FullTrust？放入GAC的assembly是否是FullTrust的？
FullTrust完全信任。放入GAC中的Assembly是否FullTrust，在我的理解时不是，我理解FullTrust是可以通过代码设

定的；

`gacutil /l | find /i "Corillian" `这句命令的作用是什么？
全局程序集缓存中如果有Corillian就更新该程序集，没有就安装；

`sn -t foo.dll `这句命令是干嘛的？
显示程序集foo.dll的公钥标记；

#### DCOM需要防火墙打开哪些端口？端口135是干嘛用的？
135端口，因为DCOM的端口号是随机分配的，默认情况下，会分配1024以上的端口号，所以默认情况下，

#### DCOM不能穿越防火墙。因为根本不晓得开哪个端口；
135是远程过程调用(RPC)的默认端口；

#### 对比OOP和SOA，它们的目的分别是什么？
我想OOP和SOA应该没有对比性吧。OOP是一种编程模型，强调将复杂的逻辑分解出小的模块，特性是继承，封装

和多态。而SOA是一个技术框架，技术框架和编程模型应该说不是一码事吧？SOA的思想是将业务逻辑封装成服务

或者中间件提供给应用程序来调用，当然其组件化思想是继承和发扬了OOP的优点；

XmlSerializer是如何工作的？使用这个类的进程需要什么ACL权限？

XmlSerializer是将对象的属性和字段进行序列化和反序列化的，序列化成为XML数据，反序列化再将xml转换成对

象。应该至少需要ACL权限中的读权限；

ACL(Access Control List) 访问控制表，ACL是存在于计算机中的一张表，它使操作系统明白每个用户对特定系统

对象，例如文件目录或单个文件的存取权限。每个对象拥有一个在访问控制表中定义的安全属性。这张表对于每个

系统用户有拥有一个访问权限。最一般的访问权限包括读文件(包括所有目录中的文件)，写一个或多个文件和执行

一个文件(如果它是一个可执行文件或者是程序的时候)。Windows NT、Novell公司的Netware，Digital公司的

OpenVMS和基于UNIX系统是使用这种访问控制表的系统。而此表的实现在各个系统中却不一样；

#### 为什么不提倡catch(Exception)？
原因可能有两点：
1、try ... catch ... 在出现异常的时候影响性能；
2、应该捕获更具体的异常，比如IOExeception，OutOfMemoryException等；

#### Debug.Write和Trace.Write有什么不同？何时应该使用哪一个？
Debug.Write是调试的时候向跟踪窗口输出信息。当编译模式为Debug的时候才有效，为Release的时候

Debug.Write在编译的时候会忽略，而Trace则是在Debug和Release两种模式下均可以向跟踪窗口输出信息；

#### Debug Build 和 Release Build的区别，是否会有明显的速度变化？请说明理由？
Debug会产生pdb文件，Release不会；
Debug用于开发时的调试，不能用于部署，而Release用于部署；
Debug编译一些特殊代码，比如#IFDEBUG Debug.Write等，而Release则会将那些特殊标记省略；
PDB程序数据库文件，文件保存着调试和项目状态信息，使用这些信息可以对程序的调试配置进行增量链接；

#### JIT是以Assembly为单位发生还是以方法为单位发生？这对于工作区有何影响？
以方法为单位发生，道理很简单，因为对于一次运行，很可能只用到一个程序集中极少数类型和对象，而大部分可

能并不会被使用；

#### a.Equals(b) 和 a == b 一样吗？
不一样。a.Equals(b) 表示a与b一致，a==b 表示a与b的值相等；

#### 在对象比较中，对象一致和对象相等分别是指什么？
对象一致是指两个对象是同一个对象，引用相同；对象相等是指两个对象的值相同，但引用不一定相同；

#### 在.Net中如何实现深拷贝(Deep Copy)？
实现IClonable接口；

#### 请解释一下IClonable？
IClonable方法是实现深度复制的接口，实现它应该能深度复制一个对象出来。深度复制的特征的调用对象的构造

方法，创建新的对象，包括创建对象中嵌套的引用对象的新实例；Shadow复制则不同，是浅表复制，不重新创建

新实例，浅表复制的实现是Object.MemberWiseClone()；

#### string是值类型还是引用类型？
引用类型；

#### XmlSerializer使用的针对属性的模式有什么好处？解决了什么问题？
只序列化有用的数据，而不是序列化整个对象。实现没必要的数据冗余和提升序列化时的性能；

#### 特性能够放到某个方法的参数上？如果可以，这有什么用？
可以，作用可以对参数有进一步限定；

#### 用什么方法比较2个字符串相似？且在系统运行时长驻内存？
生成正则表达式进行比较，使用静态变量/方法就可以长驻内存了；

#### Collection和Collections的区别？
Collection是集合类的上级接口，Collections是针对集合类的一个帮助类，它提供一系列静态方法来实现对各种集

合的搜索、排序、线程安全化操作，存在于System.Collections命名空间；

#### 死锁的必要条件？怎么克服？
系统的资源不足，进程的推进的顺序不合适，资源分配不当，一个资源每次只能被一个进程使用，一个资源请求资

源时，而此时这个资源已阻塞，对已获得资源不放，进程获得资源时，未使用完前，不能强行剥夺。

#### 什么是死锁？

死锁(Deadlock)：是指两个或两个以上的进程在运行过程中，因争夺资源而造成的一种互相等待(谁也无法再继续

推进)的现象，若无外力作用，它们都将无法推进下去。

死锁的四个必要条件：
互斥条件(Mutual exclusion)：资源不能被共享，只能由一个进程使用；
请求与保持条件(Hold and wait)：已经得到资源的进程可以再次申请新的资源；
非剥夺条件(No pre-emption)：已经分配的资源不能从相应的进程中被强制地剥夺；
循环等待条件(Circular wait)：系统中若干进程组成环路，该环路中每个进程都在等待相邻进程正占用的资源；

处理死锁的策略：
1、忽略该问题。例如鸵鸟算法，该算法可以应用在极少发生死锁的的情况下。为什么叫鸵鸟算法呢，因为传说中

鸵鸟看到危险就把头埋在地底下，可能鸵鸟觉得看不到危险也就没危险了吧，跟掩耳盗铃有点像；
2、检测死锁并且恢复；
3、仔细地对资源进行动态分配，以避免死锁；
4、通过破除死锁四个必要条件之一，来防止死锁产生；

#### 什么是Windows服务？它的生命周期与标准的EXE程序有什么不同？
Windows服务是运行在Windows后台指定用户下(默认System)的应用程序，它没有标准的UI界面，相比标准的

EXE程序，Windows服务是在服务开始的时候创建，而在服务结束的时候销毁，而且可以设置服务是否与操作系统

一起启动，一起关闭。它支持三种方式：自动方式、手动方式、禁用。自动方式的时候，Windows服务将在OS启

动后自动启动运行，而手动方式则必须手工启动服务，禁用的情况下服务将不能被启动。另外标准的EXE默认使用

的当前登录的用户，而Windows服务则默认使用System用户，这在对系统资源访问的时候特别需要注意；

#### 请解释转发与跳转的区别？
转发就是服务端的跳转A页面提交数据到B页面，B页面进行处理然后从服务端跳转到其它页面。跳转就是指客户端

的跳转；

#### 请简述一下用Socket进行同步通讯编程的详细步骤？
1、在应用程序和远程设备中使用协议和网络地址初始化套接字；
2、在应用程序中通过指定端口和地址建立监听；
3、远程设备发出连接请求；
4、应用程序接受连接产生通信Scoket；
5、应用程序和远程设备开始通讯(在通讯中应用程序将挂起直到通讯结束)；
6、通讯结束，关闭应用程序和远程设备的Socket回收资源;

#### 什么叫做SQL注入？如何防止？请举例说明？
利用SQL关键字对网站进行攻击；
过滤关键字；
所谓SQL注入(SQL Injection)，就是利用程序员对用户输入数据的合法性检测不严或不检测的特点，故意从客户端

提交特殊的代码，从而收集程序及服务器的信息，从而获取想得到的资料；

String类与StringBuilder类有什么区别？为什么.Net类库中要同时存在这两个类？

如果要操作一个不断增长的字符串，尽量不用String类，改用 StringBuilder 类。两个类的工作原理不同：String类

是一种传统的修改字符串的方式，它确实可以完成把一个字符串添加到另一个字符串上的工作没错，但是在.Net框

架下，这个操作实在是划不来。因为系统先是把两个字符串写入内存，接着删除原来的String对象，然后创建一个

String对象，并读取内存中的数据赋给该对象。这一来二去的，耗了不少时间。而使用 System。Text 命名空间下

面的 StringBuilder 类就不是这样了，它提供的Append方法，能够在已有对象的原地进行字符串的修改，简单而且

直接。当然，一般情况下觉察不到这二者效率的差异，但如果你要对某个字符串进行大量的添加操作，那么

StringBuilder 类所耗费的时间和String类简直不是一个数量级的；