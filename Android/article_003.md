# Android编程编码规范
## 1.编程原则

### 1.1 为方法和类赋予表义性强的名字

为了使代码更加容易理解，最容易的方法之一是为你的方法赋予表义性强的名字。函数名DoIt、GetIt的可读性很难CalculateSalesTax、 RetrieveUserID相比。
由缩写方法名组成的代码很难理解和维护，没有理由再这样做了。
给方法正确的命名，可使程序工程的调试和维护工作大大的改观。请认真对待方法命名的工作，不要为了减少键入操作量而降低方法的可理解度。

实际应用举例：
1)    给方法命名时应大小写字母混合使用。如果句子全使用大写字母，那么阅读起来就非常困难，而大小写字母混合使用的句子，阅读起来就很容易。
2)    定义方法名时不要使用缩写。如果你认为应用程序中的某些工程应使用缩写，那么请将这些情况加上注释，并确保每个人在所有时间内都使用这些缩写。决不要在某些方法中对某些单词进行缩写，而在别的方法中却不使用缩写。

### 1.2 为每个方法赋予单个退出点

### 1.3 创建方法时，始终都应显式地定义它的作用域。

1)    如果你真的想创建一个公用方法，请向代码阅读者说明这一点。
2)    通过为每个方法赋予一个明确定义的作用域，可以减少代码阅读者需要投入的工作量。应确保你为方法赋予最有意义的作用域。如果一个方法只被同一类中的另一个方法调用，那么请将它创建成私有方法。如果该方法是从多个类中的多个方法中调用，请将该说明为公用方法。

###  1.4 用参数在方法之间传递数据

应尽量避免使用类变量。一般来说，变量的作用域越小越好。为了减少类变量，方法之一是将数据作为参数在不同方法之间传递，而不是让方法共享类变量。
1)    为每个参数指定数据类型。
2)    始终要对数进行检验，决不要假设你得数据没有问题。程序员常犯的一个错误是在编写方法时假设数据没有问题。在初始编程阶段，当编写调用方法时，这样的假设并无大碍。这时你完全能够知道什么是参数的许可值，并按要求提供这些值。但如果你不对参数的数据进行检验，那么下列情况就会给你带来很大麻烦：另外某个人创建了一个调用方法，但此人不知道允许的值；你在晚些时候添加了新的调用方法，并错误的传递了坏数据。

## 2. 命名约定
第一个字母小写，中间每个单词的首写字母大写，其它字母小写，这样保证了对变量名能够进行正确的断句。请避免任何自定义的单词缩写，除非此缩写已经人尽皆知。

### 2.1     包、类及方法命名

| 标示符类型 | 命名约定 | 例子 |
|--------|--------|--------|
|包|l 全部小写|局部包|
| |l  标识符用点号分隔开来。为了使包的名字更易读，Sun 公司建议包名中的标识符用点号来分隔。|interface.screens|
| |l  Sun 公司的标准 java 分配包用标识符 .java 开头。|全局包|
| |l 全局包的名字用你的机构的 Internet 保留域名开头 。|com.rational.www. interface.screens|
| | | |
|类，接口|l 类的名字应该使用名词。|Class Hello ;|
| |l 每个单词第一个字母应该大写。|Class HelloWorld ;|
| |l  避免使用单词的缩写，除非它的缩写已经广为人知，如HTTP。|Interface Apple ;|
| | | |
|方法|l 第一个单词一般是动词。|getName();|
| |l  第一个字母是小写，但是中间单词的第一个字母是大写。|setName();|
| |l 如果方法返回一个成员变量的值，方法名一般为get+成员变量名，如若返回的值是bool变量，一般以is作为前缀。|isFirst();|
| | l 如果方法修改一个成员变量的值，方法名一般为：set + 成员变量名。| |
| | | |
|变量|l 第一个字母小写，中间单词的第一个字母大写。|String myName;|
| |l 不要用_或&作为第一个字母。|boolean mShowInvisible;|
| |l 尽量使用短而且具有意义的单词。| |
| |l  单字符的变量名一般只用于生命期非常短暂的变量。i,j,k,m,n一般用于integers；c,d,e一般用于characters。|int[] students;)|
| |l 如果变量是集合，则变量名应用复数。| |
| | | |
|常量|l  所有常量名均全部大写，单词间以‘_’隔开。|int MAX_NUM;



