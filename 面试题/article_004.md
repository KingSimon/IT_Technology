# ASP.NET面试心得
#### 1.维护数据库的完整性、一致性、你喜欢用触发器还是自写业务逻辑？为什么
答：尽可能用约束（包括CHECK、主键、唯一键、外键、非空字段）实现，这种方式的效率最好；其次用触发器，这种方式可以保证无论何种业务系统访问数据库都能维持数据库的完整性、一致性；最后再考虑用自写业务逻辑实现，但这种方式效率最低、编程最复杂，当为下下之策

#### 2 : ADO.NET相对于ADO等主要有什么改进？
答 ADO数据以Recordset 形式存储 ADO.NET以DataSet形式存储
Recordset对数据库持续连接访问ADO.NET提供对数据库断开连接
ADO.NET与ADO相比，优势在于提供了数据集和数据适配器，有利于实现分布式处理，降低对数据库服务器资源的消耗。

#### 3 : ASP.NET与ASP相比，主要有哪些进步？
答 asp.net可以使用强类型语言页面是编译，执行速度快， 增加安全性和可靠性，通过继承机制来支持代码的重用，提供声明性服务器控件减少代码行数Asp需要解释，执行速度慢，重用代码不方便，没有调试机制

#### 4：C#中的委托是什么？事件是不是一种委托？
答 委托本质上是一种“方法接口”，它相当于C/C++中的函数指针，当然它比函数指针安全，在C#中通常用于事件处理。与JAVA相比，可以避免使用大量小粒度的匿名类。
事件不是委托，不过由于事件的性质决定了处理它的程序逻辑能访问的参数，因此，在C#中处理事件的逻辑都包装为委托（一种“方法接口”）。实际上，如果你处理自定义的事件，就像JAVA中那样用接口实现也是可以的，不过这么做在C#一般没有什么特别的好处。

#### 5：new有几种用法
第一种:new Class();
第二种:覆盖方法
public new XXXX(){}
第三种:new 约束指定泛型类声明中的任何类型参数都必须有公共的无参数构造函数。

#### 6：如何把一个array复制到arrayList里
答
```
foreach( object o in array )
{
arrayList.Add(o)
}
```
#### 7：DataGrid、DataSource可以连接什么数据源
答
```
[ DataSet、DataTable、DataView] DataSet、DataTable、DataView、IList
```
#### 8：概述反射和序列化
答 反射:程序集包含模块，而模块包含类型，类型又包含成员。反射则提供了封装程序集、模块和类型的对象。可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型，然后，可以调用类型的方法或访问其字段和属性
答 序列化:序列化是将对象转换为容易传输的格式的过程。例如，可以序列化一个对象，然后使用 HTTP 通过 Internet 在客户端和服务器之间传输该对象。在另一端，反序列化将从该流重新构造对象。

#### 9：概述OR/Mapping 的原理
答 利用反射，配置 将类于数据库表映射

#### 10：类成员有( )种可访问形式
答 可访问性：public、protected 、private、internal

#### 11：用sealed修饰的类有什么特点
答 sealed 修饰符用于防止从所修饰的类派生出其它类。如果一个密封类被指定为其他类的基类，则会发生编译时错误。密封类不能同时为抽象类。
sealed 修饰符主要用于防止非有意的派生，但是它还能促使某些运行时优化。具体说来，由于密封类永远不会有任何派生类，所以对密封类的实例的虚拟函数成员的调用可以转换为非虚拟调用来处理。

#### 12：执行下面代码后：
```
String strTemp =”abcdefg 某某某”；
Int i System.Text.Encoding.Default.GetBytes(strTemp).Length;
Int j = strTemp.Length;
```
问：`i=(14 ) ；j=(11 )`
`i=(14 ) ；j=(11 )` 中文两个字节

#### 13：C#中，string str = null 与 string str =”"，请尽量用文字说明区别。(要点：说明详细的内存空间分配)
答 string str = null 是不分配内存空间,而string str = “” 给它分配长度为空字符串的内存空间。

#### 14：概述.NET里对 Remoting 和 Webservices 两项技术的理解和实际中的应用
答 远程逻辑调用，Remoing接口只能用在.NET中

#### 16：概述三层结构体系
答 表示层 数据层 业务层

#### 17：用.net做B/S结构的系统，您是用几层结构来开发，每一层之间的关系以及为什么要这样分层？
答：一般为3层
数据访问层，业务层，表示层。
数据访问层对数据库进行增删查改。
业务层一般分为二层，业务表观层实现与表示层的沟通，业务规则层实现用户密码的安全等。
表示层为了与用户交互例如用户添加表单
优点： 分工明确，条理清晰，易于调试，而且具有可扩展性。
缺点： 增加成本。

#### 18：什么是ASP.NET中的用户控件
答:用户控件就是.ascx扩展名的东西喽,可以拖到不同的页面中调用,以节省代码.比如登陆可能在多个页面上有,就可以做成用户控件,但是有一个问题就是用户控件拖到不同级别的目录下后里面的图片等的相对路径会变得不准确,需要自已写方法调整。

#### 19：什么叫应用程序域？
什么是受管制的代码？
什么是强类型系统？
什么是装箱和拆箱？
什么是重载？
#### CTS、CLS和CLR分别作何解释？
答 应用程序域可以理解为一种轻量级进程。起到安全的作用。占用资源小
受管制的代码：unsafe：非托管代码。不经过CLR运行。
强类型系统 RTTI：类型识别系统。
装箱就是把值类型到引用类型的转换
拆箱就是引用类型到值类型的转换
重载 方法名相同 不用个数的参数或者指定不同参数类型
CTS：通用语言系统。CLS：通用语言规范。CLR：公共语言运行库。
```
class box{
int aa(object i) {}
object bb(){}
}
int bb=5
new box().aa(bb) //装箱
int conver=（int）new box().zz(); //拆箱
//重载
public void aaa(int rad){ }
public void aaa(int len,int bre ){ }
public void aaa(sting str){ }
```
#### 20：列举一下你所了解的XML技术及其应用
答:XML可以统一数据格式，XML可是好东西,保存配置,站与站之间的交流，WebServices都要用它.

#### 21 ：ADO.NET中常用的对象有哪些？分别描述一下。
答: Connection 对象用于在应用程序和数据库之间的连接
Command 可以检索和操纵数据库中的数据
DataAdapter对象充当DataSet和数据源之间用于检索和保存数据的桥接器
DataSet 对象从数据库检索的数据可以存储在其中能够以XML形式保存
DataReader 是查询结果的一种只进。只读的视图 不具有DataSet的任何复杂功能
所以可以加快访问和查看数据的速度 不提供断开式访问

#### 22：如何理解委托？
答:据说相当于函数指针,定义了委托就可以在不调用原方法名称的情况下调用那个方法.
msdn2005中是这样解释的:
委托具有以下特点：委托类似于 C++ 函数指针，但它是类型安全的。
委托允许将方法作为参数进行传递。委托可用于定义回调方法
委托可以链接在一起；例如，可以对一个事件调用多个方法。
方法不需要与委托签名精确匹配。

#### 23：C#中的委托是什么？事件是不是一种委托？
答 委托可以把一个方法作为参数代入另一个方法。
委托可以理解为指向一个函数的引用。
是，是一种特殊的委托

#### 24：UDP连接和TCP连接的异同。
答:UDP前者只管传,不管数据到不到,无须建立连接.TCP后者保证传输的数据准确,须要连结。

#### 25：ASP.NET的身份验证方式有哪些？分别是什么原理？
答:Forms认证,Windows集成认证等,Passport验证 None

#### 27：什么是Code-Behind技术。
答:代码分离,这是个明智的东西,或者可以理解成HTML代码写在前台,C#代码写在后台.当然前台也有脚本,类的调用等,其实写在一起也是可以的。

#### 28：.NET中读写XML的类都归属于哪些命名空间？
答:System.Xml

