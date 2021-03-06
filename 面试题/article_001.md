# .NET 面试题总结 第1部分
#### 什么是面向对象？
面向对象OO = 面向对象的分析(OOA) + 面向对象的设计(OOD) + 面向对象的编程(OOP)；
通俗的解释就是万物皆对象，把所有的事物都看作一个个可以独立的对象(单元)，它们可以自己完成自己的功能，而不是像C那样分成一个个函数；
现在纯正的OO语言主要是JAVA和C#，C++也支持OO，C是面向过程的。

#### 阐述面向接口、面向对象、面向方面编程的区别？

面向对象不好解释，可以理解为以一切元素都是对象，在设计时以对象为单位，考虑它的属性及方法。设计中采用了封装、继承、抽象的手法；

面向接口本身就是面向对象的，无所谓区别，只不过面向接口的好处是耦合性低；

面向方面Aspect-Oriented Programming (AOP) 就是大名鼎鼎的AOP。其实有点像struts里的拦截；

#### 面向对象的思想主要包括什么？
* 继承：子类拥有父类的所有数据和操作；
* 封装：用抽象的数据类型将数据和基于数据的操作封装在一起，数据被保护在抽象数据类型内；
* 多态：一个程序中同名的不同方法共存的情况。有两种形式的多态——重载与重写；

#### 抽象类是否可以继承实体类？
抽象类可以继承实体类，但是有个条件，条件是——实体类必须要有明确的构造函数。

#### 当类T只声明了私有实例构造函数时，则在T的程序文本外部，是否可以从T派生出新的类，是否可以直接创建T的任何实例？
不可以，不可以

#### C#中有没有静态构造函数，如果有是做什么用的？
有。静态构造函数用于初始化类。在创建第一个实例或引用任何静态成员之前，将自动调用静态构造函数来初始化类。静态构造函数既没有访问修饰符，也没有参 数。在创建第一个实例或引用任何静态成员之前，将自动调用静态构造函数来初始化类。无法直接调用静态构造函数。在程序中，用户无法控制何时执行静态构造函 数。静态构造函数的典型用途是：当类使用日志文件时，将使用这种构造函数向日志文件中写入项。

#### 什么是.NET？
.NET 是一种平台和框架, .NET 不是单纯的语言也不是单纯的工具,它是从底层平台开始构建起来的一个整体框架。

#### 程序集与命名空间有什么不同？
命名空间是用于避免命名冲突，专用于组织代码，当代码要在其他某个应用程序中重用时，可以降低复杂性；

程序集是重用代码的一种方式验证控件；

不同: 可以将相同命名空间中的类部署到不同的程序集中，也可以将命名空间中的类部署到一个程序集中；

命名空间：
* 有逻辑编译时机制
* 不是运行时实体
* 为源代码元素的名称提供逻辑结构

程序集：
* 有物理编译时机制
* 是运行时实体
* 为可执行文件的运行时提供物理结构

#### 什么是Web控件？使用Web控件有那些优势？

Web控件是能拖放在WEB页面上的控件

Web控件分为：内部控件、列表控件、复杂控件；

Web控件优势:
Web控件是对象，与对象一样，Web控件拥有方法和属性，并且响应事件
一旦将Web控件包括在Web页中，就可以设置其属性并调用其方法，可以为Web控件编写服务器端代码以响应在客户端上发生的事件；

#### ASP.NET中共有几种类型的控件？各有什么区别？

Web控件分为：内部控件、列表控件、复杂控件、验证控件；

* 内部控件：内部控件的使用方法与 HTML 控件相同，它们映射到HTML元素并通过使用 runat = "server" 属性在服务器上执行；

* 列表控件：用于在Web页中创建数据列表

* 复杂控件：当希望控件拥有复杂的功能或使用HTML元素无法创建的功能丰富的用户界面时，可以使用复杂控件；

* 验证控件：输入控件的数据需要进行准确性和取值范围方面的检查；

#### Web控件可以激发服务端事件，请谈谈服务端事件是怎么发生并解释其原理？自动传回是什么？为什么要使用自动传回？

由于ASP.NET是Web页面和服务端分离的，因此要产生了服务端事件；

使用DoPostBack函数能够自动地把客户端的Java script 事件转变为一个服务器端的事件，ASP.NET框架自动为我们处理有关的细节简化工作；

使用自动传回的特性：在检测到一个特定用户动作时,自动传回能够将这个页面传回服务器以处理事件；

在Web控件发生事件时，客户端采用提交的形式将数据交回服务端，服务端先调用Page_Load事件,然后根据传回的状态信息自动调用服务端事件。自动 传回是当我们在点击客户端控件时，采用提交表单的形式将数据直接传回到服务端。只有通过自动传回才能实现服务端事件的机制，如果没有自动回传机制就只能调 用客户端事件，而不能调用服务端事件；

#### 请解释ASP.NET中以什么方式进行数据验证？
ASP.NET中有非空验证、比较验证、取值范围验证、正则表达式验证及客户自定义验证五大控件，另还有一个集中验证信息处理控件；

#### 什么是ASP.NET中的用户控件？

用户控件是能够在其中放置标记和Web服务器控件的容器。然后，可以将用户控件作为一个单元对待，为其定义属性和方法；

用户控件以.ascx为扩展名，可以拖到不同的页面中调用，以节省代码。比如登录可能在多个页面上有，就可以做成用户控件，但是有一个问题就是用户控件拖到不同级别的目录下后里面的图片等的相对路径会变得不准确，需要自已写方法调整；

问这样的问题，一般是迷惑你。因为新手还是分不清楚用户控件和服务器控件(也称自定义控件)，用户控件一般用在内容多为静态，或者少许会改变的情况下，用的比较大，类似ASP中的include，但是功能要强大的多；

#### Web控件及HTML服务端控件能否调用客户端方法？如果能，请解释如何调用？

可以调用
例如：
```
<asp:TextBox id="TextBox1" onclick="clientfunction();" runat="server"></asp:TextBox>
<INPUT id="Button2" value="Button" name="Button2"runat="server" onclick="clientfunction();">
```

#### C#、Java 和 C++ 的特点，有什么相同的地方，不同的地方？C#分别从C++和Java中吸取了他们哪些优点？

C#看起来与Java有着惊人的相似：它包括了诸如单一继承、界面、与Java几乎同样的语法和编译成中间代码再运行的过程。

但是C#与Java有着明显的不同：它借鉴了Delphi的一个特点，与COM(组件对象模型)是直接集成。

微软C#语言定义主要是从C和C++继承而来的,而且语言中的许多元素也反映了这一点。C#在设计者从C++继承的可选选项方面比Java要广泛一些(比如说 structs),它还增加了自己新的特点(比方说源代码版本定义)；

#### C#从Java继承而来的特点？

类：在C#中类的声明与Java很相似，特点看起来与Java相比没有变化；

布尔运算：条件表达式的结果是布尔数据类型，布尔数据类型是这种语言中独立的一种数据类型。从布尔类型到其他类型没有直接的转换过程。布尔常量true和false是C#中的关键字；

错误处理：如Java中那样，通过抛出和捕捉异常对象来管理错误处理过程；

内存管理：由底层.NET框架进行自动内存垃圾回收；

#### C#从C和C++继承的特点？

编译：程序直接编译成标准的二进制可执行形式；

结构体：一个C#的结构体与C++的结构体是相似的，因为它能够包含数据声明和方法。但是，不像C++，C#结构体与类是不同的而且不支持继承。但是，与Java相同的是，一个结构体可以实现界面；

预编译：C#中存在预编译指令支持条件编译、警告、错误报告和编译行控制；

#### C#独有的特点？

