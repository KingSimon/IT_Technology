# Java中Native关键字
JNI是Java Native Interface的 缩写。从Java 1.1开始，Java Native Interface (JNI)标准成为java平台的一部分，它允许Java代码和其他语言写的代码进行交互。JNI一开始是为了本地已编译语言，尤其是C和C++而设计 的，但是它并不妨碍你使用其他语言，只要调用约定受支持就可以了。

使用java与本地已编译的代码交互，通常会丧失平台可移植性。但是，有些情况下这样做是可以接受的，甚至是必须的，比如，使用一些旧的库，与硬件、操作系统进行交互，或者为了提高程序 的性能。JNI标准至少保证本地代码能工作在任何Java 虚拟机实现下。

JNI（Java Native Interface）的书写步骤

* 编写带有native声明的方法的java类
* 使用javac命令编译所编写的java类
* 使用javah ?jni java类名生成扩展名为h的头文件
* 使用C/C++（或者其他编程想语言）实现本地方法
* 将C/C++编写的文件生成动态连接库

## 1. 编写java程序：
这里以HelloWorld为例。
代码1：
```
class HelloWorld {
    public native void displayHelloWorld();

    static {
        System.loadLibrary("hello");
    }

    public static void main(String[] args) {
        new HelloWorld().displayHelloWorld();
    }
}
```
声明native方法：如果你想将一个方法做为一个本地方法的话，那么你就必须声明改方法为native的，并且不能实现。其中方法的参数和返回值在后面讲述。
Load 动态库：System.loadLibrary("hello");加载动态库（我们可以这样理解：我们的方法displayHelloWorld()没 有实现，但是我们在下面就直接使用了，所以必须在使用之前对它进行初始化）这里一般是以static块进行加载的。同时需要注意的是 System.loadLibrary();的参数“hello”是动态库的名字。
main()方法

## 2. 编译没有什么好说的了
javac HelloWorld.java

## 3. 生成扩展名为h的头文件
javah ?jni HelloWorld
头文件的内容：

```
/* DO NOT EDIT THIS FILE - it is machine generated */
#include
/* Header for class HelloWorld */

#ifndef _Included_HelloWorld
#define _Included_HelloWorld
#ifdef __cplusplus
extern "C" {
#endif
/*
* Class: HelloWorld
* Method: displayHelloWorld
* Signature: ()V
*/
JNIEXPORT void JNICALL Java_HelloWorld_displayHelloWorld
(JNIEnv *, jobject);

#ifdef __cplusplus
}
#endif
#endif

```
（这里我们可以这样理解：这个h文件相当于我们在java里面的接口，这里声明了一个Java_HelloWorld_displayHelloWorld (JNIEnv *, jobject);方法，然后在我们的本地方法里面实现这个方法，也就是说我们在编写C/C++程序的时候所使用的方法名必须和这里的一致）。

## 4. 编写本地方法
实现和由javah命令生成的头文件里面声明的方法名相同的方法。
代码2：

```
#include
#include "HelloWorld.h"
#include

JNIEXPORT void JNICALL Java_HelloWorld_displayHelloWorld(JNIEnv *env, jobject obj)
{
printf("Hello world!/n");
return;
}
```

注 意代码2中的第1行，需要将jni.h（该文件可以在%JAVA_HOME%/include文件夹下面找到）文件引入，因为在程序中的JNIEnv、 jobject等类型都 是在该头文件中定义的；另外在第2行需要将HelloWorld.h头文件引入（我是这么理解的：相当于我们在编写java程序的时候，实现一个接口的话 需要声明才可以，这里就是将HelloWorld.h头文件里面声明的方法加以实现。当然不一定是这样）。然后保存为HelloWorldImpl.c就 ok了。

## 5. 生成动态库
这里以在Windows中为例，需要生成dll文件。在保存HelloWorldImpl.c文件夹下面，使用VC的编译器cl成。
cl -I%java_home%/include -I%java_home%/include/win32 -LD HelloWorldImp.c -Fehello.dll
注 意：生成的dll文件名在选项-Fe后面配置，这里是hello，因为在HelloWorld.java文件中我们loadLibary的时候使用的名字 是hello。当然这里修改之后那里也需要修改。另外需要将-I%java_home%/include -I%java_home%/include/win32参数加上，因为在第四步里面编写本地方法的时候引入了jni.h文件。

## 6. 运行程序
java HelloWorld就ok。

JNI（Java Native Interface）调用中考虑的问题

在首次使用JNI的时候有些疑问，后来在使用中一一解决，下面就是这些问题的备忘：

### 1. java和c是如何互通的？
      其实不能互通的原因主要是数据类型的问题，jni解决了这个问题，例如那个c文件中的jstring数据类型就是java传入的String对象 ，经过jni函数的转化就能成为c的char*。
      对应数据类型关系如下表：
        Java 类型 本地c类型 说明
        boolean jboolean 无符号，8 位
        byte jbyte 无符号，8 位
        char jchar 无符号，16 位
        short jshort 有符号，16 位
        int jint 有符号，32 位
        long jlong 有符号，64 位
        float jfloat 32 位
        double jdouble 64 位
        void void N/A

### 2. 如何将java传入的String参数转换为c的char*，然后使用?
java 传入的String参数，在c文件中被jni转换为jstring的数据类型，在c文件中声明char* test，然后test = (char*)(*env)->GetStringUTFChars(env, jstring, NULL);注意：test使用完后，通知虚拟机平台相关代码无需再访问：(*env)->ReleaseStringUTFChars(env, jstring, test);

### 3. 将c中获取的一个char*的buffer传递给java？
这个char*如果是一般的字符串的话，作为string传回去就可以了。如果是含有’/0’的buffer，最好作为bytearray传出，因为可以制定copy的length，如果copy到string，可能到’/0’就截断了。

有两种方式传递得到的数据：

    *  一种是在jni中直接new一个byte数组，然后调用函数(*env)->SetByteArrayRegion(env, bytearray, 0, len, buffer);将buffer的值copy到bytearray中，函数直接return bytearray就可以了。
    * 一种是return错误号，数据作为参数传出，但是java的基本数据类型是传值，对象是传递的引用，所以将这个需要传出的byte数组用某个类包一下，如下：
```
class RetObj
{
public byte[] bytearray;
}
```
这个对象作为函数的参数retobj传出，通过如下函数将retobj中的byte数组赋值便于传出。代码如下：
```
jclass cls;
jfieldID fid;
jbyteArray bytearray;
bytearray = (*env)->NewByteArray(env,len);
(*env)->SetByteArrayRegion(env, bytearray, 0, len, buffer);
cls = (*env)->GetObjectClass(env, retobj);
fid = (*env)->GetFieldID(env, cls, "retbytes", "[B"]);
(*env)->SetObjectField(env, retobj, fid, bytearray);
```

### 4. 不知道占用多少空间的buffer，如何传递出去呢？
 在jni的c文件中new出空间，传递出去。java的数据不初始化，指向传递出去的空间即可。