#### 29解释一下UDDI、WSDL的意义及其作用
答:UDDI 用于注册各个服务提供商提供的服务 以便可以共享它们 它还有助于WEB服务客户或web用户查找该Web服务
WSDL 是web服务描述语言（web Services Description Language）

#### 30：什么是SOAP,有哪些应用。
答:SOAP（Simple Object Access Protocol ）简单对象访问协议是在分散或分布式的环境中交换信息并执行远程过程调用的协议，是一个基于XML的协议。使用SOAP，不用考虑任何特定的传输协议（最常用的还是HTTP协议），可以允许任何类型的对象或代码，在任何平台上，以任何一直语言相互通信。这种相互通信采用的是XML格式的消息,具体请看

#### 31：如何理解.NET中的垃圾回收机制。
答 GC?对象创建了总要清除啊,不然内存哪够用?

#### 32：常用的调用Webservice方法有哪些？
答 HTTP-get Http-post Http-soap

#### 33 概述.NET里对 remoting 和 webservice 两项技术的理解和实际中的应用。
答 远程逻辑调用，remoing接口只能用在.net中

#### 34: 简述 private、 protected、 public、 internal 修饰符的访问权限。
答 private : 私有成员, 在类的内部才可以访问。
protected : 保护成员，该类内部和继承类中可以访问。
public : 公共成员，完全公开，没有访问限制。
internal: 在同一命名空间内可以访问。

#### 35：列举ASP.NET 页面之间传递值的几种方式。
答. 1.使用QueryString,
源页面
```
string url;
url=”anotherwebform.aspx?name=” + TextBox1.Text
Response.Redirect(url);
```
目标页面
```
Label1.Text=Request.QueryString["name"];
```
2.使用Session变量
源页面
```
Session["name"]=TextBox1.Text;
Session["email"]=TextBox2.Text;
Server.Transfer(“anotherwebform.aspx”);
```
目标页面
```
Label1.Text=Session["name"].ToString();
Label2.Text=Session["email"].ToString();
Session.Remove(“name”);
Session.Remove(“email”);
```
3.使用Server.Transfer
源页面代码:
```
public string Name
{
get {return TextBox1.Text; }
}
//然后调用Server.Transfer方法
private void Button1_Click(object sender, System.EventArgs e)
{
Server.Transfer(“anotherwebform.aspx”);
}
```
目标页面代码：
```
private void Page_Load(object sender, System.EventArgs e)
{
WebForm1 wf1;
wf1=(WebForm1)Context.Handler;
Label1.Text=wf1.Name;
}
```
#### 36：一列数的规则如下: 1、1、2、3、5、8、13、21、34…… 求第30位数是多少， 用递归算法实现。
答：
```
public class MainClass
{
public static void Main()
{
Console.WriteLine(Foo(30));
}
public static int Foo(int i)
{
if (i 0 && i return 1;
else return Foo(i -1) + Foo(i – 2);
}
}
```
#### 37：override与重载的区别
答 ：
override 与重载的区别。重载是方法的名称相同。参数或参数类型不同，进行多次重载以适应不同的需要
Override 是进行基类中函数的重写。为了适应需要。

#### 38：请编程遍历页面上所有TextBox控件并给它赋值为string.Empty？
答：
```
foreach (System.Windows.Forms.Control control in this.Controls)
{
if (control is System.Windows.Forms.TextBox)
{
System.Windows.Forms.TextBox tb = (System.Windows.Forms.TextBox)control ;
tb.Text = String.Empty ;
}
}
```
#### 39：请编程实现一个冒泡排序算法？
答：
```
int [] array = new int [*] ;
int temp = 0 ;
for (int i = 0 ; i < array.Length – 1 ; i++)
{
for (int j = i + 1 ; j < array.Length ; j++)
{
if (array[j] < array[i])
{
temp = array[i] ;
array[i] = array[j] ;
array[j] = temp ;
}
}
}
```
#### 40：描述一下C#中索引器的实现过程，是否只能根据数字进行索引？
答：不是。可以用任意类型。

#### 41：求以下表达式的值，写出您想到的一种或几种实现方法： 1-2+3-4+……+m
答：
```
int Num = this.TextBox1.Text.ToString() ;
int Sum = 0 ;
for (int i = 0 ; i < Num + 1 ; i++) { If((i%2) == 1) { Sum += i ; } else { Sum = Sum – I ; } } System.Console.WriteLine(Sum.ToString()); System.Console.ReadLine() ; 42：在下面的例子里 using System; class A { public A() { PrintFields(); } public virtual void PrintFields(){} } class B:A { int x=1; int y; public B() { y=-1; } public override void PrintFields() { Console.WriteLine(“x={0},y={1}”,x,y); } } 当使用new B()创建B的实例时，产生什么输出？ 答：x= 1 y = 0 44：ASP.net的身份验证方式有哪些？分别是什么原理？ 答： Windwos(默认)用IIS… From(窗体)用帐户 Passport(密钥) 45：在.net中，配件的意思是？ 答：程序集。（中间语言，源数据，资源，装配清单） 46：net Remoting 的工作原理是什么？ 答：服务器端向客户端发送一个进程编号，一个程序域编号，以确定对象的位置 47：根据委托(delegate)的知识，请完成以下用户控件中代码片段的填写： namespace test { public delegate void OnDBOperate(); public class UserControlBase : System.Windows.Forms.UserControl { public event OnDBOperate OnNew; privatevoidtoolBar_ButtonClick(objectsender,System.Windows.Forms.ToolBarButtonClickEventArgs e) { if(e.Button.Equals(BtnNew)) { //请在以下补齐代码用来调用OnDBOperate委托签名的OnNew事件。 } } } 答：if( OnNew != null OnNew( this, e ; 48：SQLSERVER服务器中，给定表 table1 中有两个字段 ID、LastUpdateDate，ID表示更新的事务号， LastUpdateDate表示更新时的服务器时间，请使用一句SQL语句获得最后更新的事务号 答：Select ID FROM table1 Where LastUpdateDate = (Select MAX(LastUpdateDate) FROM table1) 49：根据线程安全的相关知识，分析以下代码，当调用test方法时i>10时是否会引起死锁?并简要说明理由。
public void test(int i)
{
lock(this)
{
if (i>10)
{
i–;
test(i);
}
}
}
```
答：不会发生死锁，（但有一点int是按值传递的，所以每次改变的都只是一个副本，因此不会出现死锁。但如果把int换做一个object，那么死锁会发生）

#### 50：简要谈一下您对微软.NET 构架下remoting和webservice两项技术的理解以及实际中的应用。
答：WS主要是可利用HTTP穿透防火墙。Remoting可以利用TCP/IP，二进制传送提高效率

#### 51：用C＃实现以下功能
a 产生一个int数组，长度为100，并向其中随机插入1-100，并且不能重复
答 ：
```
List L=new List();
Random random = new Random();
for (int i = 1; i {
if (L.IndexOf(i) < 0)
{
L.Add(random.Next(1,100));
}
}
```
b 对上面生成的数组排序，需要支持升序、降序两种顺序
答 L.Reverse(L);
L.Sort(L);

#### 52：请说明.net中的错误处理机制，并举例
答 异常是从 Exception 类类继承的对象。异常从发生问题的代码区域引发，然后沿堆栈向上传递，直到应用程序处理它或程序终止。
```
try
{ //执行代码，不确定是否会出错 }
catch
{ //出错处理 }
finally
{ //无论怎样,都要执行 }
```
#### 53：请说出强名的含义
答 垃圾回收的原理是根据是否空引用,和该数据类型占用内存来判断该收回多少内存.强类型说白就是必须要有个数据类型.

#### 54：请列出c＃中几种循环的方法，并指出他们的不同
答 FOR WHILE foreach do while

#### 55：请指出.net中所有类型的基类
答 object

#### 56：请指出GAC的含义
答 全局访问缓存