中间代码：微软在用户选择何时MSIL应该编译成机器码的时候是留了很大的余地。微软公司很小心的声称MSIL不是解释性的，而是被编译成了机器码。它也 明白许多——如果不是大多数的话——程序员认为Java程序要不可避免的比C编写的任何东西都要慢。而这种实现方式决定了基于MSIL的程序(指的是用 C#、Visual Basic、Managed C++ —— C++的一个符合CLS的版本 ——等语言编写的程序)将在性能上超过“解释性的”Java代码。当然，这一点还需要得到事实证明，因为C#和其他生成MSIL的编译器还没有发布。但是 Java JIT编译器的普遍存在使得Java和C#在性能上相对相同。像“C#是编译语言而Java是解释性的”之类的声明只是商业技巧。Java的中间代码和 MSIL都是中间的汇编形式的语言，它们在运行时或其它的时候被编译成机器代码；

命名空间中的声明：当你创建一个程序的时候，你在一个命名空间里创建了一个或多个类。同在这个命名空间里(在类的外面)你还有可能声明界面，枚举类型和结构体。必须使用using关键字来引用其他命名空间的内容；

基本的数据类型：C#拥有比C、C++或者Java更广泛的数据类型。这些类型是bool、byte、ubyte、short、ushort、int、 uint、long、ulong、float、double和decimal。像Java一样，所有这些类型都有一个固定的大小。又像C和C++一样，每 个数据类型都有有符号和无符号两种类型。与Java相同的是，一个字符变量包含的是一个16位的Unicode字符。C#新的数据类型是decimal数 据类型，对于货币数据，它能存放28位10进制数字；

两个基本类：一个名叫object的类是所有其他类的基类。而一个名叫string的类也像object一样是这个语言的一部分。作为语言的一部分存在意 味着编译器有可能使用它 —— 无论何时你在程序中写入一句带引号的字符串，编译器会创建一个string对象来保存它；

参数传递：方法可以被声明接受可变数目的参数。缺省的参数传递方法是对基本数据类型进行值传递。ref关键字可以用来强迫一个变量通过引用传递，这使得一个变量可以接受一个返回值。out关键字也能声明引用传递过程，与ref不同的地方是，它指明这个参数并不需要初始值；

与COM的集成：C#对Windows程序最大的卖点可能就是它与COM的无缝集成了，COM就是微软的Win32组件技术。实际上，最终有可能在任 何.NET语言里编写COM客户和服务器端。C#编写的类可以子类化一个以存在的COM组件；生成的类也能被作为一个COM组件使用，然后又能使用。比方 说，J script 语言子类化它从而得到第三个COM组件。这种现象的结果是导致了一个运行环境的产生，在这个环境里的组件是网络服务，可用用任何.NET语言子类化；

索引下标：一个索引与属性除了不使用属性名来引用类成员，而是用一个方括号中的数字来匿名引用(就像用数组下标一样)以外是相似的；

代理和反馈：一个代理对象包括了访问一个特定对象的特定方法所需的信息。只要把它当成一个聪明的方法指针就行了。代理对象可以被移动到另一个地方，然后可 以通过访问它来对已存在的方法进行类型安全的调用。一个反馈方法是代理的特例。event关键字用在将在事件发生的时候被当成代理调用的方法声明中；

#### 在C#中，string str = null 与 string str = "" 的区别？
string str = null 是不给它分配内存空间，而 string str = "" 给它分配长度为空字符串的内存空间；

#### 怎样理解静态变量？静态成员和非静态成员的区别？
静态变量属于类，而不属于对象，并对所有对象所享；
静态成员在加类的时候就被加载；

#### 静态成员和非静态成员的区别？
静态变量使用 static 修饰符进行声明，静态成员在加类的时候就被加载，通过类进行访问；

不带有static 修饰符声明的变量称做非静态变量，在对象被实例化时创建，通过对象进行访问；

一个类的所有实例的同一静态变量都是同一个值，同一个类的不同实例的同一非静态变量可以是不同的值；

静态函数的实现里不能使用非静态成员，如非静态变量、非静态函数等；

#### DataReader 和 DataSet 的异同？

DataReader使用时始终占用SqlConnection，在线操作数据库，任何对SqlConnection的操作都会引发DataReader 的异常，因为DataReader每次只在内存中加载一条数据，所以占用的内存是很小的，因为DataReader的特殊性和高性能。所以 DataReader是只进的，你读了第一条后就不能再去读取第一条了；

DataSet则是将数据一次性加载在内存中，抛弃数据库连接，读取完毕即放弃数据库连接。因为DataSet将数据全部加载在内存中，所以比较消耗内存，但是确比DataReader要灵活，可以动态的添加行、列和数据，对数据库进行回传更新操作；

#### 简述静态类和静态成员？

静态类中的成员都必须是静态的。静态类无构造方法，并且是密封类无法被继承；

静态成员访问时只能通过类名来访问，不能通过对象访问，this 也无法访问静态成员；

#### 描述接口的作用？

充当类的功能界面，接口里的成员属于抽象描述，必须通过类的实现才能使用。如：某个项目有多个模块组成，每个模块由一个开发者完成，开发者只需编写完模块功能实现后，留下的接口供其他人使用。其他人在程序中使用接口时，只需知道接口的功能，不需了解如何实现；

当功能模块无法满足需要或功能模块的需求变更时，程序员只需将该功能模块的实现代码进行修改和扩充，而其他调用接口的程序无须改动。

接口的这种应用模式成为Bridge模式(即意图和实现分离)；

接口反映了面向对象的多态特征，即通过相同方法得到不同实现。接口也反映了面向对象的封装特征，使用者可以不清楚接口成员的实现细节；

注意：因为接口成员默认的访问权限是 public，所以在实现接口时，类的成员必须为 public，且方法名和参数必须一致；

#### 描述抽象类？
用abstract修饰的类。抽象类可以包含实现的成员。未实现的成员隐含的是virtual属性，子类实现时必须用override关键字；

#### 请解释接口的显式实现有什么意义？
接口是其他类型为确保它们支持某些操作而实现的引用类型。接口从不直接创建而且没有实际的表示形式，其他类型必须转换为接口类型。一个接口定义一个协定。实现接口的类或结构必须遵守其协定。接口可以包含方法、属性、索引器和事件作为成员；

#### 静态成员和非静态成员的区别？
静态变量使用 static 修饰符进行声明，静态成员在加类的时候就被加载，通过类进行访问；

不带有static 修饰符声明的变量称做非静态变量，在对象被实例化时创建，通过对象进行访问；

一个类的所有实例的同一静态变量都是同一个值，同一个类的不同实例的同一非静态变量可以是不同的值；

静态函数的实现里不能使用非静态成员，如非静态变量、非静态函数等；

#### 在项目中为什么使用接口？接口的好处是什么？什么是面向接口开发？
接口只是一种约束。使用 interface 去定义某些特定的功能，为的是不要将代码写死在自定义类里，以便在系统开发、事后维护、功能扩充 上更有扩展性；

接口用于描述一组类的公共方法/公共属性。它不实现任何的方法或属性，只是告诉继承它的类至少要实现哪些功能，继承它的类可以增加自己的方法。使用接口可以使继承它的类：命名统一/规范、易于维护；

提供永远的接口。当类增加时，现有接口方法能够满足继承类中的大多数方法，没必要重新给新类设计一组方法，也节省了代码，提高了开发效率；
面向接口开发的好处有结构清晰，类间通信简单易懂，扩展性好，提高复用性等等；

面向接口开发就是指面向抽象协议编程，实现者在实现时要严格按协议来办；

#### 接口和类有什么异同？

不同点：
* 1、不能直接实例化接口；
* 2、接口只包含方法或属性的声明，不包含方法的实现；
* 3、接口可以多继承，类只能单继承；
* 4、类有分部类的概念，定义可在不同的源文件之间进行拆分；
* 5、表达的含义不同，接口主要定义一种规范，统一调用方法，也就是规范类，约束类，类是方法功能的实现和集合；

相同点:
* 1、接口、类和结构都可以从多个接口继承；
* 2、接口类似于抽象基类：继承接口的任何非抽象类型都必须实现接口的所有成员；
* 3、接口和类都可以包含事件、索引器、方法和属性；

#### 您在什么情况下会用到虚方法或抽象类、接口？

如果某个方法可能性在派生类中会被重写。这时就将该方法写为虚方法；

抽象类：是一个类型，与派生类之间的关系是一个“is-a”的关系。用来做基类，抽象类不能创建对象，类中包括抽象方法和实例方法；

接口：是设计一个规范，描述了 Can do ；与实现类之间是种“like-a”的关系，C#中接口不能包含字段访问修饰符；

#### 对比抽象基类和接口的使用？
抽象类能有具体实现，而接口只定义行为规范，不能有具体实现。一个类只能继承一个父类，但能实现多个接口；

#### C#中的接口和抽象类有什么异同？你选择使用接口和抽象类的依据是什么？

* 1、继承：接口支持多继承，抽象类不能实现多继承；

* 2、表达的概念：接口用于规范，抽象类用于共性。抽象类是一类事物的高度聚合，那么对于继承抽象类的子类来说，对于抽象类来说，属于“是”的关系；而接口是定义行为规范，因此对于实现接口的子类来说，相对于接口来说，是“行为需要按照接口来完成”；

* 3、方法实现：对抽象类中的方法，即可以给出实现部分，也可以不给出；而接口的方法(抽象规则)都不能给出实现部分，接口中方法不能加修饰符；

* 4、子类重写：继承类对于两者所涉及方法的实现是不同的。继承类对于抽象类所定义的抽象方法，可以不用重写，也就是说，可以延用抽象类的方法；而对于接口类所定义的方法或者属性来说，在继承类中必须重写，给出相应的方法和属性实现；

* 5、新增方法的影响：在抽象类中，新增一个方法的话，继承类中可以不用作任何处理；而对于接口来说，则需要修改继承类，提供新定义的方法；

* 6、接口可以作用于值类型(枚举可以实现接口)和引用类型；抽象类只能作用于引用类型；

* 7、接口不能包含字段和已实现的方法，接口只包含方法、属性、索引器、事件的签名；抽象类可以定义字段、属性、包含有实现的方法；

* 8、接口可以用于支持回调(CallBack)；抽象类不能实现回调，因为继承不支持；

如何选择：
* 1、看是否需要多继承，如果需要就只能使用接口；
* 2、看你在类里定义的方法是否需要有实现的代码，如果要，就使用抽象类；
* 3、使不同的类型有共同的特性的时候使用接口，因为它支持多继承；只想从一个类型继承出不同的行为的子类的时候使用抽象类，可以在基类里有代码实现；

从实现接口和现实抽象类的方法来看，接口是死的，抽象类是活的，当然实现接口的类是活的；

#### 结构和类的区别？

简单的说成class可以被实例化，属于引用类型struct属于值类型；

* 1、类型、空间分配：结构是值类型，是分配在内存的栈上的；类时引用类型，是分配在内存的堆上的；结构实例化可以不用new，即使用new操作也不会在堆里分配内存。作参数是按值传递，类时引用传递，变量用完自动解除内存分配，类需要垃圾回收；

* 2、基类：结构继承自 System.ValueType 类，因此不具多态性；但是注意，System.ValueType是个引用类型，类继承自System.Object类；

* 3、职能：struct常用于存储数据；而class表现为行为；

* 4、结构没有类的特性：不支持继承，但可以实现接口；

* 5、结构无法声明无参的构造函数，但可以声明有参的构造函数；

* 6、结构的实例成员不能直接赋初值，必须通过构造函数来赋值，但静态成员可以直接赋初值；

* 7、无抽象结构，但有抽象类(abstract)；

* 8、class可以声明protected成员、virtual成员、sealed成员和override成员；而struct不可以，但是值得注意的 是，struct可以重载System.Object的3个虚方法，Equals()、ToString()和GetHashTable()；

#### 接口与继承的区别？什么时候使用接口，什么时候使用继承？
* 1、接口定义一个类型需要实现的方法，属性，索引和事件，包括可能的参数类型和返回值类型，而把具体的实现交由相应的类或结构来做，从而为组件提供多态能力；

* 2、继承常用于在一个现有父类的基础上的功能扩展，往往是我们将几个类中相同的成员提取出来放在父类中实现，然后在各自的子类中加以继承；

* 3、接口可以实现多接口继承，而继承只能实现单继承；

* 4、实现继承可继承父类型的实现，由于接口中没有定义方法的实现，因此必须实现继承后该接口的所有方法；

* 5、为父类型添加方法可能不影响使用继承自该类型实现的用户，而为接口添加方法导致用户必须为新方法添加实现；

* 6、当派生类和基类是“is-a”的关系时使用"继承"，典型案例“苹果 is-a 水果”，存在“can-do”的关系时使用“接口”；

#### 重载(overload)和覆写(Override)的区别？
简单的说，一个是同一个函数的几种形式，一个是重写父类函数；
* 重载：当类包含两个名称相同但签名不同(方法名相同，参数列表不相同)的方法时发生方法重载。用方法重载来提供在语义上完成相同而功能不同的方法；
* 覆写：在类的继承中使用，通过覆写子类方法可以改变父类虚方法的实现；

区别：
* 1、方法的覆盖是子类和父类之间的关系，是垂直关系；方法的重载是同一个类中方法之间的关系，是水平关系；
* 2、覆盖只能由一个方法，或只能由一对方法产生关系；方法的重载是多个方法之间的关系；
* 3、覆盖要求参数列表相同；重载要求参数列表不同；
* 4、覆盖关系中，调用哪个方法体，是根据对象的类型(对象对应存储空间类型)来决定；重载关系，是根据调用时的实参表与形参表来选择方法体的；

#### `<%# %>` 和 `<% %>` 有什么区别？
`<%# %>` 表示绑定的数据源;
`<% %>`是服务器端代码块;

#### 值类型和引用类型的区别？
值类型包括简单类型、结构体类型和枚举类型，引用类型包括自定义类、数组、接口、委托等；
* 1、赋值方式：将一个值类型变量赋给另一个值类型变量时，将复制包含的值。这与引用类型变量的赋值不同，引用类型变量的赋值只复制对象的引用，而不复制对象本身；
* 2、派生：值类型不可能派生出新的类型，所有的值类型均隐式派生自System.ValueType。但与引用类型相同的是，结构也可以实现接口；
* 3、null：与引用类型不同，值类型不可能包含null值。然而，可空类型功能允许将null赋给值类型；
* 4、每种值类型均有一个隐式的默认构造函数来初始化该类型的默认值；

    值类型主要由两类组成：结构、枚举

    结构分为以下几类：数值类型、整型、浮点型、decimal、bool、用户定义的结构；

    引用类型的变量又称为对象，可存储对实际数据的引用。声明引用类型的关键字：class、interface、delegate、内置引用类型：object、string；
* 5、值类型存储在栈中，而引用类型存储在动态的堆中，栈是先进先出的由系统管理的空间，而堆是由应用程序控制的，可随时申请和释放该空间，在.NET中一般情况下由垃圾收集器处理，他们的不同导致在编程上的不同；

#### C#中的委托是什么？如何理解委托？
委托是一种方法容器，里面可以装载若干个具有相同签名的方法引用地址，那么调用委托，就相当于同时调用了该容器内的所有方法；
委托可以看做一种新的对象类型，具有面向对象的特点，定义时可签名接收参数，委托实例化时，可以把方法名作为一个参数传递给委托对象，委托可以理解为指向 函数的引用。生成的委托对象可以代理所传递的方法，可以接收方法的参数。也就是定义了委托，可以在不用调用原方法的情况下，调用那个方法；
委托类似于C或 C++中的函数指针，但不同的是委托是面向对象、类型安全的；
委托允许将方法作为参数进行传递；
委托可用于定义回调方法；
委托可以链接在一起，创建多个对象，使用“+=”累加到同一个委托对象上的引用上；
方法不需要与委托签名精确匹配；

#### 事件是不是一种委托？
委托是一种安全的函数指针，事件是一种消息机制；

委托与事件是什么关系？为什么要使用委托？
委托提供了封装方法的方式，事件是某动作已发生的说明，事件是建立于委托之上的；
程序运行时同一个委托能够用来调用不同的方法，只要改变它的引用方法即可，因此委托调节器用的方法不是在编译时决定的，而是在运行时确定的；

#### 请解释这种语法现象 `Session[“Name”]=20` ？
给类的索引器赋值；

#### ASP.NET的身份验证方式有哪些？分别是什么原理？
ASP.NET的身份验证有有三种：分别是Windows、Forms、Passport；
Windows验证：ASP.NET会结合信息服务(IIS)，为每个用户开启Windows帐号，验证其身份，安全性较高；
Forms验证：为每个登陆用户写入一个身份验证票据，在Web使用最广的验证方式，灵活方便；
Passport验证：由Microsoft提供的集中身份验证服务，该服务为成员站点提供单一登录和核心配置；

#### 什么是“Code-Behind”技术？
就是代码隐藏，在ASP.NET中通过ASPX页面指向CS文件的方法实现显示逻辑和处理逻辑的分离，这样有助于Web应用程序的创建。比如分工、美工和编程的可以个干各的，不用再像以前ASP那样代码和HTML代码混在一起，难以维护；
新建一个VS.Net下的项目。看到ASPX、RESX和CS三个后缀的文件，这个就是代码分离。实现了HTML代码和服务器代码分离，方便代码编写和整理；

#### 什么是活动目录？
活动目录是 Windows 2000 的最重要的功能。可以将用户信息全部集成起来，登陆以后可以访问多个不同的网络服务；
活动目录包括两个方面：目录和与目录相关的服务。安装了活动目录的计算机称为“域控制器”，对于用户而言，只要加入并接受域控制器的管理就可以在一次登录之后全网使用，方便地访问活动目录提供的网络资源。对于管理员，则可以通过对活动目录的集中管理就能够管理全网的资源；

#### .Net中读写XML的类都归属于哪些命名空间？
System.XML命名空间，任何类型的项目都可以通过System.XML命名空间进行XML处理。使用System.Xml命名空间中的 XmlDocument 类来操作XML的数据；

#### C#中Socket所在的命名空间是？
System.Net.Sockets —— Socket类为网络通信提供了一套丰富的方法和属性。Socket类允许您使用 ProtocolType枚举中所列出的任何一种协议执行异步和同步数据传输；

#### 什么是SOAP？有哪些应用？
SOAP(Simple Object Access Protocol)简单对象访问协议是在分散或分布式的环境中交换信息并执行远程过程调用的协议，是一个基于XML的协议。使用SOAP，不用考虑任何特 定的传输协议(最常用的还是HTTP协议)，可以允许任何类型的对象或代码，在任何平台上，以任何一直语言相互通信。这种相互通信采用的是XML格式的消 息；
SOAP是一种轻量级协议，用于在分散型、分布式环境中交换结构化信息。SOAP利用XML技术定义一种可扩展的消息处理框架，它提供了一种可通过多种底层协议进行交换的消息结构。这种框架的设计思想是要独立于任何一种特定的编程模型和其他特定实现的语义；

#### 如何理解.Net中的垃圾回收机制？

垃圾回收器每次进行垃圾回收时，对堆上的对象进行检查，把没有被任何变量引用的对象销毁，但并不是检查堆上的每个对象，而是将对象进行分类，将所有对象分类三代(Generation)。生命周期越短(新创建的对象)代数越小，反之越大；

在堆空间不够用时，垃圾回收器回收垃圾，检查第0代对象，如果发现没有被引用的对象，则标记这些为“垃圾”，并销毁。而幸存的部分的第0代对象将升级为第 1代对象，某些标记为“垃圾”的对象也会幸存而升级。这时如果堆空间仍然不够用(如创建比较大的新对象)，垃圾收集器将会检查第1代对象，将没有引用的对 象进行销毁。幸存部分升级为第2代对象，当内存堆空间仍然不够用时，检查第2代对象，不过第2代对象检查后仍然是第2代对象，不会继续升级；

如果发现内存不够，则垃圾回收器，将全部对象作为无效对象(被回收对象)，然后先将全局变量、static处于活动中的局部变量，以及当前CG指针指向的对象放入一个表中。然后会搜索新列表中的对象所引用的对象，加入列表中，其他没有被加入列表的对象都会被回收；

垃圾回收器优化引擎根据正在进行的分配情况确定执行回收的最佳时间。当垃圾回收器执行回收时，它检查托管堆中不再被应用程序使用的对象并执行必要的操作来回收它们占用的内存。

三个Generation，当每个Generation内存满了的时候检查引用，无引用就回收内存；

#### 常用的调用Web Service方法有哪？
* 1、使用 WSDL.exe 命令行工具；
* 2、使用VS.NET中的 “Add Web Reference” 菜单选项；

#### 什么是XML？列举一下你所了解的XML技术及其应用？

XML即“可扩展标记语言”(eXtensible Markup Language)。标记是指计算机所能理解的信息符号，通过此种标记，计算机之间可以处理包含各种信息的文章等。如何定义这些标记，既可以选择国际通用 的标记语言，比如HTML，也可以使用象XML这样由相关人士自由决定的标记语言，这就是语言的可扩展性。XML是从SGML中简化修改出来的。它主要用 到的有XML、XSL和XPath等；

XML可以用来做网页(XSLT)；XML可以当作数据库；XML可以用来保存对象的系列化；XML用于配置；用于保存静态数据类型；接触XML最多的是 Web Services 和 Web.Config；

#### XML与HTML的主要区别？
1、XML是区分大小写字母的，HTML不区分；
2、XML中，绝对不能省略掉结束标记。在HTML中，如果上下文清楚地显示出段落或者列表键在何处结尾，那么你可以省略</p>或者</li>之类的结束标记；
3、在XML中，拥有单个标记而没有匹配的结束标记的元素必须用一个 / 字符作为结尾。这样分析器就知道不用查找结束标记了；
4、在XML中，属性值必须在引号中；在HTML中，引号是可用可不用的；
5、在XML中，所有的属性都必须带有相应的值；在HTML中，可以拥有不带值的属性名；

#### C#中property与attribute的区别，他们各有什么用处？这种机制的好处在哪里？
property和attribute汉语都称之为属性；
property一个是属性，用于存取类的字段，类向外提供的数据区域；
attribute一个是特性，用来标识类、方法等的附加性质，描述对象在编译时或运行时的属性；

#### C#可否对内存进行直接的操作？
这个问题比较难回答，也是个很大的问题。但是可以这样问答。C#是可以对内存进行直接操作的，虽然很少用到指针，但是C#是可以使用指针的，在用的时候需 要在前边加 unsafe。在.Net中使用了垃圾回收机制(GC)功能，它替代了程序员，不过在C#中不可以直接使用finalize方法，而是在析构函数中调用基 类的finalize()方法；

#### 用最有效的方法算出2的3次方等于几？
`2<<3；`

#### 为了维护数据库的完整性和一致性，你喜欢用触发器还是自写业务逻辑？为什么？
触发器，性能好，事务性；

#### ADO.NET相对于ADO等主要有什么改进？

简单的说，ADO.NET新增DataSet等，不需要随时保持连接，性能提高；

* 1、ADO.NET不依赖于OLE DB提供程序，而是使用.Net托管提供的程序；
* 2、不使用COM
* 3、不再支持动态游标和服务器端游标；
* 4、可以断开Connection而保留当前数据集可用；
* 5、强类型转换；
* 6、XML支持；

您可以通过将ADO.NET的各项功能与“ActiveX数据对象”(ADO)的特定功能进行比较来理解ADO.NET的功能：

* 1、数据的内存中表示形式：
在ADO中，数据的内存中表示形式为记录集；在ADO.NET中，它是数据集。它们之间有重要的差异；

* 2、表的个数：
记录集看起来像单个表。如果记录集将包含来自多个数据库表的数据，则它必须使用JOIN查询，将来自各个数据库表的数据组合到单个结果表中。相反，数据集 是一个或多个表的集合，数据集内的表称为数据表。明确地说，它们是DataTable对象，如果数据集包含来自多个数据库表的数据，它通常将包含多个 DataTable对象。即每个DataTable对象通常对应于单个数据库表或视图。这样，数据集可以模仿基础数据库的结构；
数据集通常还包含关系。数据集内的关系类似于数据库中的外键关系，即它使多个表中的行彼此关联。例如，如果数据集包含一个有关投资者的表和另一个有关每个投资者的股票购买情况的表，则数据集可能还包含一个关系来连接投资者表的各个行和购买表的对应行；
由于数据集可以保存多个独立的表并维护有关表之间关系的信息，因此它可以保存比记录集丰富得多的数据结构，包括自关联的表和具有多对多关系的表；

* 3、数据导航和游标：
在ADO中，您使用ADO MoveNext()方法顺序扫描记录集的行。在 ADO.NET中，行表示为集合，因此您可以像依次通过任何集合那样依次通过表，或通过序号索引或主键索引访问特定行。DataRelation对象维护 有关主记录和详细资料记录的信息，并提供方法使您可以获取与正在操作的记录相关的记录。例如，从Investor表的“Nate Sun”的行开始，可以定位到Purchase表中描述其购买情况的那行。“游标”是数据库元素，它控制记录导航、更新数据的能力和其他用户对数据库所做 更改的可见性。ADO.NET不具有固有的游标对象，而是包含提供传统游标功能的数据类。例如，在ADO.NET DataReader对象中提供只进、只读游标的功能；

* 4、将打开连接的时间降至最低：
在ADO.NET中，打开连接的时间仅足够执行数据库操作，例如“选择”(Select)或“更新”(Update)。您可以将行读入数据集中，然后在不保持与数据源的连接的情况下使用它们。在ADO中，记录集可以提供不连接的访问，但ADO主要是为连接的访问设计的；
ADO和ADO.NET中的不连接处理之间存在一个显著差异。在ADO中，通过调用OLE DB提供程序来与数据库通信。但在ADO.NET中，您通过数据适配器(OleDbDataAdapter、SqlDataAdapter、 OdbcDataAdapter或OracleDataAdapter对象)与数据库通信，这将调用OLE DB提供程序或基础数据源提供的API。

ADO和ADO.NET之间的主要区别在于：

在ADO.NET中，数据适配器允许您控制将对数据集所做的更改传输到数据库的方式，方法是实现性能优化、执行数据验证检查或添加其他任何额外处理。

注意：数据适配器、数据连接、数据命令和数据读取器是组成。.NET Framework 数据提供程序的组件。Microsoft和第三方供应商可能会提供其它提供程序，这些提供程序也可集成到“Visual Studio”中。

* 5、在应用程序间共享数据：
在应用程序间传输ADO.NET数据集比传输ADO不连接的记录集要容易得多。若要将ADO不连接的记录集从一个组件传输到另一个组件，请使用COM封 送。若要在ADO.NET中传输数据，请使用数据集，它可以传输XML流。相对于COM封送，XML文件的传输提供以下便利之处：更丰富的数据类 型.COM封送提供一组有限的数据类型(由COM标准定义的那些类型)，由于ADO.NET中的数据集传输基于XML格式，所以对数据类型没有限制。因 此，共享数据集的组件可以使用这些组件一般会使用的任何丰富的数据类型集；

* 6、性能：
传输大型ADO记录集或大型ADO.NET数据集会使用网络资源；随着数据量的增长，施加于网络的压力也在增加。ADO和ADO.NET都使您可以最大限 度地降低所传输的数据。但ADO.NET还提供另一个性能优势：ADO.NET不需要数据类型转换，而需要COM封送来在组件间传输记录集的ADO则需要 将ADO数据类型转换为COM数据类型；

* 7、穿透防火墙：
防火墙可以影响试图传输不连接的ADO记录集的两个组件。请记住，防火墙通常配置为允许HTML文本通过，但防止系统级请求(如COM封送)通过。因为组件使用XML交换ADO.NET数据库，所以防火墙可以允许数据集通过。

#### ASP.NET与ASP相比，主要有哪些进步？

ASP解释型，ASP.NET编译型，性能提高，有利于保护源码；

ASP的缺点：
* VB script 和Java script 是在ASP中仅可使用的两种脚本语言。它们是基本的非类型化语言。在ASP中不能使用强类型语言；
* ASP页面需要解释，使得它执行速度较慢；
* ASP页面非常凌乱；
* 在使用ASP创建Web应用程序时，程序员和设计人员必须在同一文件上一起工作；
* 在ASP中，必须通过编写代码来提供所需的任何功能；
* 在ASP中没有对代码给予太多的关注；
* 在ASP中没有调试机制；
* 在ASP中。必须停止Web服务器才能安装DLL的新版本，并且在应用程序中使用DLL的新版本之前，必须先在注册表中注册它，而且DLL注册过程非常复杂；

ASP.NET的优点：
* ASP.NET中支持强类型语言；
* ASP.NET页将被编译而不是解释，这样它们的执行速度就比ASP页快；
* ASP.NET提供声明性服务器控件；
* ASP.NET通过继承机制来支持代码的重用；
* ASP.NET具有Trace的内置方法，可以帮助对页面进行调试；
* 在ASP.NET中，置于应用程序的Bin目录中的任何组件将自动对应用程序可用；

#### 你对XML、HTTP、Web Service了解吗？简单描述其特点、作用？
XML、HTTP可以主动获取远端Web代码，类似HttpWebRequest；

#### 存储过程和函数的区别？
存储过程是编译好的存储在数据库的操作，函数不用说了；

#### Session、ViewState、Application、Cookie的区别？

Session：用于保持状态的基于Web服务器的方法。Session允许通过将对象存储在Web服务器的内存中在整个用户会话过程中保持任何对象。主 要用于保持代码隐藏类中对象的状态。为每个用户创建的，用于存储单个用户。因为他是相对每个用户的，所以可能来取得在线人数等。

ViewState：主要用于保持Web页上控件的状态，当Web页上的控件被绑定到代码隐藏类中的对象；

Application：用于存储所有用户都可视的信息。所以它存储的是要让所有用户共享的一些信息；

Cookie：通常我们都把它放在客户端，也可以存储在服务器端。主要用它存储用户的个性设制和登陆信息；

#### 请说明在.Net中常用的几种页面间传递参数的方法，并说出他们的优缺点？

1、QueryString —— URL参数，简单，显示于地址栏，长度有限
优点：简单易用，资源占用比较少；
缺点：传递数据大小有限制，只能传递基本类型的数据，安全性差；

2、Session(ViewState)
优点：简单，灵活性强，能传递复杂的对象；
缺点：但易丢失，资源消耗大；

3、Cookie
优点：简单；
缺点：但可能不支持，可能被伪造，大小有限制不能超过4KB，不能够存储复杂对象；

4、Server.Transfer
优点：URL地址不变，安全性高，灵活性强，能传递复杂的对象；
缺点：资源消耗大；

5、Hidden Control / ViewState 简单，可能被伪造；

6、Static Member。

7、Cache

8、Application
优点：全局；
缺点：资源消耗大；

9、DataBase 数据库 稳定，安全，但性能相对弱；

10、XML or Other Files

11、XMLHTTP or Hidden iFrame / Frame

12、Context

我正在做一个通用提示页面，所有页面出现问题都要，传递几个变量字符串到同一个页面。ASPX变量字符串包括提示语言、即将跳转的页面、跳转时间。在上面的种方案中哪个更好些？

1、QueryString的毛病是无法传递很长字符串，比如系统错误信息往往就一整个屏幕；
2、Session的毛病是不能过多使用，容易丢失；
3、Cookie的毛病是依赖客户端设置，不可靠；
4、Server.Transfer的毛病是接收页面要为发送页面准备好，只能定制专门一个页面接受定制好的页面。不能是一个页面接受任何页面；
5、Hidden Control / Viewstate 只能传递本页，除非特殊制作；
6、Static Member 无法保证线程安全，可能会此处栽瓜他处得豆；
7、Cache不适合使用一次就扔的变量；
8、Application 全局的，开销大；
9、DataBase 全局固化的，开销更大，除非做日志跟踪；
10、XML or Other Files 全局固化的，开销大，除非做日志跟踪；
11、XMLHTTP or Hidden iFrame / Frame 做这个过于烦琐；
12、Context这个占用了用户ID，不适合做这个；

#### 如果在一个B/S结构的系统中需要传递变量值，但是又不能使用Session、Cookie、Application，您有几种方法进行处理？

1、input type="hidden" 简单，可能被伪造；
2、URL参数 简单，显示于地址栏，长度有限；
3、数据库 稳定，安全，但性能相对弱；
4、Server.Transfer 在新页面获得值的代码如下：
```
if (Page.PreviousPage != null)
{
TextBox st =
(TextBox)Page.PreviousPage.FindControl("TextBox1");
if (st != null)
{
Label1.Text = SourceTextBox.Text;
}
}
```
#### ASP.NET页面跳转的几种方法？

1、超链接跳转
`<a>`标签
1. `<a href=”test。aspx”></a>`
2. 这是最常见的一种转向方法;

2、HyperLink控件
1. ASP.NET服务器端控件属性NavigateUrl指定要跳转到的Url地址；
2. NavigateUrl是可以在服务器端使用代码修改，这个区别于<a>；
3. 由于HyperLink本身没有事件所以要在服务器端其它事件中设置NavigateUrl；
4. 代码示例
<asp:HyperLink id=”hyperlink” runat=”server” NavigatoeUrl=”Test.aspx”>
Test</asp:HyperLink>

3、Response.Redirect()方法
1. 过程：发送一个HTTP响应到客户端，通知客户端跳转到一个新的页面，然后客户端再发送跳转请求到服务器端；
2. 页面跳转之后内部控件保存的所有信息丢失，当A跳转到B，B页面将不能访问A页面提交的数据信息；
3. 使用这个方法使用这个方法跳转后浏览器地址栏的URL信息改变；
4. 可以使用Session、Cookies、Application等对象进行页面间的数据传递；
5. 重定向操作发生在客户端，总共涉及到两次与Web服务器的通信：一次是对原始页面的请求，另一次是重定向新页面的请求；

4、Server.Transfer()方法
1. 实现页面跳转的同时将页面的控制权进行移交；
2. 页面跳转过程中Reques、Session等保存的信息不变，跳转之后可以使用上一个页面提交的数据；
3. 跳转之后浏览器地址栏的URL不变；
4. 这种方法的重定向请求是在服务器端的进行的，浏览器不知道页面已经发生了一次跳转；

5、Server.Execute()方法
1. 该方法允许当前页面执行同一个Web服务器上的另一个页面；
2. 页面执行完毕之后重新回到原始页面发出Server.Execute()的位置；
3. 这种方式类似针对页面的一次函数调用被请求的页面可以使用原始页面的表单数据和查询字符串集合；
4. 被调用页面的Page指令的EnableViewStateMac属性设置为False；

#### 通过超链接怎样传递中文参数？
URLEncode URLDecode

#### 请说出强名的含义？
对程序集，进行公钥/私钥对签名，称为强名。用名称，版本，文化，公钥唯一确定程序集具有自己的Key，可以在GAC为公用；

#### 请列出C#中几种循环的方法，并指出他们的不同？
* for：使用于确定次数的循环；
* foreach：使用于遍历的元素只读；
* while：次数不确定条件随机变化；
* do...while：次数不确定条件随机变化，但至少要保证能被执行一次；

#### 请指出.Net中所有类型的基类？
Object

#### C#中有没有运算符重载？能否使用指针？

有，重载操作符意味着使该操作符具有不同的行为；使用操作符可以使方程式简单易懂；重载运算符使用operator关键字来创建一个运算符方法，只能在类或结构中使用operator;

能使用指针。在C#中很少需要使用指针，但仍有一些需要使用的情况。例如，在下列情况中使用允许采用指针的不安全上下文是正确的：处理磁盘上的现有结构、 涉及内部包含指针的结构的高级COM或平台调用方案、性能关键代码。不鼓励在其他情况下使用不安全上下文。具体地说，不应该使用不安全上下文尝试在C#中 编写C代码；

#### C#可否对内存进行直接的操作？
C#在unsafe 模式下可以使用指针对内存进行操作，但在托管模式下不可以使用指针，.NET默认不运行带指针的，需要设置下，选择 项目右键 -> 属性 -> 选择生成 -> “允许不安全代码” 打勾 -> 保存；

#### 用“Visual C++ 6.0”编写的代码(unmanaged code)，如何在CLR下和其他 .Net Component 结合？
.Net与COM互操作在.Net中可以通过添加引用的方式将COM加载在CLR下，将原有的COM中的类型相应变化为.Net 下可识别的类型；

#### 私有程序集与共享程序集有什么区别？

私有程序集通常为单个应用程序所使用，并且存储于这个应用程序所在的目录之中，或此目录下面的一个子目录中；

共享程序集通常存储在全局程序集缓存(Global Assembly Cache)之中，这是一个由.NET运行时所维护的程序集仓库。共享程序集通常是对许多应用程序都有用的代码库，比如.NET Framework类；

私有程序集：
* 1、默认情况下，C#程序编译为私有程序集；
* 2、需要放在应用程序所在的文件夹中；
* 3、程序集的名称在应用程序中应当是唯一的；

共享程序集：
* 1、可以被不同的应用程序共享；
* 2、在所有使用程序集的应用程序中，程序集名称应当是唯一的；
* 3、放在全局程序集缓存中；

#### 什么是GAC？它解决了什么问题？
Gloal Assembly Cache，全局应用程序集缓存。它解决了几个程序共享某一个程序集的问题，不必再将那个被共享的程序集拷贝到应用程序目录。其实这道理很简单，.Net 应用程序在加载的时候，会首先查看全局应用程序集缓存，如果有就可以直接使用，没有再到应用程序目录进行查；

#### 请指出GAC的含义？
全局程序集缓存(Global Assembly Cache)可全局使用的程序集的缓存。大多数共享程序集都安装在这个缓存中，其中也安装了一些私有程序集。存放共享程序的文件夹，可被任何项目使用。在 全局程序集缓存中部署的应用程序必须具有强名称。.Net提供的命令行工具 gacutil.exe 用于支持这一功能，gacutil.exe可以将具有强名称的程序集添至全局程序集缓存；

#### 怎样理解静态变量？
所有实例公用一个的变量；

#### 向服务器发送请求有几种方式？区别是什么？
GET、POST：GET —— 一般为链接方式；POST —— 一般为按钮方式；

主要区别如下：
* 1、GET从服务器上获取数据；POST向服务器传送数据；
* 2、GET把参数队列加到表单Action所指定的URL地址中，值和表单内的各个字段一一对应，在URL中可以显示出来；POST把表单中的各个字段及其内容放到HTML Header里，一起传送到Action所指定的URL地址中，不会在URL中可以显示出来；
* 3、对于GET，服务器端用Request.QueryString获取变量提交的值，对于Post服务器端用Request.Form获取提交的数据；
* 4、因受到URL长度的限制，GET传输的数据量少，不能大于2K；POST传输的数据量较大，一般默认没限制。理论上IIS5中为100K，最大可以达到2M；
* 5、GET安全性低；POST安全性高；

#### 软件开发过程一般有几个阶段？每个阶段的作用？
需求分析、架构设计、代码编写、QA、部署；
需求分析、概要设计、详细设计、软件编码、软件测试；
可行性分析、需求分析、实施和编码、测试、维护；
分析(需要、概要、详细)、开发(编程、单元测试)、测试(集成测试)、维护；

#### 有哪几种方法可以实现一个类存取另外一个类的成员函数及属性，并请举列来加以说明和分析？
取：类在封装时将属性及函数设置成 public ；
存：继承；

#### 如果需记录类的实例个数，该如何实现，请写一个简单的类于以证明？
const static int classNum=0;
classNum++;

#### A类是B类的基类，并且都有自己的构造，析构函数，请举例证明B类从实例化到消亡过程中构造，析构函数的执行过程？
构造函数先父后子，析构函数反之；

#### 需要实现对一个字符串的处理，首先将该字符串首尾的空格去掉，如果字符串中间还有连续空格的话，仅保留一个空格，即允许字符串中间有多个空格，但连续的空格数不可超过一个？
string inputStr=" XX XX ";
inputStr = Regex.Replace(inputStr.Trim()，" *"，" ");

#### new有几种用法？
* 1、new 运算符 —— 创建对象，调用构造函数；
* 2、new 修饰符 —— 覆盖方法，隐藏父类的成员。public new XXX(){ ... }
* 3、new 约束 —— 用于在泛型声明中，约束指定泛型类声明中的任何类型参数都必须有公共的无参数构造函数。当泛型类创建类型的新实例时，将此约束应用于类型参数，当与其他约束一起使用时，new()约束必须最后指定。如：
public class ItemFactory<T> where T : IComparable， new(){ ... }

#### 在C#中using和new这两个关键字有什么意义，请写出你所知道的意义？
using：引入名称空间或者使用非托管资源；
new：新建实例或者隐藏父类方法；

下面这段代码输出什么？为什么？
```
int i=5;
int j=5;
if (Object.ReferenceEquals(i，j))
Console.WriteLine("Equal");
else
Console.WriteLine("Not Equal");
```
输出“Not Equal”，不相等，因为比较的是对象。ReferenceEquals(object a，object b)，里面的两个参数是object对象；

#### 写一个实现对一段字符串翻转的方法，附加一些条件，如其中包括“，”、“。”，对其设计测试用例？
```
inputStr = inputStr.ToCharArray().Reverse().ToString();
```
#### 如何写Singleton设计模式？
static属性里面new，构造函数private；

#### 什么是 Application Pool？
Web应用，类似Thread Pool，提高并发性能；

#### 链表和数组的区别，各有什么优缺点？
一个可以动态增长，一个固定，性能数组较好；

#### 什么是友元函数？
friendly声明，可以访问protect级别方法；

#### 什么是虚函数？
可以被重写；

#### 什么是抽象函数？
必须被重写；

#### 什么是内存泄漏，怎样最简单的方法判断内存泄漏？
C++、C中忘了释放内存，内存不会再次分配；

#### C#中有很多类被定义为public有什么意义？
public关键字将公共访问权限授予一个或多个被声明的编程元素，对公共元素的可访问性没有限制；

#### internal修饰符有什么含义？
internal 关键字是类型和类型成员的访问修饰符。内部成员只有在同一程序集中的文件内才是可访问的。内部访问通常用于基于组件的开发，因为它使一组组件能够以私有方 式进行合作，而不必向应用程序代码的其余部分公开。例如，用于生成图形用户界面的框架可以提供“控件”类和“窗体”类，这些类通过使用具有内部访问能力的 成员进行合作。由于这些成员是内部的，它们不向正在使用框架的代码公开。在定义具有内部访问能力的成员的程序集外部引用该成员是错误的；

#### 简述 private、 protected、 public、 internal 修饰符的访问权限？
private：私有成员，在类的内部才可以访问；
protected：保护成员，该类内部和继承类中可以访问；
public：公共成员，完全公开，没有访问限制；
internal：在同一命名空间内可以访问；

#### Java的代码是半编译半解释的，C#的代码是否也是这样？
C#源码经过语言编译器执行第一次编译，变为中间语言，然后再由CLR编译成可执行代码；

#### 进程和线程的区别？
1、单位：进程是系统进行资源分配和调度的单位；线程是CPU调度和分派的单位；
2、一个进程可以有多个线程，这些线程共享这个进程的资源。进程是比线程大的程序运行单元，都是由操作系统所控制的系统运行单元，一个程序中至少要有一个进程，有一个进程中，至少要有一个线程，线程的划分尺度要比进程要小；
3、进程拥有独立的内存单元，线程是共享内存，从而极大的提高了程序的运行效率同一个进程中的多个线程可以并发执行；
4、边界：二者都定义了某种边界，进程是应用程序与应用程序之间的边界，不同的进程之间不能共享代码和数据空间，而线程是代码执行堆栈和执行上下文的边界；

#### 成员变量和成员函数前加static的作用？
它们被称为静态成员变量和静态成员函数，又称为类成员变量和类成员函数。分别用来反映类的状态，比如类成员变量可以用来统计类实例的数量，类成员函数负责这种统计的动作；

#### malloc和new的区别？
malloc在分配内存时必须按给出的字节分配，new可以按照对象的大小自动分配，并且能调用构造函数；new是对象的对象，而malloc不是；本质上new分配内存时，还会在实际内存块的前后加上附加信息，所以new所使用的内存大小比malloc多；

#### 堆和栈的区别？

栈：编译期间就分配好的内存空间，是由是操作系统(编译器)自动分配和释放的，栈上的空间是有限的。程序在编译期间变量和函数分配内存都是在栈上进行的，且在运行时函数调用时的参数的传递也是在栈上进行的；

堆：程序运行期间动态分配的内存空间，你可以根据程序的运行情况确定要分配的堆内存的大小。一般由程序员分配释放。用new、malloc等分配内存函数分配得到的就是在堆上；

栈是机器系统提供的数据结构；而堆则是C/C++函数库提供的；

栈是系统提供的功能，特点是快速高效，缺点是有限制，数据不灵活；而栈是函数库提供的功能，特点是灵活方便，数据适应面广泛，但是效率有一定降低。

栈是系统数据结构，对于进程/线程是唯一的；堆是函数库内部数据结构，不一定唯一。不同堆分配的内存无法互相操作;

栈空间分静态分配和动态分配两种。静态分配是编译器完成的，比如自动变量(auto)的分配。动态分配由alloca函数完成。栈的动态分配无需释放(是 自动的)，也就没有释放函数。为可移植的程序起见，栈的动态分配操作是不被鼓励的。堆空间的分配总是动态的，虽然程序结束时所有的数据空间都会被释放回系 统，但是精确的申请内存/释放内存匹配是良好程序的基本要素；

#### 在.Net中，类 System.Web.UI.Page 可以被继承吗？
可以；

#### 你觉得ASP.NET 2.0 (VS2005) 和你以前使用的开发工具(.Net 1.0 或 其他)有什么最大的区别？你在以前的平台上使用的哪些开发思想(Pattern / Architecture)？
1、ASP.NET 2.0 把一些代码进行了封装打包，所以相比1.0相同功能减少了很多代码；
2、同时支持代码分离和页面嵌入服务器端代码两种模式，以前1.0版本，.Net提示帮助只有在分离的代码文件，无法在页面嵌入服务器端代码获得帮助提示；
3、代码和设计界面切换的时候，2.0支持光标定位；
4、在绑定数据、做表的分页、UPDATE、DELETE等操作都可以可视化操作，方便了初学者；
5、在ASP.NET中增加了40多个新的控件，减少了工作量；

#### .Net的错误处理机制是什么？
.Net错误处理机制采用try -> catch -> finally 结构，发生错误时，层层上抛，直到找到匹配的catch为止；

#### 如何把一个array复制到arrayList里
方法1：`foreach( object o in array ) arrayList.Add(o);`
方法2：`string[] s ={ "111"， "22222" }; ArrayList list = new ArrayList(); list.AddRange(s);`
方法3：`string[] s ={ "111"， "22222" }; ArrayList list = new ArrayList(s);`

#### DataGrid.Datasouse可以连接什么数据源？
DataTable、DataView、DataSet、DataViewManager，任何实现IListSource或IList接口的组件；

#### 概述反射和序列化？

反射：程序集包含模块，而模块包含类型，类型又包含成员。反射则提供了封装程序集、模块和类型的对象。您可以使用反射在运行时动态地创建类型的实例，将类 型绑定到现有对象，或从现有对象中获取类型。然后，可以调用类型的方法或访问其字段和属性。通过反射命名空间中的类以及 System.Type，可以使用反射在运行时动态地创建类型的实例，然后调用和访问这些实例。也可以获取有关已加载的程序集和在其中定义的类型(如类、 接口和值类型)的信息；
序列化：序列化是将对象转换为容易传输的格式的过程。例如，可以序列化一个对象，然后使用 HTTP 通过 Internet 在客户端和服务器之间传输该对象。在另一端，反序列化将从该流重新构造对象；

#### 概述 O/R Mapping 的原理？
利用反射，配置将类与数据库表映射；

#### 用 sealed 修饰的类有什么特点？
1、sealed 修饰符用于防止从所修饰的类派生出其它类。如果一个密封类被指定为其他类的基类，则会发生编译时错误；
2、密封类不能同时为抽象类；
3、sealed 修饰符主要用于防止非有意的派生，但是它还能促使某些运行时优化。具体说来，由于密封类永远不会有任何派生类，所以对密封类的实例的虚拟函数成员的调用可以转换为非虚拟调用来处理；

#### UDP连接和TCP连接的异同？

UDP是用户数据报协议，是一个简单的面向数据报的传输协议，是不可靠的连接；

TCP是传输控制协议，提供的是面向连接的，是可靠的，字节流服务，当用户和服务器彼此进行数据交互的时候，必须在他们数据交互前要进行TCP连接之后才能传输数据。TCP提供超时重拨，检验数据功能；

前者只管传，不管数据到不到，无须建立连接；后者保证传输的数据准确，须要连结;

#### .Net中读写数据库需要用到哪些类？列举ADO.NET中的五个主要对象，以及他们的作用？
1、Connection用来创建一个到数据库的连接；
2、DataAdapter用来将数据填充到DataSet，在数据源和DataSet间起数据传输；
3、DataSet可以视为一个暂存区(Cache)，把从数据库中所查询到的数据保留起来，用来无连接的储存多个表的数据，并包含表与表之间的关联关系；
4、DataTable 用来存储一个表的数据；
5、DataReader用来顺序读取数据。因为 DataReader 在读取数据的时候限制了每次只读取一条数据，而且只能读，所以使用起来不但节省资源而且效率很好。使用 DataReader 对象除了效率较好之外，因为不用把数据全部传回，故可以降低网络的负载；
6、Command用来执行SQL语句；

#### 是否可以继承String类？
String类是sealed类故不可以继承；

#### .Net Remoting 的工作原理是什么？
服务器端向客户端发送一个进程编号，一个程序域编号，以确定对象的位置；

#### Remoting的作用？
采用分布式进行编程的一种技术，Remoting主要用于管理跨应用程序域的同步和异步RPC(远程过程调用协议 Remote Procedure Call protocol)会话。在默认情况下，Remoting使用 HTTP 或 TCP 协议，并使用 XML 编码的 SOAP 或本机二进制消息格式进行通信。.Net Remoting 提供了非常灵活和可扩展的编程框架，并且他可以管理对象的状态；

#### 讲一讲你理解的 Web Service，在 .Net Framework 中，怎么很好的结合XML？
从表面上看，Web Service 就是一个应用程序，它向外界暴露出一个能够通过Web进行调用的API。这就是说，你能够用编程的方法通过Web调用来实现某个功能的应用程序。从深层次 上看，Web Service 是一种新的Web应用程序分支，它们是自包含、自描述、模块化的应用，可以在网络(通常为Web)中被描述、发布、查找以及通过Web来调用。可扩展的标 记语言XML是Web Service平台中表示数据的基本格式。除了易于建立和易于分析外，XML主要的优点在于它既与平台无关，又与厂商无关。XML是由万维网协会 (W3C)创建，W3C制定的XML SchemaXSD 定义了一套标准的数据类型，并给出了一种语言来扩展这套数据类型。Web Service平台是用XSD来作为数据类型系统的。当你用某种语言如VB.NET或C#来构造一个 Web Service 时，为了符合 Web Service 标准，所有你使用的数据类型都必须被转换为XSD类型。如想让它使用在不同平台和不同软件的不同组织间传递，还需要用某种东西将它包装起来。这种东西就是 一种协议，如 SOAP；

#### Remoting 和 Web Service 的概念和原理？

简单的说，Web Service 主要是可利用HTTP，穿透防火墙；而 Remoting 可以利用 TCP/IP ，二进制传送提高效率；

1、Remoting 可以灵活的定义其所基于的协议，如果定义为HTTP，则与 Web Service 就没有什么区别了，一般都喜欢定义为 TCP，这样比 Web Service 稍为高效一些；
2、Remoting 不是标准，而 Web Service 是标准；
3、Remoting 一般需要通过一个 WinForm 或是 Windows 服务进行启动，而 Web Service 则需要IIS进行启动；
4、在 VS.Net 开发环境中，专门对 Web Service 的调用进行了封装，用起来比 Remoting 方便；
我建议还是采用 Web Service 好些，对于开发来说更容易控制。Remoting 一般用在C/S的系统中，Web Service 是用在B/S系统中，后者还是各语言的通用接口，相同之处就是都基于XML；

为了能清楚地描述 Web Service 和 Remoting 之间得区别，我打算从他们的体系结构上来说起:

Web Service 大体上分为5个层次:
1、HTTP 传输信道
2、XML 数据格式
3、SOAP 封装格式
4、WSDL 描述方式
5、UDDI 体系框架
总体上来讲，.Net下的 Web Service 结构比较简单，也比较容易理解和应用，一般来讲在.Net结构下的WebService应用都是基于 .Net Framework 以及IIS的架构之下，所以部署(Dispose)起来相对比较容易点。
从实现的角度来讲，首先 Web Service 必须把暴露给客户端的方法所在的类继承于System.Web.Services.WebService这个基类，其次所暴露的方法前面必须有[WebMethod]或者[WebMethodAttribute]。
WebService的运行机理：首先客户端从服务器找到 WebService 的 WSDL，同时在客户端声称一个代理类(Proxy Class)，这个代理类负责与 WebService 服务器进行 Request 和 Response ，当一个数据(XML格式的)被封装成SOAP格式的数据流发送到服务器端的时候，就会生成一个进程对象并且把接收到这个 Request 的 SOAP 包进行解析，然后对事物进行处理，处理结束以后再对这个计算结果进行 SOAP 包装，然后把这个包作为一个 Response 发送给客户端的代理类(Proxy Class)，同样地，这个代理类也对这个SOAP包进行解析处理，继而进行后续操作。这就是WebService的一个运行过程；

下面对.Net Remoting进行概括的阐述：
.Net Remoting 是在 DCOM 等基础上发展起来的一种技术，它的主要目的是实现跨平台、跨语言、穿透企业防火墙，这也是他的基本特点，与 WebService 有所不同的是，它支持HTTP以及TCP信道，而且它不仅能传输XML格式的SOAP包，也可以传输传统意义上的二进制流，这使得它变得效率更高也更加灵 活。而且它不依赖于IIS，用户可以自己开发(Development)并部署(Dispose)自己喜欢的宿主服务器，所以从这些方面上来讲 Web Service 其实上是 .Net Remoting 的一种特例。
1、Remoting 是 MarshByReference 的，可以传变量的引用，直接对服务器对象操作。速度快，适合Intrane(企业内部互联)。Web Service 是 MarshByValue 的，必须传对象的值。速度慢，可以过Firewall，配置比较简单，适合Internet(因特网)；
2、一般来说，Remoting 是和平台相关的不跨平台的，需要客户和服务器都是.Net，但可配置特性比较好，可以自定义协议。Web Service 可以做到跨平台通信，但必须采用SOAP协议；
3、SOAP 消息有 RPC 和文档两种样式。文档样式的body元素中包含一个或多个元素，可以是任何内容，只要接受者理解就行了。RPC样式的的body元素中包含调用的方法或远程过程的名称，以及代表方法参数的元素；