### 2.2 其它

开发人员如果遇到上述表格中未列举的类型，请书面通知相关管理人员，由管理人员集中更新列表内容。

## 3. 使用常量

### 3.1 使用常量

1. 常数很容易在数据输入时出错

常数存在的主要问题之一是你很容易在键入数字时出错，从而颠倒了数字的位置。例如，当你键入数字10876时，很容易的键入10867或18076。与处理变量和保留字的方法不同，编译器并不在乎颠倒了位置和不正确的数字，有时简单的错误造成的问题不会立即表现出来，而当问题表现出来时，它们会以随机的计算错误的形式出现，这些错误很难准确定位。用常量来取代常数时，编译器将在编译时检查常量的有效性。如果常量不存在，编译器便将这一情况通知你，并拒绝进行编译，这可以消除错误键入的数字带来的问题，只要常量拥有正确的值，使用该常量的所有代码也有使用该正确值。

2. 常数很难不断更新

3. 常量使代码更容易阅读

使用常量后，得到的一个额外好处是可使创建的代码更容易阅读。常数很不直观。也许你对常数非常了解，但其他人则根本看不明白。通过合理的给常量命名，使用这些常量的代码就变得比较直观了，更容易阅读。

为常量赋予较宽的作用域，这与使用变量时的情况不同。在一个应用程序中你决不应该两次创建相同的常量。如果你发现自己复制了一个常量，请将原始的常量说明转至较宽的作用域，直到该常量可供引用它的所有方法为止。

## 4. 变量

### 4.1 定义有焦点的变量

用于多个目的的变量称为无焦点（多焦点）的变量。无焦点变量所代表的意义与程序的执行流程有关，当程序处于不同位置时，它所表示的意义是不固定的，这样就给程序的可读性和可维护性带来了麻烦。

### 4.2 只对常用变量名和长变量名进行缩写

如果需要对变量名进行缩写时，一定要注意整个代码中缩写规则的一致性。例如，如果在代码的某些区域中使用Cnt，而在另一些区域中又使用Count，就会给代码增加不必要的复杂性。

变量名中尽量不要出现缩写。

### 4.3 使用统一的量词

通过在结尾处放置一个量词，就可创建更加统一的变量，它们更容易理解，也更容易搜索。例如，请使用strCustomerFirst和strCustomerLast，而不要使用strFirstCustomer和strLastCustomer。

量词列表：

|量词后缀|说明|
|--------|--------|
|First|一组变量中的第一个|
|Last|一组变量中的最后一个|
|Next|一组变量中的下一个变量|
|Prev|一组变量中的上一个|
|Cur|一组变量中的当前变量|

### 4.4 使用肯定形式的布尔变量

给布尔变量命名时，始终都要使用变量的肯定形式，以减少其它开发人员在理解布尔变量所代表的意义时的难度。

### 4.5 为每个变量选择最佳的数据类型

这样即能减少对内存的需求量，加快代码的执行速度，又会降低出错的可能性。用于变量的数据类型可能会影响该变量进行计算所产生的结果。在这种情况下，编译器不会产生运行期错误，它只是迫使该值符合数据类型的要求。这类问题极难查找。总而言之，请尽量避免隐式转换。

### 4.6 尽量缩小变量的作用域

如果变量的作用域大于它应有的范围，变量可继续存在，并且在不再需要该变量后的很长时间内仍然占用资源。

它们的主要问题是，任何类中的任何方法都能对它们进行修改，并且很难跟踪究竟是何处进行修改的。

占用资源是作用域涉及的一个重要问题。对变量来说，尽量缩小作用域将会对应用程序的可靠性产生巨大的影响。

## 5. 代码的格式化

### 5.1 对代码进行格式化时，要达到的目的

1. 通过代码分割成功能块和便于理解的代码段，使代码更容易阅读和理解；

2. 使用空行和注释行，将程序中逻辑上不相关的代码块分开。比如：变量声明部分和代码语句间的分隔；较长的方法中，完成不同功能的代码块间的分隔。要避免出现逻辑上混乱的分隔，如：某一逻辑功能代码块中间用空行进行了分隔，但是在相邻功能代码块之间却没有分隔，这样会给程序阅读者造成错觉。