#### 57：SQL SREVER中，向一个表中插入了新数据，如何快捷的得到自增量字段的当前值
答
```
INSERT INTO jobs (job_desc,min_lvl,max_lvl)
VALUES (‘Accountant’,12,125)
```
#### 58:什么是WEB控件？使用WEB控件有那些优势？
答 web控件就是可以在服务器执行的控件，优势在于可以回传数据，带有事件驱动

#### 59: 请解释ASP.NET中以什么方式进行数据验证？
答 .NET中提供了几个数据验证控件，可以在服务器端或者客户端进行验证。

#### 60: 请谈谈对正则表达式的看法？
答 主要用在字符串的匹配上面，与具体的语言环境无关。

#### 61: ASP.NET中共有几种类型的控件？各有什么区别？
答 Html 控件 传统的html标记
Web 控件 可以回传数据，事件驱动
自定义 控件 在原有控件的基础上增加功能
复合控件 多个子控件复合成一个新的控件

#### 62: WEB控件可以激法服务端事件，请谈谈服务端事件是怎么发生并解释其原理？自动传回是什么？为什么要使用自动传回。
答 通过实现IPostBack这个接口来执行事件。自动回传就是AutoPostBack,使用自动回传可以监视客户端变化情况并将这种改变返回到服务器端

#### 63: WEB控件及HTML服务端控件能否调用客户端方法？如果能，请解释如何调用？
答 能，服务器端控件在html中表现形式还是html标记，所以可以执行客户端事件，有多种方式：1、control.Attributes["onclick"]=”…;”;
2、

#### 64: 请解释ASP.NET中的web页面与其隐藏类之间的关系？
答 继承的关系

#### 65: 什么是viewstate，能否禁用？是否所用控件都可以禁用?
答 可以全部禁用，viewstate就是hidden input,只不过加上了微软的编码方式记录控件的状态

#### 66: 当发现不能读取页面上的输入的数据时很有可能是什么原因造成的？怎么解决
答 可能就是事件没有关联，或者根本就没有写读取代码。
解决 检查代码， 设断点调试

#### 67：请解释一个WEB页面中代码执行次序。
答 参看.cs 知道最先执行OnInit(); 然后到Page_Load，最后到具体的执行事件。

#### 68：请解释什么是上下文对象，在什么情况下要使用上下文对象
答 HttpContext, 在类中调用的时候需要用到HttpContext

#### 69：请解释转发与跳转的区别？
答 Transfer 是转发 包括HttpHandler对象
Redirect 就是跳转

#### 70： 请解释ASP.NET中Button、LinkButton、ImageButton 及HyperLink这四个控件之间的功别
答 Button、ImageButton将数据传递回服务器
HyperLink 页面之间的导航
LinkButton主要用于将数据保存到服务器或访问服务器上的数据

#### 71：请解释一下.NET多层应用程序中层与层之间以那几种方式进行数据传递。并解释你自己的项目中采用那种方式进行。
答 这个传递方式不定，很多都是传递DataSet， XML也可以

#### 72：如果出现ASP.NET中的事件不能触发可能由于什么原因造成？
答 事件丢失，特别是使用VSS的时候最容易造成这种情况，原因不明

#### 73：如果需要在DataGrid控件中的某一列中添加下拉列表框并绑定数据怎么解决？
答 使用模板列来添加DropDwonList绑定数据使用代码前置的方式

#### 74：请解释ASP.NET中的数据绑定与传统数据绑定有什么区别？
答 更灵活 更方便

#### 76：请解释接口的显式实现有什么意义？
答 规定强制执行，保持一致

#### 77： 什么情况用HTML控件，什么情况用Web控件，并比较两者差别
答 客户端表现用HTML控件，如果想和Server端交互，那么加上runat=server，它就成了服务器端控件，但它没有Web控件的很多方法和属性，如果你需要用到，那么还是用Web Controls

#### 78: C#中的接口和类有什么异同。
答 接口只能包含抽象方法，不能包含任何方法实现，不能创建接口实例，接口成员没有访问修饰符，接口成员必须是方法属性事件或者索引器不能包含常数字段运算符也不能有静态成员

可访问性的关键字有如下5个：
internal：在所属程序集中可以访问。
private：私有成员, 在当前类中可以访问。
protected：保护成员，在当前类及其子类中可以访问。
public：公共成员，完全公开，没有访问限制。
internal protected：在所属程序集或当前类的子类中可以访问。（题目中未提及）
【扩展】

类的修饰符：abstract,sealed,static,unsafe 4个。
abstract：类是抽象的，不能创建类的实例
sealed：类是密封的，不能被继承
static：类是静态的，只有静态成员，没有非静态成员
unsafe：类有非安全的结构，比如指针

#### 79 .列举ASP.NET 页面之间传递值的几种方式。
答：有 querystring,cookie,session,server.transfer,application 5种方式。

【扩展】
1.使用QueryString方式(或称url传值、Response.Redirect传值)，这是最简单的方式，因为传递的值在浏览器的url中会显示出来，所以用来传递没有安全要求的值。
发送页面代码：
```
Response.Redirect(“index.aspx?username=”+txtUserName.Text.Trim());接收页面代码：
if(Request.QueryString["username"]!=null)
{
strUserName = Request.QueryString["username"];
}
```
2.使用cookie方式，cookie由服务器创建，但是保存在客户端

发送页面代码：
```
HttpCookie userName = new HttpCookie(“username”);
userName.Value = this.txtUserName.Text.Trim();
Response.Cookies.Add(userName);
Response.Redirect(“index.aspx”);接收页面代码：

if (Request.Cookies["username"] != null)
{
strUserName = Request.Cookies["username"].Value;
}
```
3.使用Session变量，session在用户向服务器发出首次请求时被创建，在服务器端，在用户关闭浏览器或异常发生时终止（还有别的session过期情况）。

发送页面代码：
```
Session["username"] = this.txtUserName.Text.trim();
Response.Redirect(“index.aspx”);接收页面代码：

if (Session["username"] != null)
{
strUserName = Session["username"].ToString();
}
```
4.使用Application变量

发送页面代码：
```
Application["username"] = this.txtUserName.Text.trim();
Response.Redirect(“index.aspx”);接收页面代码：

if (Application["username"] != null)
{
strUserName = Application["username"].ToString();
}
```
5.使用Server.Transfer方式（或称为HttpContext方式），要传递的变量可以通过属性或方法来获得，使用属性比较容易一些。

发送页面制作一个属性：
```
public string GetName
{
get { return this.txtUserName.Text.Trim(); }
}
```
发送页面代码：
```
Server.Transfer(“index.aspx”);接收页面代码：
w = (WebForm4)Context.Handler;
strUserName = w.GetName;
```
#### 80.重写、重载和隐藏三个概念的区别。
答：重写（Override）指用Override关键字重新实现基类中的虚方法，在运行过程中，无论通过哪个类型的引用，真正对象类型的方法将被调用。
重载（Overload）指多个方法共享一个名字并且拥有相同的返回值，但是拥有不同的参数。
隐藏（new）指用new关键字重新实现基类中的方法，在运行的过程中通过引用的类型判断应该调用哪个类型的方法。
【扩展】
重写实现的是运行时多态，重载实现的是编译时多态。
override 与重载的区别。重载是方法的名称相同。参数或参数类型不同，进行多次重载以适应不同的需要
Override 是进行基类中函数的重写。为了适应需要。

#### 81.如果在一个B/S结构的系统中需要传递变量值，但是又不能使用Session、Cookie、Application，您有几种方法进行处理？
答 ：
this.Server.Transfer

#### 82.请编程遍历页面上所有TextBox控件并给它赋值为string.Empty？
答：
```
foreach (System.Windows.Forms.Control control in this.Controls)
{
if (control is System.Windows.Forms.TextBox)
{
System.Windows.Forms.TextBox tb = (System.Windows.Forms.TextBox)control ;
tb.Text = String.Empty ;
}
}
```

#### 83.什么是受管制的代码？
答：unsafe：非托管代码。不经过CLR运行。

