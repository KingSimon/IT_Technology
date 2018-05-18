# C#中的类属性
使用过RAD开发工具的一定inspector很熟悉，程序员通过它可以操作对象的属性，DELPHI中引入了PUBLISH关键字来公布对象属性受到程序员的普遍欢迎.通过存取标志来访问private成员，在c#中有两种途径揭示类的命名属性——通过域成员或者通过属性。前者是作为具有公共访问性的成员变量而被实现的；后者并不直接回应存储位置，只是通过存取标志(accessors)被访问。当你想读出或写入属性的值时，存取标志限定了被实现的语句。用于读出属性的值的存取标志记为关键字get，而要修改属性的值的读写符标志记为set。

类属性

* 只能读 get
* 只能写 set
* 可读可写 set/get


请看例子：

```
using System;
public class Test {
    private int m_nWrite;
    private int readonly m_nRead=100;
    private int m_nWriteRead;
    public int WRITEREAD {
        get {
            return m_nWriteRead;
        }
        set {
            m_nWriteRead=value;
        }
    }
    public int WRITE {
        set {
            m_nWrite = value;
        }
    }
    public int READ {
        get {
            return m_nRead;
        }
    }
}
class TestApp {
    public static void Main() {
        Test MyTest = new Test();
        int i=MyTest.READ; //get
        MyTest.WRITE=250; //set
        MyTest.WRITEREAD+=10000000 ; //set and get
        Console.WriteLine("get:{0} set:{1} set/get:{2} ",i,MyTest.WRITE,MyTest.WRITEREAD);
    }
}
```
如果你想要隐藏类内部存储结构的细节时，就应该采用存取标志。存取标志给值参数中的属性传递新值。同时你可以获得实现在set标志中增加有效代码的机会。