3. 减少为理解代码结构而需要做的工作；

4. 使代码的阅读者不必进行假设；

5. 使代码结构尽可能做到格式清楚明了。

### 5.2 编程原则

1. 不要将多个语句放在同一行上

不论是变量声明，还是语句都不要在一行上书写多个。

2. 在if语句后缩进；

在else语句后缩进

在switch语句后缩进

在case语句后缩进

在do句后缩进

已经用行接续符分割的语句的各个行要缩进

对从属于行标注的代码进行缩进。

.       在执行统一任务的各个语句组之间插入一个空行。好的代码应由按逻辑顺序排列的进程或相关语句组构成。

## 6. 代码的注释

### 6.1 使用代码注释的目的

1. 文字说明代码的作用(即为什么要用编写该代码,而不是如何编写);

2. 确指出该代码的编写思路和逻辑方法;

3. 人们注意到代码中的重要转折点;

4. 使代码的阅读者不必在他们的头脑中仿真运行代码的执行方法.

### 6.2 编程原则

1. 用文字说明代码的作用:

简单的重复代码做写什么,这样的注释几乎不能给注释增加什么信息.如果你使用好的命名方法来创建直观明了的代码那么这些类型的注释绝对增加不了什么信息.

2. 如果你想违背好的编程原则,请说明为什么

有的时候你可能需要违背好的编程原则，或者使用了某些不正规的方法.遇到这种情况时,请用内部注释来说明你在做什么和为什么要这样做。

技巧性特别高的代码段，一定要加详细的注释，不要让其他开发人员花很长时间来研究一个高技巧但不易理解的程序段。

3. 用注释来说明何时可能出错和为什么出错

4. 在编写代码前进行注释

给代码加注释的方法之一是在编写一个方法前首先写上注释.如果你愿意,可以编写完整句子的注释或伪代码.一旦你用注释对代码进行了概述,就可以在注释之间编写代码.


5. 在要注释的代码前书写注释

注释一定出现在要注释的程序段前，不要在某段程序后书写对这段程序的注释，先看到注释对程序的理解会有一定帮助。

如果有可能，请在注释行与上面代码间加一空行。

6. 纯色字符注释行只用于主要注释

注释中要分隔时，请使用一行空注释行来完成，不要使用纯色字符，以保持版面的整洁、清晰。

7. 避免形成注释框

用星号围成的注释框，右边的星号看起来很好,但它们给注释增加了任何信息吗?实际上这会给编写或编辑注释的人增加许多工作。

8. 增强注释的可读性

注释是供人阅读的，而不是让计算机阅读的。

1) 使用完整的语句。虽然不必将注释分成段落（最好也不要分成段落），但你应尽量将注释写成完整的句子。

2) 避免使用缩写。缩写常使注释更难阅读，人们常用不同的方法对相同的单词进行缩写，这会造成许多混乱，如果必须对词汇缩写，必须做到统一。

3) 将整个单词大写，以突出它们的重要性。若要使人们注意注释中的一个或多个单词，请全部使用大写字母。（此处限于英文注释）

9. 对注释进行缩进，使之与后随的语句对齐。

注释通常位于它们要说明的代码的前面。为了从视觉上突出注释与它的代码之间的关系，请将注释缩进，使之与代码处于同一个层次上。


10. 为每个方法赋予一个注释标头

每个方法都应有一个注释标头。方法的注释标头可包含多个文字项，比如输入参数、返回值、原始作者、最后编辑该方法的程序员、上次修改日期、版权信息。

11. 当行尾注释用在上面这种代码段结构中时，它们会使代码更难阅读。

使用多个行尾注释时（比如用于方法顶部的多个变量说明），应使它们互相对齐。这可使它们稍容易阅读一些。

12. 何时书写注释

1) 请在每个if语句的前面加上注释。

2) 在每个switch语句的前面加上注释。与if语句一样，switch语句用于评估对程序执行产生影响的表达式。

3) 在每个循环的前面加上注释。每个循环都有它的作用，许多情况下这个作用不清楚直观。

### 6.3 注释那些部分