#### 84.什么是强类型系统？
答：RTTI：类型识别系统。

#### 85.常用的调用WebService的方法有哪些？
答：1.使用WSDL.exe命令行工具。
2.使用VS.NET中的Add Web Reference菜单选项

#### 86..NET Remoting 的工作原理是什么？
答：服务器端向客户端发送一个进程编号，一个程序域编号，以确定对象的位置。

#### 87.请详述在.NET中类(class)与结构(struct)的异同？
答：Class可以被实例化,属于引用类型,是分配在内存的堆上的,Struct属于值类型,是分配在内存的栈上的.

#### 88.公司要求开发一个继承System.Windows.Forms.ListView类的组件，要求达到以下的特殊功能：点击ListView各列列头时，能按照点击列的每行值进行重排视图中的所有行 (排序的方式如DataGrid相似)。根据您的知识，请简要谈一下您的思路
答：根据点击的列头,包该列的ID取出,按照该ID排序后,在给绑定到ListView中。

#### 89.给定以下XML文件，完成算法流程图。

`< DriverC >`

请画出遍历所有文件名（FileName）的流程图(请使用递归算法)。
答：
```
void FindFile( Directory d )
{
FileOrFolders = d.GetFileOrFolders();
foreach( FileOrFolder fof in FileOrFolders )
{
if( fof is File )
You Found a file;
else if ( fof is Directory )
FindFile( fof );
}
}
```
#### 90.写出一条Sql语句：取出表A中第31到第40记录（SQLServer,以自动增长的ID作为主键,注意：ID可能不是连续的。
答：解1: `select top 10 * from A where id not in (select top 30 id from A)`
解2: `select top 10 * from A where id > (select max(id) from (select top 30 id from A )as A)`

#### 91.能用foreach遍历访问的对象需要实现 ________________接口或声明________________方法的类型。
答：IEnumerable 、 GetEnumerator。

#### 92.`String s = new String(“xyz”);`创建了几个String Object?
答：两个对象，一个是“xyx”,一个是指向“xyx”的引用对象s。

#### 93.构造器Constructor是否可被Override?
答：构造器Constructor不能被继承，因此不能重写Overriding，但可以被重载Overloading。

#### 94.try {}里有一个return语句，那么紧跟在这个try后的finally {}里的code会不会被执行，什么时候被执行，在return前还是后?
答：会执行，在return前执行。

#### 95.如何处理几十万条并发数据？
答：用存储过程或事务。取得最大标识的时候同时更新..注意主键不是自增量方式这种方法并发的时候是不会有重复主键的..取得最大标识要有一个存储过程来获取.

#### 96.Session有什么重大BUG，微软提出了什么方法加以解决？
答：是IIS中由于有进程回收机制，系统繁忙的话Session会丢失，可以用Sate Server或SQL Server数据库的方式存储Session不过这种方式比较慢，而且无法捕获Session的END事件。

#### 97.进程和线程的区别？
答：进程是系统进行资源分配和调度的单位；线程是CPU调度和分派的单位，一个进程可以有多个线程，这些线程共享这个进程的资源。

#### 98.堆和栈的区别？
答：
栈：由编译器自动分配、释放。在函数体中定义的变量通常在栈上。
堆：一般由程序员分配释放。用new、malloc等分配内存函数分配得到的就是在堆上。

#### 99.成员变量和成员函数前加static的作用？
答：它们被称为常成员变量和常成员函数，又称为类成员变量和类成员函数。分别用来反映类的状态。比如类成员变量可以用来统计类实例的数量，类成员函数负责这种统计的动作。

#### 100.请说明在.NET中常用的几种页面间传递参数的方法，并说出他们的优缺点。
答：session(viewstate) 简单，但易丢失
application 全局
cookie 简单，但可能不支持，可能被伪造
input ttype=”hidden” 简单，可能被伪造
url参数 简单，显示于地址栏，长度有限
数据库 稳定，安全，但性能相对弱

#### 101.请指出GAC的含义？
答：全局程序集缓存。

#### 102.向服务器发送请求有几种方式？
答：get,post。get一般为链接方式，post一般为按钮方式。

#### 103.DataReader与DataSet有什么区别？
答：一个是只能向前的只读游标，一个是内存中的表。

#### 104.软件开发过程一般有几个阶段？每个阶段的作用？
答：需求分析，架构设计，代码编写，QA，部署

#### 105.在C#中using和new这两个关键字有什么意义，请写出你所知道的意义？using 指令 和语句 new 创建实例 new 隐藏基类中方法。
答：using 引入名称空间或者使用非托管资源
new 新建实例或者隐藏父类方法

#### 106.需要实现对一个字符串的处理,首先将该字符串首尾的空格去掉,如果字符串中间还有连续空格的话,仅保留一个空格,即允许字符串中间有多个空格,但连续的空格数不可超过一个.
答：
```
string inputStr=” xx xx “;
inputStr=Regex.Replace(inputStr.Trim(),” *”,” “);
```
#### 107.下面这段代码输出什么？为什么？
```
int i=5;
int j=5;
if (Object.ReferenceEquals(i,j))
Console.WriteLine(“Equal”);
else
Console.WriteLine(“Not Equal”);
```
答：不相等，因为比较的是对象

#### 108.什么叫做SQL注入，如何防止？请举例说明。
答：利用sql关键字对网站进行攻击。过滤关键字’等

#### 109.什么是反射？
答：动态获取程序集信息

#### 110.用Singleton如何写设计模式
答：static属性里面new ,构造函数private

#### 111.什么是Application Pool？
答：Web应用，类似Thread Pool，提高并发性能。

#### 112.什么是虚函数？什么是抽象函数？
答：虚函数：没有实现的，可由子类继承并重写的函数。抽象函数：规定其非虚子类必须实现的函数，必须被重写。

#### 113.什么是XML？
答：XML即可扩展标记语言。eXtensible Markup Language.标记是指计算机所能理解的信息符号，通过此种标记，计算机之间可以处理包含各种信息的文章等。如何定义这些标记，即可以选择国际通用的标记语言，比如HTML，也可以使用象XML这样由相关人士自由决定的标记语言，这就是语言的可扩展性。XML是从SGML中简化修改出来的。它主要用到的有XML、XSL和XPath等。

#### 114什么是Web Service？UDDI？
答：Web Service便是基于网络的、分布式的模块化组件，它执行特定的任务，遵守具体的技术规范，这些规范使得Web Service能与其他兼容的组件进行互操作。
UDDI 的目的是为电子商务建立标准；UDDI是一套基于Web的、分布式的、为Web Service提供的、信息注册中心的实现标准规范，同时也包含一组使企业能将自身提供的Web Service注册，以使别的企业能够发现的访问协议的实现标准。

#### 115.列举一下你所了解的XML技术及其应用
答：XML用于配置,用于保存静态数据类型.接触XML最多的是Web Services..和config

#### 116.什么是SOAP?有哪些应用。
答：simple object access protocal,简单对象接受协议.以xml为基本编码结构,建立在已有通信协议上(如http,不过据说ms在搞最底层的架构在tcp/ip上的soap)的一种规范Web Service使用的协议..

#### 117.XML 与 HTML 的主要区别
答：
1. XML是区分大小写字母的，HTML不区分。
2. 在HTML中，如果上下文清楚地显示出段落或者列表键在何处结尾，那么你可以省略

或者

之类的结束 标记。在XML中，绝对不能省略掉结束标记。
1. 在XML中，拥有单个标记而没有匹配的结束标记的元素必须用一个 / 字符作为结尾。这样分析器就知道不用 查找结束标记了。
2. 在XML中，属性值必须分装在引号中。在HTML中，引号是可用可不用的。
3. 在HTML中，可以拥有不带值的属性名。在XML中，所有的属性都必须带有相应的值。

#### 118.利用operator声明且仅声明了==，有什么错误么?
答：要同时修改Equale和GetHash() ? 重载了”==” 就必须重载 “!=”

