# ASP.NET的六大内置对象总结
ASP.NET的内置对象介绍:

[TOC]

## Request
Request对象主要是让服务器取得客户端浏览器的一些数据,包括从HTML表单用Post或者GET方法传递的参数、Cookie和用户认证。因为Request对象是Page对象的成员之一，所以在程序中不需要做任何的声明即可直接使用；
其类名为 HttpRequest
属性很多，但方法很少，只有一个BinaryRead()
1. 使用Request.Form属性获取数据
通过该属性，读取`<Form></Form>`之间的表单数据.注意：提交方式要设置为`Post`。
与Get方法相比较，使用Post方法可以将大量数据发送到服务器端
2. 利用Request.QueryString属性获取数据
Request对象的QuerySting属性可以获取 HTTP 查询字符串变量集合 。 通过该属性，我们可以读取地址信息   http://localhost/aaa.aspx?uid=tom&pwd=abc 其中标识为红色部分的数据.
注意：提交方式要设置为`Get`;
3. 问题：Request.Form用于表单提交方式为Post的情况，而Request.QueryString用于表单提交方式为Get的情况，如果用错，则获取不到数据。
解决方法：利用Request("元素名")来简化操作。
4. Request.ServerVariables("环境变量名称")
类似的还有：UserHostAddress,Browser,Cookies,ContentType,IsAuthenticated，Item,Params

## Response
Response对象用语输出数据到客户端，包括向浏览器输出数据、重定向浏览器到另一个URL或向浏览器输出Cookie文件。
其类名为httpResponse
属性和方法
Write() 向客户端发送字符串信息
BufferOutPut属性    是否使用缓存
Clear() 清除缓存
Flush()  强制输出缓存的所有数据
Redirect() 网页转向地址
End() 终止当前页的运行
WriteFile() 读取一个文件，并且写入客户端输出流
（实质：打开文件，并且输出到客户端。）

1. Response.Write 变量数据或字符串

```
Response.Write (变量数据或字符串)
<%=…%>
Response.Write("<script language=javascript>alert('欢迎学习ASP.NET')</script>")
Response.Write("<script>window.open('WebForm2.aspx')</script>")
```

2. Response对象的Redirect方法将客户端浏览器重定向到另外的URL上，即跳转到另一个网页。
例如:
Response.Redirect("http://www.163.net/")

3. Response.End() 终止当前页的运行
4. Response.WriteFile(FileName)
其中：
FileName 指代需向浏览器输出的文件的文件名

## Server
Server对象提供对服务器上的方法和属性进行的访问 .其类名称是HttpServerUtility.
Server对象的主要属性有：
MachineName：获取服务器的计算机名称。
ScriptTimeout：获取和设置请求超时（以秒计）。
方法名称 说明
CreateObject 创建 COM 对象的一个服务器实例。
Execute 执行当前服务器上的另一个aspx页，执行完该页后再返回本页继续执行
HtmlEncode 对要在浏览器中显示的字符串进行HTML编码并返回已编码的字符串。
HtmlDecode 对HTML编码的字符串进行解码，并返回已解码的字符串。
MapPath 返回与 Web 服务器上的指定虚拟路径相对应的物理文件路径。
Transfer 终止当前页的执行，并为当前请求开始执行新页。
UrlEncode 将代表URL的字符串进行编码，以便通过 URL 从 Web 服务器到客户端进行可靠的 HTTP 传输。
UrlDecode 对已被编码的URL字符串进行解码，并返回已解码的字符串。
UrlPathEncode 对 URL 字符串的路径部分进行 URL 编码，并返回已编码的字符串。
编码：
Server.HtmlEncode("HTML代码")
解码：
Server.HtmlDecode("已编码的HTML")
1. Server对象的MapPath方法将虚拟路径或相对于当前页的相对路径转化为Web 服务器上的物理文件路径。
语法：Server.MapPath("虚拟路径")
String FilePath
FilePath = Server.MapPath("/")
Response.Write(FilePath)