|项目|注释哪些部分|
|---|---|
|实参/|参数类型|
|参数|参数用来做什么|
| |任何约束或前提条件|
| |示例|
| | |
|字段/|字段描述|
|字段/属性|注释所有使用的变量、常量|
| |示例|
| | |
|类|类的目的|
| |已知的问题|
| | |
|编译单元|每一个类/类内定义的接口，含简单的说明|
| | |
|接口|目的|
| |它应如何被使用以及如何不被使用|
| | |
|局部变量|用处/目的|
| | |
|成员函数注释|成员函数做什么以及它为什么做这个|
| |哪些参数必须传递给一个成员函数|
| |成员函数返回什么|
| |已知的问题|
| |任何由某个成员函数抛出的异常|
| |可见性决策|
| |成员函数是如何改变对象的|
| |包含任何修改代码的历史|
| | |
|成员函数内部注释|控制结构|
| |代码做了些什么以及为什么这样做|
| |局部变量|
| |难或复杂的代码|
| |处理顺序|


### 6.4 示例

#### 6.4.1 块注释：

主要用来描述文件，类，方法，算法等。一般用在文档和方法的前面，也可以放在文档的任何地方。以‘/*’开头，‘*/’结尾。例：
```
……
/*
*     注释
*/
……
```

#### 6.4.2 行注释：

主要用在方法内部，对代码，变量，流程等进行说明。与块注释格式相似，但是整个注释占据一行。例：
```
……
/*    注释       */
……
```

#### 6.4.3 尾随注释：

与行注释功能相似，放在代码的同行，但是要与代码之间有足够的空间，便于分清。例：
```
int m=4 ;        /*    注释       */
```
如果一个程序块内有多个尾随注释，每个注释的缩进应该保持一致。

#### 6.4.4 行尾注释：

与行注释功能相似，放在每行的最后，或者占据一行。以‘//’开头。

#### 6.4.5 文档注释：

与块注释相似，但是可以被javadoc处理，生成HTML文件。以‘/**’开头，‘*/’结尾。文档注释不能放在方法或程序块内。例：
```
/**
注释
*/
```

## 7. 表达式和语句

### 7.1 每行应该只有一条语句。

### 7.2 if-else,if-elseif语句，任何情况下，都应该有“{”，“}”，格式如下：
```
if (condition)
{
statements;
}
else if (condition)
{
statements;
}
Else
{
statements;
}
```
### 7.3 for语句格式如下：
```
for (initialization; condition; update)
{
statements;
}
```
如果语句为空：
```
for (initialization; condition; update) ;
```
### 7.4 while语句格式如下：
```
while (condition)
{
statements;
}
```
如果语句为空:
```
while (condition);
```
### 7.5 do-while语句格式如下：
```
do
{
statements;
} while (condition);
```
### 7.6 switch语句，每个switch里都应包含default子语句,格式如下：
```
switch (condition)
{
case ABC:
statements;
/* falls through */
case DEF:
statements;
break;
case XYZ:
statements;
break;
default:
statements;
break;
}
```
### 7.7 try-catch语句格式如下：
```
try {
statements;
} catch (ExceptionClass e) {
statements;
} finally {
statements;
}
```
## 8. 调试方法

程序中应该有足够的调试信息，一般来讲调试信息必须指明此信息是在哪个文件、哪个方法、哪一行输出的。调试信息的文字描述不应采用缩写的方式，应当尽可能的描述清楚此调试信息的目的。如以下过于简短的调试信息：
```
Log.d(“xx”,”count is ”+count);
```
count指的是什么的计数呢？这就导致了模糊不清，降低了代码调试的效率。

以android常用的调试类Log为例，举个例子。

每个源文件中定义一个String 常量THIS，用来描述此文件名
```
private static final String THIS = ”PalmmessagerService”;
```
在需要输出调试信息的地方调用Log方法。注意，对于不同的情况请选择最合适的Log方法！如，在出现错误的地方调用Log.e
```
Log.e(THIS,”+++ error!there is something wrong!  +++”);
```
而一般情况下调用Log.d。如
```
Log.d(THIS,”+++  I have an apple +++”);
```
如同一个文件内，在不同的地方出现同样的调试信息，请写明各调试信息是在哪个方法中输出的。如：
```
Log.d(THIS,”+++ funca I have an apple +++”);

Log.d(THIS,”+++ funcb I have an apple +++”);
```