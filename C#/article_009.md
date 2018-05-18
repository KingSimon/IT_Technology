# C#中的内存管理
c#内存管理提供了与java一样的自动内存管理功能,让程序员从繁重的内存管理中摆脱出来，内存管理提高了代码的质量和提高了开发效率。

c#限制了着指针的使用，免除了程序员对内存泄漏的烦恼，但是不是意味着向java程序员一样c#程序员在也不能使用指针代来的好处。微软在设计C#语言时考虑到这个问题，在一方面抛弃指针的同时，另一方面采用折衷的办法，通过一个标志来时程序引入指针。

首先我们来了解自动内存管理

```
public class Stack {
    private Node first = null;
    public bool Empty {
        get {
            return (first == null);
        }
    }
    public object Pop() {
        if (first == null)
            throw new Exception("Can't Pop from an empty Stack.");
        else {
            object temp = first.Value;
            first = first.Next;
            return temp;
        }
    }
    public void Push(object o) {
        first = new Node(o, first);
    }
    class Node {
        public Node Next;
        public object Value;
        public Node(object value): this(value, null) {}
        public Node(object value, Node next) {
            Next = next;
            Value = value;
        }
    }
}
```
程序创建了一个stack类来实现一个链，使用一个push方法创建Node节点实例和一个当不再需要Node节点时的收集器。一个节点实例不能被任何代码访问时，就被收集。例如当一个点元素被移出栈，相关的Node就被收集。

The example

```
class Test {
    static void Main() {
        Stack s = new Stack();
        for (int i = 0; i < 10; i++)
            s.Push(i);
        s = null;
    }
}
```

关于指针的引用，c#中使用unsafe标志来代表队指针的引用。以下程序演示了指针的用法，不过由于使用指针，内存管理就不得不手工完成。

```
using System;
class Test {
    unsafe static void Locations(byte[] ar) {
        fixed (byte *p = ar) {
            byte *p_elem = p;
            for (int i = 0; i < ar.Length; i++) {
                byte value = *p_elem;
                string addr = int.Format((int) p_elem, "X");
                Console.WriteLine("arr[{0}] at 0x{1} is {2}", i, addr, value);
                p_elem++;
            }
        }
    }
    static void Main() {
        byte[] arr = new byte[] {1, 2, 3, 4, 5};
        WriteLocations(ar);
    }
}
```