## Application
Application对象在实际网络开发中的用途就是记录整个网络的信息，如上线人数、在线名单、意见调查和网上选举等。在给定的应用程序的多有用户之间共享信息，并在服务器运行期间持久的保存数据。而且Application对象还有控制访问应用层数据的方法和可用于在应用程序启动和停止时触发过程的事件。
1. 使用Application对象保存信息
使用Application对象保存信息
Application("键名") = 值
 或
Application("键名"，值)
获取Application对象信息
变量名 = Application("键名")
或：变量名 = Application.Item("键名")
或：变量名 = Application.Get("键名")
更新Application对象的值
Application.Set("键名", 值)
删除一个键
Application.Remove("键名", 值)
删除所有键
Application.RemoveAll()
或Application.Clear()
2. 有可能存在多个用户同时存取同一个Application对象的情况。这样就有可能出现多个用户修改同一个Application命名对象，造成数据不一致的问题。
HttpApplicationState 类提供两种方法 Lock 和 Unlock，以解决对Application对象的访问同步问题，一次只允许一个线程访问应用程序状态变量。
关于锁定与解锁
锁定：Application.Lock()
访问：Application("键名") = 值
解锁：Application.Unlock()
注意：Lock方法和UnLock方法应该成对使用。
可用于网站访问人数，聊天室等设备
3. 使用Application事件
在ASP.NET 应用程序中可以包含一个特殊的可选文件——Global.asax 文件，也称作 ASP.NET 应用程序文件，它包含用于响应 ASP.NET或HTTP模块引发的应用程序级别事件的代码。
Global.asax 文件提供了7个事件，其中5个应用于Application对象

事件名称 说明
Application_Start 在应用程序启动时激发
Application_BeginRequest 在每个请求开始时激发
Application_AuthenticateRequest 尝试对使用者进行身份验证时激发
Application_Error 在发生错误时激发
Application_End 在应用程序结束时激发

## Session
Session即会话，是指一个用户在一段时间内对某一个站点的一次访问。
Session对象在.NET中对应HttpSessionState类，表示"会话状态"，可以保存与当前用户会话相关的信息。
Session对象用于存储从一个用户开始访问某个特定的aspx的页面起，到用户离开为止，特定的用户会话所需要的信息。用户在应用程序的页面切换时，Session对象的变量不会被清除。
对于一个Web应用程序而言，所有用户访问到的Application对象的内容是完全一样的；而不同用户会话访问到的Session对象的内容则各不相同。  Session可以保存变量，该变量只能供一个用户使用，也就是说，每一个网页浏览者都有自己的Session对象变量，即Session对象具有唯一性。
1. 将新的项添加到会话状态中
语法格式为：
Session ("键名") = 值
或者
 Session.Add( "键名" , 值)
2. 按名称获取会话状态中的值
语法格式为：
变量 = Session ("键名")
或者
 变量 = Session.Item("键名")
3. 删除会话状态集合中的项
语法格式为：
 Session.Remove("键名")
4. 清除会话状态中的所有值
语法格式为：
 Session.RemoveAll()
或者
 Session.Clear()
5. 取消当前会话
语法格式为：
 Session.Abandon()
6. 设置会话状态的超时期限，以分钟为单位。
语法格式为：
 Session.TimeOut = 数值
Global.asax 文件中有2个事件应用于Session对象
事件名称 说明
Session_Start 在会话启动时激发
Session_End 在会话结束时激发

## Cookie
Cookie就是Web服务器保存在用户硬盘上的一段文本。Cookie允许一个Web站点在用户的电脑上保存信息并且随后再取回它。信息的片断以‘键/值’对的形式存储。
Cookie是保存在客户机硬盘上的一个文本文件，可以存储有关特定客户端、会话或应用程序的信息，在.NET中对应HttpCookie类。
有两种类型的Cookie：会话Cookie（Session Cookie）和持久性Cookie。前者是临时性的，一旦会话状态结束它将不复存在；后者则具有确定的过期日期，在过期之前Cookie在用户的计算机上以文本文件的形式存储。
在服务器上创建并向客户端输出Cookie可以利用Response对象实现。
Response对象支持一个名为Cookies的集合，可以将Cookie对象添加到该集合中，从而向客户端输出Cookie。
通过Request对象的Cookies集合来访问Cookie