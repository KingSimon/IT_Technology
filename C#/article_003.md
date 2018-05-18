# C#中的方法参数

## 1.1 IN 参数

c#种的四种参数形式：
* 一般参数
* in参数
* out参数
* 参数数列

本章将介绍后三种的使用。

在C语言你可以通传递地址（即实参）或是DELPHI语言中通过VAR指示符传递地址参数来进行数据排序等操作，在C#语言中，是如何做的呢？"in"关键字可以帮助你。这个关键字可以通过参数传递你想返回的值。

```C#
namespace TestRefP {
using System;
public class myClass {
    public static void RefTest(ref int iVal1 ) {
        iVal1 += 2;
    }
    public static void Main() {
        int i=3; //变量需要初始化
        RefTest(ref i );
        Console.WriteLine(i);
    }
}
}
```

必须注意的是变量要须先初始化。

结果：
```
5
```

## 1.2 OUT 参数


你是否想一次返回多个值?在C++语言中这项任务基本上是不可能完成的任务。在c#中"out"关键字可以帮助你轻松完成。这个关键字可以通过参数一次返回多个值。

```
public class mathClass {
    public static int TestOut(out int iVal1, out int iVal2) {
        iVal1 = 10;
        iVal2 = 20;
        return 0;
    }
    public static void Main() {
        int i, j; // 变量不需要初始化。
        Console.WriteLine(TestOut(out i, out j));
        Console.WriteLine(i);
        Console.WriteLine(j);
    }
}
```
结果：
```
0 10 20
```
## 1.3 参数数列

参数数列能够使多个相关的参数被单个数列代表，换就话说，参数数列就是变量的长度。

```
using System;
class Test {
    static void F(params int[] args) {
        Console.WriteLine("# 参数: {0}", args.Length);
        for (int i = 0; i < args.Length; i++)
            Console.WriteLine("\targs[{0}] = {1}", i, args[i]);
    }
    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(new int[] {1, 2, 3, 4});
    }
}
```
以下为输出结果：

```
# 参数: 0
# 参数: 1
args[0] = 1
# 参数: 2
args[0] = 1
args[1] = 2
# 参数: 3
args[0] = 1
args[1] = 2
args[2] = 3
# 参数: 4
args[0] = 1
args[1] = 2
args[2] = 3
args[3]
```