#### 119.在.net（C# or vb.net）中，Appplication.Exit 还是 Form.Close有什么不同？
答：一个是退出整个应用程序，一个是关闭其中一个Form。

#### 120.在C#中有一个double型的变量，比如10321.5，比如122235401.21644，作为货币的值如何按各个不同国家的习惯来输出。比如美国用$10,321.50和$122，235，401.22而在英国则为￡10 321.50和￡122 235 401.22
答：
```
System.Globalization.CultureInfo MyCulture = new System.Globalization.CultureInfo(“en-US”);
//System.Globalization.CultureInfo MyCulture = new System.Globalization.CultureInfo(“en-GB”);为英 国 货币类型
decimal y = 9999999999999999999999999999m;
string str = String.Format(MyCulture,”My amount = {0:c}”,y);
```
#### 121. 62-63=1 等式不成立，请移动一个数字（不可以移动减号和等于号），使得等式成立，如何移动？
答案:62移动成2的6次方

#### 122.ADO.NET相对于ADO等主要有什么改进？
答：1ADO.NET不依赖于OleDb提供程序,而是使用.NET托管提供的程序
2不使用com
3不在支持动态游标和服务器端游
4可以断开Connection而保留当前数据集可用
5强类型转换
6 xml支持

#### 123.写一个HTML页面，实现以下功能，左键点击页面时显示“您好”，右键点击时显示“禁止右键”。并在2分钟后自动关闭页面。
答：
```
<script language=javascript>
setTimeout('window.close();',3000000000);
function show()
{
if (window.event.button == 1)
{
alert("左");
}
else if (window.event.button == 2)
{
alert("右");
}else{
alert("中");
}
}
document.onmousedown=show;
</script>
```
#### 124.大概描述一下ASP.NET服务器控件的生命周期
答：初始化 加载视图状态 处理回发数据 加载 发送回发更改通知 处理回发事件 预呈现 保存状态 呈现 处置 卸载

#### 125.，&和&&的区别。
&是位运算符，表示按位与运算，&&是逻辑运算符，表示逻辑与（and）.

#### 126.和 有什么区别？
答：表示绑定的数据源
是服务器端代码块

#### 127.你觉得ASP.NET 2.0（VS2005）和你以前使用的开发工具（.Net 1.0或其他）有什么最大的区别？你在以前的平台上使用的哪些开发思想（pattern / architecture）可以移植到ASP.NET 2.0上 (或者已经内嵌在ASP.NET 2.0中)
答：1 ASP.NET 2.0 把一些代码进行了封装打包,所以相比1.0相同功能减少了很多代码.
2 同时支持代码分离和页面嵌入服务器端代码两种模式,以前1.0版本,.NET提示帮助只有在分离的代码文件,无法在页面嵌入服务器端代码获得帮助提示,
3 代码和设计界面切换的时候,2.0支持光标定位.这个我比较喜欢
4 在绑定数据,做表的分页.Update,Delete,等操作都可以可视化操作,方便了初学者
5 在ASP.NET中增加了40多个新的控件,减少了工作量

#### 128.分析以下代码。
```
public static void Test(string ConnectString)
{
System.Data.OleDb.OleDbConnection conn = new System.Data.OleDb.OleDbConnection();
conn.ConnectionString = ConnectString;
try {
conn.Open();
…….
}
catch(Exception Ex) {
MessageBox.Show(Ex.ToString());
}
finally {
if (!conn.State.Equals(ConnectionState.Closed))
conn.Close();
}
}
```
请问

1)以上代码可以正确使用连接池吗？
答：回答：如果传入的connectionString是一模一样的话，可以正确使用连接池。不过一模一样的意思是，连字符的空格数，顺序完全一致。

2)以上代码所使用的异常处理方法，是否所有在Test方法内的异常都可以被捕捉并显示出来？
答：只可以捕捉数据库连接中的异常吧. （finally中，catch中，如果有别的可能引发异常的操作，也应该用try,catch。所以理论上并非所有异常都会被捕捉。）

#### 129.什么是WSE？目前最新的版本是多少？
答：WSE (Web Service Extension) 包来提供最新的Web服务安全保证，目前最新版本2.0。

#### 130.下面的例子中
```
using System;
class A{
public static int X;
static A(){
X=B.Y+1;
}
}
class B{
public static int Y=A.X+1;
static B(){}
static void Main(){
Console.WriteLine(“X={0},Y={1}”,A.X,B.Y);
}
}
```

1. 产生的输出结果是什么？
答：x=1,y=2

2. 不定项选择：
(1) 以下叙述正确的是： B C
A. 接口中可以有虚方法。B. 一个类可以实现多个接口。 C. 接口不能被实例化。 D. 接口中可以包含已实现的方法。
(2) 从数据库读取记录，你可能用到的方法有：B C D
A. ExecuteNonQuery B. ExecuteScalar C. Fill D. ExecuteReader

#### 131.对于一个实现了IDisposable接口的类，以下哪些项可以执行与释放或重置非托管资源相关的应用程序定义的任务？(多选) ( ABC )
A.Close
B.Dispose
C.Finalize
D.using
E.Quit

#### 132.以下关于ref和out的描述哪些项是正确的？(多选) ( ACD )
A.使用ref参数，传递到ref参数的参数必须最先初始化。
B.使用out参数，传递到out参数的参数必须最先初始化。
C.使用ref参数，必须将参数作为ref参数显式传递到方法。
D.使用out参数，必须将参数作为out参数显式传递到方法。

#### 133.在对SQL Server 数据库操作时应选用（A）。
a)SQL Server .NET Framework 数据提供程序；
b)OLE DB .NET Framework 数据提供程序；
c)ODBC .NET Framework 数据提供程序；
d)Oracle .NET Framework数据提供程序；

#### 134.下列选项中，（C）是引用类型。
a)enum类型 b)struct类型 c)string类型 d)int类型

#### 135.关于ASP.NET中的代码隐藏文件的描述正确的是（C）
a)Web窗体页的程序的逻辑由代码组成，这些代码的创建用于与窗体交互。编程逻辑唯一与用户界面不同的文件中。该文件称作为“代码隐藏”文件，如果用C＃创建，该文件将具有“.ascx.cs”扩展名。
b)项目中所有Web窗体页的代码隐藏文件都被编译成.EXE文件。
c)项目中所有的Web窗体页的代码隐藏文件都被编译成项目动态链接库（.dll）文件。
d)以上都不正确。

#### 136.以下描述错误的是（A）
a)在C++中支持抽象类而在C#中不支持抽象类。
b)C++中可在头文件中声明类的成员而在CPP文件中定义类的成员，在C#中没有头文件并且在同一处声明和定义类的成员。
c)在C#中可使用 new 修饰符显式隐藏从基类继承的成员。
d)在C#中要在派生类中重新定义基类的虚函数必须在前面加Override。

#### 137.C#的数据类型有（A）
a)值类型和调用类型； b)值类型和引用类型；c)引用类型和关系类型；d)关系类型和调用类型；

#### 138.下列描述错误的是（D）
a)类不可以多重继承而接口可以；
b)抽象类自身可以定义成员而接口不可以；
c)抽象类和接口都不能被实例化；
d)一个类可以有多个基类和多个基接口；

#### 139.在DOM中，装载一个XML文档的方法（D）
a)save方法 b)load方法 c)loadXML方法 d)send方法

#### 140.下列关于构造函数的描述正确的是（C）
a)构造函数可以声明返回类型。
b)构造函数不可以用private修饰
c)构造函数必须与类名相同
d)构造函数不能带参数

#### 141.以下是一些C#中的枚举型的定义，其中错误的用法有（）
a)public enum var1{ Mike = 100, Nike = 102, Jike }
b)public enum var1{ Mike = 100, Nike, Jike }
c)public enum var1{ Mike=-1 , Nike, Jike }
d)public enum var1{ Mike , Nike , Jike }

