# C#中的多线程
如同java一样，在c#中写一个多线程应用是非常简单的，本章将介绍如何在c#种开发多线程程序。在.net中线程是由System.Threading 名字空间所定义的。所以你必须包含这个名字空间。
using System.Threading;

### 开始一个线程

System.Threading 名字空间的线程类描述了一个线程对象，通过使用类对象，你可以创建、删除、停止及恢复一个线程。创建一个新线程通过new 操作，并可以通过start()方法启动线程


```
thread = new Thread(new ThreadStart(HelloWorld));
thread.Start();
```
注意：和java程序不同，创建新线程并调用start()方法后并不去调用run()方法，而是传递线程调用程序

下面是启动线程执行的函数

```
protected void HelloWorld()
{
string str ;
Console.write("helloworld");
}
}
```

### 杀死一个线程

线程类的 Abort()方法可以永久的杀死一个线程。在杀死一个线程起前应该判断线程是否在生存期间。

```
if ( thread.IsAlive )
{
thread.Abort();
}
```
### 停止一个线程

Thread.Sleep 方法能够在一个固定周期类停止一个线程

```
thread.Sleep();
```
### 设定线程优先级

线程类中的ThreadPriority 属性是用来设定一个ThreadPriority的优先级别。线程优先级别包括Normal, AboveNormal, BelowNormal, Highest, and Lowest几种。

```
thread.Priority = ThreadPriority.Highest;
```
### 挂起一个线程

调用线程类的Suspend()方法将挂起一个线程直到使用Resume()方法唤起她。在挂起一个线程起前应该判断线程是否在活动期间。

```
if (thread.ThreadState = ThreadState.Running )
{
thread.Suspend();
}
```
### 唤起一个线程

通过使用Resume()方法可以唤起一个被挂起线程。在挂起一个线程起前应该判断线程是否在挂起期间，如果
线程未被挂起则方法不起作用。

```
if (thread.ThreadState = ThreadState.Suspended )
{
thread.Resume();
}
```