#### 142.
```
int[][] myArray3=new int[3][]{
new int[3]{5,6,2},
new int[5]{6,9,7,8,3},
new int[2]{3,2}
};
myArray3[2][2]的值是（D）。
a)9 b)2 c)6 d)越界
```
#### 143.接口是一种引用类型，在接口中可以声明（A），但不可以声明公有的域或私有的成员变量。
a)方法、属性、索引器和事件；
b)方法、属性信息、属性；
c)索引器和字段；
d)事件和字段；

#### 144.ASP.NET框架中，服务器控件是为配合Web表单工作而专门设计的。服务器控件有两种类型，它们是(A )
a)HTML控件和Web控件 b)HTML控件和XML控件 c)XML控件和Web控件 d)HTML控件和IIS控件

#### 145.ASP.NET中，在Web窗体页上注册一个用户控件，指定该控件的名称为”Mike”，正确的注册指令为( D)
a)
b)
c)
d)以上皆非

#### 146.在ADO.NET中，对于Command对象的ExecuteNonQuery()方法和ExecuteReader()方法，下面叙述错误的是（C）。
a)insert、update、delete等操作的Sql语句主要用ExecuteNonQuery()方法来执行；
b)ExecuteNonQuery()方法返回执行Sql语句所影响的行数。
c)Select操作的Sql语句只能由ExecuteReader()方法来执行；
d)ExecuteReader()方法返回一个DataReder对象；

#### 147.下列ASP.NET语句（b）正确地创建了一个与SQL Server 2000数据库的连接。
a)SqlConnection con1 = new Connection(“Data Source = localhost; Integrated Security = SSPI; Initial Catalog = myDB”);
b)SqlConnection con1 = new SqlConnection(“Data Source = localhost; Integrated Security = SSPI; Initial Catalog = myDB”);
c)SqlConnection con1 = new SqlConnection(Data Source = localhost; Integrated Security = SSPI; Initial Catalog = myDB);
d)SqlConnection con1 = new OleDbConnection(“Data Source = localhost; Integrated Security = SSPI; Initial Catalog = myDB”);

#### 148.WinForm中，关于ToolBar控件的属性和事件的描述不正确的是(D)。
a)Buttons属性表示ToolBar控件的所有工具栏按钮
b)ButtonSize属性表示ToolBar控件上的工具栏按钮的大小，如高度和宽度
c)DropDownArrows属性表明工具栏按钮（该按钮有一列值需要以下拉方式显示）旁边是否显示下箭头键
d)ButtonClick事件在用户单击工具栏任何地方时都会触发

#### 149.在ADO.NET中执行一个存储过程时，如果要设置输出参数则必须同时设置参数的方向和（B ），必要时还要设置参数尺寸。
a)大小； b)上限； c)初始值； d)类型；
18.如果将窗体的FormBoderStyle设置为None，则( B)。
a)窗体没有边框并不能调整大小； b)窗体没有边框但能调整大小；
c)窗体有边框但不能调整大小； d)窗体是透明的；

#### 150.如果要将窗体设置为透明的，则( B)
a)要将FormBoderStyle属性设置为None； b)要将Opacity属性设置为小于100%得值；
c)要将locked 属性设置为True； d)要将 Enabled属性设置为True；

#### 151.下列关于C#中索引器理解正确的是(B/C )
a)索引器的参数必须是两个或两个以上 b)索引器的参数类型必须是整数型
c)索引器没有名字 d)以上皆非

#### 152.下面描述错误的是( C/D)。
a)窗体也是控件； b)窗体也是类； c)控件是从窗体继承来的； d)窗体的父类是控件类；

#### 153.要对注册表进行操作则必须包含( D)。
a)System.ComponentModel命名空间； b)System.Collections命名空间；
c)System.Threading命名空间； d)Microsoft.Win32命名空间；

#### 154.要创建多文档应用程序，需要将窗体的(D )属性设为true。
a)DrawGrid； b)ShowInTaskbar； c)Enabled； d)IsMdiContainer；

#### 155.如果设treeView1=new TreeView()，则treeView1.Nodes.Add(“根节点”)返回的是一个 ()类型的值。
a)TreeNode；
b)int；
c)string；
d)TreeView；

#### 156.下面关于XML的描述错误的是（D）。
a)XML提供一种描述结构化数据的方法；
b)XML 是一种简单、与平台无关并被广泛采用的标准；
c)XML文档可承载各种信息；
d)XML只是为了生成结构化文档；

#### 157.装箱、拆箱操作发生在: ( C )
A.类与对象之间 B.对象与对象之间
C.引用类型与值类型之间 D.引用类型与引用类型之间

#### 158.用户类若想支持Foreach语句需要实现的接口是: ( A )
A.IEnumerableB.IEnumerator
C.ICollectionD.ICollectData

#### 159..Net Framework通过什么与COM组件进行交互操作？( C )
A.Side By SideB.Web Service
C.InteropD.PInvoke

#### 160..Net依靠以下哪一项技术解决COM存在的Dll Hell问题的？( A )
A.Side By SideB.Interop
C.PInvokeD.COM+

#### 161.以下哪个是可以变长的数组？( D )
A.`Array` B.`string[]`
C.`string[N]` D.`ArrayList`

#### 162.用户自定义异常类需要从以下哪个类继承：( A )
A.Exception B.CustomException
C.ApplicationException D.BaseException

#### 163.对于一个实现了IDisposable接口的类，以下哪些项可以执行与释放或重置非托管资源相关的应用程序定义的任务？(多选) ( ABC )
A.Close B.DisposeC.Finalize
D.using E.Quit

#### 164.Net依赖以下哪项技术实现跨语言互用性？( C )
A.CLR B.CTS C.CLS D.CTT
11.请问: String类与StringBuilder类有什么区别？为什么在.Net类库中要同时存在这2个类？(简答)
如果要操作一个不断增长的字符串，尽量不用String类,改用StringBuilder类。两个类的工作原理不同:String类是一种传统的修改字符串的方式，它确实可以完成把一个字符串添加到另一个字符串上的工作没错,但是在.NET框架下，这个操作实在是划不来。因为系统先是把两个字符串写入内存，接着删除原来的String对象，然后创建一个String对象，并读取内存中的数据赋给该对象。这一来二去的，耗了不少时间。而使用System.Text命名空间下面的StringBuilder类就不是这样了，它提供的Append方法，能够在已有对象的原地进行字符串的修改，简单而且直接。当然，一般情况下觉察不到这二者效率的差异，但如果你要对某个字符串进行大量的添加操作，那么StringBuilder类所耗费的时间和String类简直不是一个数量级的。

#### 165.以下哪些可以作为接口成员？(多选) ( ABDE )
A.方法B.属性C.字段D.事件E.索引器
F.构造函数G.析构函数
#### 166.以下关于ref和out的描述哪些项是正确的？(多选) ( ACD )
A.使用ref参数，传递到ref参数的参数必须最先初始化。
B.使用out参数，传递到out参数的参数必须最先初始化。
C.使用ref参数，必须将参数作为ref参数显式传递到方法。
D.使用out参数，必须将参数作为out参数显式传递到方法。

#### 167.“访问范围限定于此程序或那些由它所属的类派生的类型”是对以下哪个成员可访问性含义的正确描述？( B )
A.public B.protected C.internal D.protected internal
#### 168.
```
class Class1
{
private static int count = 0;
static Class1()
{
count++;
}
public Class1()
{
count++;
}
}
Class1 o1 = new Class1();
Class1 o2 = new Class1();
```
请问，o1.Count的值是多少？( C )
A.1 B.2 C.3 D.4

#### 169.
```
abstract class BaseClass
{
public virtual void MethodA()
{
}
public virtual void MethodB()
{
}
}
class Class1: BaseClass
{
public void MethodA(string arg)
{
}
public override void MethodB()
{
}
}
class Class2: Class1
{
new public void MethodB()
{
}
}
class MainClass
{
public static void Main(string[] args)
{
Class2 o = new Class2();
Console.WriteLine(o.MethodA());
}
}
```
请问，o.MethodA调用的是: ( A )
A.BaseClass.MethodAB.Class2.MethodA
C.Class1.MethodAD.都不是

#### 170.请叙述属性与索引器的区别。
属性 索引器
通过名称标识。 通过签名标识。
通过简单名称或成员访问来访问。 通过元素访问来访问。
可以为静态成员或实例成员。 必须为实例成员。
属性的 get 访问器没有参数。 索引器的 get 访问器具有与索引器相同的形参表。
属性的 set 访问器包含隐式 value 参数。 除了 value 参数外，索引器的 set 访问器还具有与索引器相同的形参表。

#### 171.请叙述const与readonly的区别。
每一个class至多只可以定义一个static构造函数，并且不允许增加访问级别关键字，参数列必须为空。
为了不违背编码规则，通常把static数据成员声明为private，然后通过statci property提供读写访问。
const 关键字用于修改字段或局部变量的声明。它指定字段或局部变量的值不能被修改。常数声明引入给定类型的一个或多个常数。
const数据成员的声明式必须包含初值，且初值必须是一个常量表达式。因为它是在编译时就需要完全评估。
const成员可以使用另一个const成员来初始化，前提是两者之间没有循环依赖。
readonly在运行期评估赋值，使我们得以在确保“只读访问”的前提下，把object的初始化动作推迟到运行期进行。
readonly 关键字与 const 关键字不同：　const 字段只能在该字段的声明中初始化。readonly 字段可以在声明或构造函数中初始化。因此，根据所使用的构造函数，readonly 字段可能具有不同的值。另外，const 字段是编译时常数，而 readonly 字段可用于运行时常数。
readonly 只能在声明时或者构造函数里面初始化，并且不能在 static 修饰的构造函数里面。

#### 172.您需要创建一个ASP.NET应用程序，公司考虑使用Windows身份认证。
所有的用户都存在于AllWin这个域中。您想要使用下列认证规则来配置这个应用程序：
a、 匿名用户不允许访问这个应用程序。
b、 所有雇员除了Tess和King都允许访问这个应用程序。
请问您应该使用以下哪一个代码段来配置这个应用程序？( A )
A.

B.

C.

D.

E.

#### 173.您要创建一个显示公司员工列表的应用程序。您使用一个DataGrid控件显示员工的列表。您打算修改这个控件以便在这个Grid的Footer显示员工合计数。请问您应该怎么做？( C? )
A.重写OnPreRender事件，当Grid的Footer行被创建时显示合计数。
B.重写OnItemCreated事件，当Grid的Footer行被创建时显示合计数。
C.重写OnItemDataBound事件，当Grid的Footer行被创建时显示合计数。
D. 重写OnLayout事件，当Grid的Footer行被创建时显示合计数。

#### 174.您要创建ASP.NET应用程序用于运行AllWin公司内部的Web站点，这个应用程序包含了50个页面。您想要配置这个应用程序以便当发生一个HTTP代码错误时它可以显示一个自定义的错误页面给用户。您想要花最小的代价完成这些目标，您应该怎么做？(多选)( CD )
A.在这个应用程序的Global.asax文件中创建一个Application_Error过程去处理ASP.NET代码错误。
B.在这个应用程序的Web.config文件中创建一个applicationError节去处理ASP.NET代码错误。
C.在这个应用程序的Global.asax文件中创建一个CustomErrors事件去处理HTTP错误。
D.在这个应用程序的Web.config文件中创建一个CustomErrors节去处理HTTP错误。
E.在这个应用程序的每一页中添加一个Page指示符去处理ASP.NET 代码错误。
F. 在这个应用程序的每一页中添加一个Page指示符去处理ASP.NET HTTP错误。

#### 175.您的公司有一个DB Server，名为AllWin，其上装了MS SQLSERVER 2000。现在需要您写一个数据库连接字符串，用以连接AllWin上SQL SERVER中的一个名为PubBase实例的Test库。请问，应该选择下面哪一个字符串？( B )
A. “Server=AllWin;Data Source=PubBase;Initial Catalog=Test;Integrated Security=SSPI”
B. “Server= AllWin;Data Source=PubBase;Database=Test;Integrated Security= SSPI”
C. “Data Source= AllWin \PubBase;Initial Category=PubBase;Integrated Security= SSPI”
D. “Data Source= AllWin \ PubBase;Database=Test;Integrated Security= SSPI”

#### 176.您为AllWin公司创建了一个ASP.NET应用程序。这个应用程序调用一个 Xml Web Service。这个 Xml Web Service 将返回一个包含了公司雇员列表的DataSet对象。请问您该如何在这个程序中使用这个 Xml Web Service？( ? )
A.在“引用”对话框的.Net标签中选择 System.Web.Services.dll。
B.在“Web引用”对话框中输入这个 XML Web service 的地址。
C.在您的 Global.asax.cs 中添加一条 using 语句并指定这个 XML Web service 的地址。
D.在您的 Global.asax.cs 中写一个事件处理器导入这个 Xml Web Service 相应的 .wsdl 和 .disco 文件。

#### 177.您要创建一个ASP.NET应用程序在DataGrid控件中显示一个经过排序的列表。产品数据被存放于一个名为PubBase的Microsoft SQL Server 数据库。每个产品的主键是ProductID，Numeric型并且每个产品有一个字母描述字段，名为ProductName。您使用一个SqlDataAdapter对象和一个SqlCommand对象通过调用一个存储过程从数据库中获取产品数据。您将SqlCommand对象的CommandType属性设置为CommandType.StoredProcedure，并将它的CommandText属性设置为procProductList。您成功的获取了一个DataTable对象，其中是已经按ProductID降序排列的产品列表。您打算显示以相反的字母顺序排列的ProductName，请问该怎么做？ ( B )
A. 将SqlCommand对象的CommandType属性修改为CommandType.Text，将CommandText属性修改为”SELECT * FROM procProductList ORDER BY ProductName DESC”。然后将这个DataTable对象绑定到DataGrid控件。
B. 创建一个基于这个DataTable对象的新的DataView并将这个DataView的Sort属性设置为“ProductName DESC”。然后将这个DataView对象绑定到DataGrid控件。
C. 将DataGrid控件的AllowSorting属性设置为True，并将DataGridColumn的SortExpression属性设置为 “ProductName DESC”.以显示ProductName。然后将这个DataTable对象绑定到DataGrid控件。
D. 将DataTable对象的DisplayExpression属性设置为 “ORDER BY ProductName DESC”.。然后将这个DataTable对象绑定到DataGrid控件。
#### 178.C#代码实现，确保windows程序只有一个实例（instance）
```
///

///应用程序的主入口点。
///


[STAThread]
staticvoid Main()
{
//防止程序多次运行
if(!OneInstance.IsFirst(“GetPayInfo”))
{
MessageBox.Show (“警告:程序正在运行中! 请不要重复打开程序!可在右下角系统栏找到!”,”程序错误提示:”,MessageBoxButtons.OK,MessageBoxIcon.Stop);
return;
}
Application.Run(new Form1());
}
// ******************* 防止程序多次执行 **************************
publicabstractclass OneInstance
{
///

///判断程序是否正在运行
///


///程序名称 ///如果程序是第一次运行返回True,否则返回False
publicstaticbool IsFirst(string appId)
{
bool ret=false;
if(OpenMutex(0x1F0001,0,appId)==IntPtr.Zero)
{
CreateMutex(IntPtr.Zero,0,appId);
ret=true;
}
return ret;
}
[DllImport("Kernel32.dll",CharSet=CharSet.Auto)]
privatestaticextern IntPtr OpenMutex(
uint dwDesiredAccess, // access
int bInheritHandle, // inheritance option
string lpName // object name
);

[DllImport("Kernel32.dll",CharSet=CharSet.Auto)]
privatestaticextern IntPtr CreateMutex(
IntPtr lpMutexAttributes, // SD
int bInitialOwner, // initial owner
string lpName // object name
);
}
```
#### 179. DataReader和DataSet的异同
DataReader和DataSet最大的区别在于,DataReader使用时始终占用SqlConnection,在线操作数据库..任何对SqlConnection的操作都会引发DataReader的异常..因为DataReader每次只在内存中加载一条数据,所以占用的内存是很小的..因为DataReader的特殊性和高性能.所以DataReader是只进的..你读了第一条后就不能再去读取第一条了..
DataSet则是将数据一次性加载在内存中.抛弃数据库连接..读取完毕即放弃数据库连接..因为DataSet将数据全部加载在内存中.所以比较消耗内存…但是确比DataReader要灵活..可以动态的添加行,列,数据.对数据库进行回传更新操作…

#### 180． 私有程序集与共享程序集有什么区别？
一个私有程序集通常为单个应用程序所使用，并且存储于这个应用程序所在的目录之中，或此目录下面的一个子目录中。共享程序集通常存储在全局程序集缓存（Global Assembly Cache）之中，这是一个由.NET运行时所维护的程序集仓库。共享程序集通常是对许多应用程序都有用的代码库，比如.NET Framework类。

#### 181． 请解释进程与线程的区别？进程与程序的区别?
一般，一个应用程序对应于一个或多个进程，可以把进程看作是该应用程序在*作系统中的标识；而一个进程通常由多个线程组成，而线程是*作系统为该应用程序分配处理时间的最小单元。

#### 182． CLR与IL分别是什么含义？
CLR:公共语言运行时，类似于Java中的JVM，Java虚拟机；在.Net环境下，各种编程语言使用一种共同的基础资源环境，这就是CLR，CLR将直接与*作系统进行通信，而编程语言如C#.NET将尽量避免直接与*作系统直接通信，加强了程序代码的执行安全性，可以这样看：CLR就是具体的编程语言如：C#.NET与*作系统之间的翻译，同时它为具体的编程语言提供了许多资源：
IL，中间语言，也称MSIL，微软中间语言，或CIL，通用中间语言；所有.NET源代码（不管用哪种语言编写）在进行编译时都被编译成IL。在应用程序运行时被即时（Just-In-Time，JIT）编译器处理成为机器码，被解释及执行。

#### 183． 请解释web.config文件中的重要节点
appSettings包含自定义应用程序设置。
system.web 系统配置
compilation动态调试编译设置
customErrors自定义错误信息设置
authentication身份验证,此节设置应用程序的身份验证策略。
authorization授权, 此节设置应用程序的授权策略.

#### 184． 请解释ASP。NET中的web页面与其隐藏类之间的关系？
一个ASP.NET页面一般都对应一个隐藏类,一般都在ASP.NET页面的声明中指定了隐藏类例如一个页面Tst1.aspx的页面声明如下

#### 185.`Codebehind=”Tst1.aspx.cs”` 表明经编译此页面时使用哪一个代码文件
`Inherits=”T1.Tst1″` 表用运行时使用哪一个隐藏类

#### 186． 什么是ViewState，能否禁用？是否所用控件都可以禁用?
ViewState是保存状态的一种机制，EnableViewState属性设置为false即可禁用

#### 187． 当发现不能读取页面上的输入的数据时很有可能是什么原因造成的？怎么解决
很有可能是在Page_Load中数据处理时没有进行Page的IsPostBack属性判断

#### 188． 请解释什么是上下文对象，在什么情况下要使用上下文对象
上下文对象是指HttpContext类的Current 属性，当我们在一个普通类中要访问内置对象(Response,Request,Session,Server,Appliction等)时就要以使用此对象

#### 189． 请解释转发与跳转的区别？
转发就是服务端的跳转A页面提交数据到B页面,B页面进行处理然后从服务端跳转到其它页面
跳转就是指客户端的跳转

#### 190. 简述全局变量和局部变量的区别。
全局变量和局部变量的差别：程序上讲是作用域不同，生存期不同，存储上来讲，存储的位置不同，
一个存放在静态存储区，另一个存放在动态存储区。

#### 191. 简述传值调用与传址调用的主要区别。
传值调用，实参把值传递给参数，在被调函数中形参的值得变化不会影响实参。传址调用，实参把值的地址传给了形参，如果被调函数中形参所指的值发生了变化，实参所指的值夜将变化，因为他们指向的是同一个地址。

#### 192. 试述函数和过程的区别。
函数和过程不是asp.net的内容吧？pascal中有这个概念，函数可有返回值，存过程没有（但是可以通过
参数达到这样的效果）。

#### 193. 对变量而言，遵循“先声明，后使用”的原则有什么好处。
先声名，在编译调试的过程中可以及早的发现问题。众所周知，解决问题越早付出的代价越少。

#### 194. HtmlGenericControl控件与那些Html标记对应？
HtmlGenericControl控件与那些Html标记对应？可以到msdn上察看。这就不累赘了。

#### 195. 控件LinkButton与Button的主要区别在哪里？
LinkButton和Button外观上不同，linkbutton上的文本显示为超链接的样子，用法和Button相同。

#### 196. 控件ImageButton与Image有哪些不同？
ImageButton上显示的是图片，用法和Button也大同小异。

#### 197. 在设计ASP.NET应用程序时，怎样获取客户端的IP地址？
可以在客户端提取IP,也可以在服务器端提取。客户端用jscript或javascript,服务器端有相应的类解决。

#### 198. 在ASP.NET应用程序设计中，Server.MapPath()方法有什么实用价值？
Server.MapPath()，程序做出来以后，移植性好。

#### 199. 简述认证和授权的概念。
包名：System.Security.Principal:
Identity:（识别）包装了已经验证过的用户名和认证的方式
主要成员：Name, IsAuthenticated, AuthenticationType
Principal: 当前代码的security上下文。包含Identity和Roles. 用于授权
主要成员：IsInRole, Identity

* // 一般用户可以有多个Indentity, 即多种身份来访问不同资源 –pending
* 每个AppDomain里面都有CallContext，CallContext里面包含Principal。线程在启动的时候也会带上

Pricncipal的ref。静态方法，仅对当前线程
* Thread.CurrentPrincipal / WindowsIdentity.GetCurrent()静态方法返回当前用户。

Permission: 权限。不是用户需要权限，是执行它的代码需要权限。
Demand()要求调用此代码的代码有什么权限。Assert()断言
三种权限：
1. 代码权限: 基类为CodeAccessPermission .用来保护环境变量、文件、访问非托管代码。
2. Identity权限：基类为CodeAccessPermission。对应于控制台中的信任集设定。基于发行者、强类型、域、URL。总表：
ms-help://MS.VSCC.2003/MS.MSDNQTR.2003APR.1033/cpguide/html/cpconidentitypermissions.htm
3. PrincipalPermission(Role Based Permission)

Authorization 授权 判断用户是否有权操作，比如登录的用户有没有权限访问资源或者数据库
Authentication 认证 用户的Identity. 主要有：HTTP基础认证、证书、Kerberos、Passport、NTLM、
Forms-based、Digest

这两个东西最好从读音上区别，以前一直糊涂。一般应用先authenticate用户, 判断用户是否能链接到系
统。然后authorization, 判断对某个功能是否有权限。

authorization一般有两种：ACL/ROLES
ACL:Acess Control Lists. 判断用户是否在有权限的用户组内。缺点：不能定义动态条件。
Role based: 用户加入到某个role以后，自动获得了很多特定的权限。先判断请求者的Identity, 然后看

它是否在Role里面。类似windows用户和组的关系

1. 代码中的检查方式：new PrincipalPermission(name,role).Demand();
2. 利用Attribute的方式：
```
[PrincipalPermissionAttribute(SecurityAction.Demand,Name="MyUser",Role="Administrator")]
```
3. 使用 Principal 对象中的属性和 IsInRole 方法执行显式安全性检查。
4. web.config里面authorization节中的users/roles(这个一般资料都没提到）