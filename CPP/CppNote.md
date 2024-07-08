## CMAKE

![1718682476768](image/CppNote/1718682476768.png)

![1718682483540](image/CppNote/1718682483540.png)

## 指针

![1718682047449](image/CppNote/1718682047449.png)

![1718682051007](image/CppNote/1718682051007.png)

## dllimport / dllexport

在Windows上进行开发程序通常会使用到dll, 微软希望通过dll机制加强软件的模块化设计，使得各模块之间能够松散的组合，重用以及升级。那么接下内容会介绍Windows C++中如何生成以及使用dll。

在生成dll的时候我们希望将我们的符号导出（符号就是程序中定义的变量或者方法），在使用dll时则时希望导入符号。通常由两种方式来实现符号表的导入导出。

1. _*declspec(dllimport) * *和 * __declspec(dllexport)

__declspec关键字可以用来修饰某个函数或者时变量，以及c++中的某个类，如果作用于某个class上则表示将class中的所有方法以及static data member 会被导出。

通常如果你要导出一个class，
你需要下面一段代码, 并且你需要在你的dll 工程中定义宏DLLEXPORT

#ifdef DLLEXPORT #define _DLL_DECLARE_
declspec(dllexport) #else #define _DLL_DECLARE_ declspec(dllimport) class
_DLL_DECLARE_ Math { public: double Add(double a, double b); double Sub(double
a, double b); };

如果你需要导出一个全局的函数那么你需要在dll的工程中显示的指出导出某个符号,例如下面的代码我们需要导出函数myFunc

__declspec(dllexport) double
myFunc(double a, double b);

在对应的使用该方法的地方需要显示的导入该方法才可以使用

__declspec(dllimport) double
myFunc(double a, double b);使用.def文件

2. ".def"文件是类似于ld连接器的连接脚本文件，可以被当作link连接器的输入文件，用域控制链接过程。“.def”文件中的IMPORT或者EXPORTS段可以用来声明导入导出符号。

[C++导入导出符号（dllimport/dllexport） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/407291193)

[C++编程笔记：dll的生成与使用_c++ dll 使用-CSDN博客](https://blog.csdn.net/elaine_bao/article/details/51784864)

dll 分三个模块；

![1718682416595](image/CppNote/1718682416595.png)

[为什么动态链接.dll和.lib都需要(详解静、动态链接库)_windows的动态链接库为什么多个lib文件-CSDN博客](https://blog.csdn.net/weixin_43744293/article/details/117283686)


## API 设计

第一种是PIMP方法，即Pointer to Implementation，在接口类成员中包含一个指向实现类的指针，这样可以最大限度的做到接口和实现分离的原则。

[PIMPL：这样写隐藏接口的实现细节 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/264820635)

第二种方法叫Object-Interface方法，它的思想是采用C++的动态功能，实现类继承接口类，功能接口函数定义成虚函数。

[如何设计C++接口类 - 简书 (jianshu.com)](https://www.jianshu.com/p/2f870b7a3434)

2.2 隐藏实现细节：

创建API的主要原因是隐藏所有的实现细节，以免将来修改API对已有客户造成影响。任何内部实现细节(那些很可能变更的部分)必须对该API的客户隐藏。主要有两种技巧可以达到此目标：物理隐藏和逻辑隐藏。物理隐藏表示只是不让用户获得私有源代码。逻辑隐藏则需要使用语言特性限制用户访问API的某些元素。

物理隐藏：在C和C++中，声明和定义是有特定含义的精确术语。 声明只是告诉编译器一个名字以及它的类型，并不为其分配任何内存

。与之相对，定义提供了类型结构体的细节，如果是变量则为其分配内存。声明告诉编译器某个标识符的名称及类型。定义提供该标识符的完整细节，即它是一个函数体还是一块内存区域。

物理隐藏表示将内部细节 (.cpp) 与公有接口(.h) 分离，存储在不同的文件中 。

逻辑隐藏：封装提供了限制访问对象成员的机制：public、protected、private。封装是将API的公有接口与其底层实现分离的过程。

逻辑隐藏指的是使用 C++ 语言中受保护的和私有的访问控制特性从而限制访问内部细节

。类的数据成员应该始终声明为私有的，而不是公有的或受保护的。

永远不要返回私有数据成员的非const指针或引用，这会破坏封装性。

强烈建议在API中采用Pimpl惯用法，这样就可以将所有实现细节完全和公有头文件分开。 Pimpl

惯用法：它将所有的私有数据成员隔离到一个.cpp 文件中独立实现的类或结构体中。之后，.h

文件仅需要包含指向该类实例的不透明指针(opaque pointer) 即可 。

将私有功能声明为.cpp文件中的静态函数，而不要将其作为私有方法暴露在公开的头文件中。(更好的做法是使用Pimpl惯用法。)

隐藏实现类：除了隐藏类的内部方法和变量之外，还应该尽力隐藏那些纯粹是实现细节的类。实际上，一些类仅用于实现，因此应该将其从API的公有接口中移除。

[C++ API设计-CSDN博客](https://blog.csdn.net/cuijian2B/article/details/122133521)

## 回调函数


C 语言回调函数详解[](https://www.runoob.com/w3cnote_genre/code)

1. 什么是回调函数？

百度百科的解析我觉得还算不错（虽然经常吐槽百度搜索...）：回调函数就是一个通过函数指针调用的函数。如果你把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时，我们就说这是回调函数。

下面先说说我的看法。我们可以先在字面上先做个分解，对于"回调函数"，中文其实可以理解为这么两种意思：1) 被回调的函数；2) 回头执行调用动作的函数。那这个回头调用又是什么鬼？

先来看看来自维基百科的对回调（Callback）的解析：In computer programming, a callback is any executable code that is passed as an argument to other code, which is expected to call back (execute) the argument at a given time. This execution may be immediate as in a synchronous callback, or it might happen at a later time as in an asynchronous callback. 也就是说，把一段可执行的代码像参数传递那样传给其他代码，而这段代码会在某个时刻被调用执行，这就叫做回调。如果代码立即被执行就称为同步回调，如果在之后晚点的某个时间再执行，则称之为异步回调。关于同步和异步，这里不作讨论，请查阅相关资料。

再来看看来自Stack Overflow某位大神简洁明了的表述：A "callback" is any function that is called by another function which takes the first function as a parameter。 也就是说，函数 F1 调用函数 F2 的时候，函数 F1 通过参数给 函数 F2 传递了另外一个函数 F3 的指针，在函数 F2 执行的过程中，函数F2 调用了函数 F3，这个动作就叫做回调（Callback），而先被当做指针传入、后面又被回调的函数 F3 就是回调函数。到此应该明白回调函数的定义了吧？

2. 为什么要使用回调函数？

很多朋友可能会想，为什么不像普通函数调用那样，在回调的地方直接写函数的名字呢？这样不也可以吗？为什么非得用回调函数呢？有这个想法很好，因为在网上看到解析回调函数的很多例子，其实完全可以用普通函数调用来实现的。要回答这个问题，我们先来了解一下回到函数的好处和作用，那就是解耦，对，就是这么简单的答案，就是因为这个特点，普通函数代替不了回调函数。所以，在我眼里，这才是回调函数最大的特点。来看看维基百科上面我觉得画得很好的一张图片。


下面以一段不完整的 C 语言代码来呈现上图的意思：

```cpp
#include
#include // 包含Library Function所在读得Software library库的头文件int Callback() // Callback Function
{
    // TODO
    return 0;
}
int main() // Main program
{
    // TODO
    Library(Callback);
    // TODO
    return 0;
}
```


乍一看，回调似乎只是函数间的调用，和普通函数调用没啥区别，但仔细一看，可以发现两者之间的一个关键的不同：在回调中，主程序把回调函数像参数一样传入库函数。这样一来，只要我们改变传进库函数的参数，就可以实现不同的功能，这样有没有觉得很灵活？并且丝毫不需要修改库函数的实现，这就是解耦。再仔细看看，主函数和回调函数是在同一层的，而库函数在另外一层，想一想，如果库函数对我们不可见，我们修改不了库函数的实现，也就是说不能通过修改库函数让库函数调用普通函数那样实现，那我们就只能通过传入不同的回调函数了，这也就是在日常工作中常见的情况。现在再把main()、Library()和Callback()函数套回前面 F1、F2和F3函数里面，是不是就更明白了？

明白了回调函数的特点，是不是也可以大概知道它应该在什么情况下使用了？没错，你可以在很多地方使用回调函数来代替普通的函数调用，但是在我看来，如果需要降低耦合度的时候，更应该使用回调函数。

3. 怎么使用回调函数？

知道了什么是回调函数，了解了回调函数的特点，那么应该怎么使用回调函数？下面来看一段简单的可以执行的同步回调函数代码。

实例

```cpp
#include<stdio.h>
int Callback_1() // Callback Function 1
{
    printf("Hello, this is Callback_1 ");
    return 0;
}
int Callback_2() // Callback Function 2
{
    printf("Hello, this is Callback_2 ");
    return 0;
}
int Callback_3() // Callback Function 3
{
    printf("Hello, this is Callback_3 ");
    return 0;
}
int Handle(int (*Callback)())
{
    printf("Entering Handle Function. ");
    Callback();
    printf("Leaving Handle Function. ");
}
int main()
{
    printf("Entering Main Function. ");
    Handle(Callback_1);
    Handle(Callback_2);
    Handle(Callback_3);
    printf("Leaving Main Function. ");
    return 0;
}
```


运行结果：

```
Entering Main Function.
Entering Handle Function.
Hello, this is Callback_1
Leaving Handle Function.
Entering Handle Function.
Hello, this is Callback_2
Leaving Handle Function.
Entering Handle Function.
Hello, this is Callback_3
Leaving Handle Function.
Leaving Main Function.
```

可以看到，Handle()函数里面的参数是一个指针，在main()函数里调用Handle()函数的时候，给它传入了函数Callback_1()/Callback_2()/Callback_3()的函数名，这时候的函数名就是对应函数的指针，也就是说，回调函数其实就是函数指针的一种用法。现在再读一遍这句话：A "callback" is any function that is called by another function which takes the first function as a parameter，是不是就更明白了呢？

4. 怎么使用带参数的回调函数？

眼尖的朋友可能发现了，前面的例子里面回调函数是没有参数的，那么我们能不能回调那些带参数的函数呢？答案是肯定的。那么怎么调用呢？我们稍微修改一下上面的例子就可以了：

实例

```cpp
#include <stdio.h>
int Callback_1(int x) // Callback Function 1
{
    printf("Hello, this is Callback_1: x = %d ", x);
    return 0;
}int Callback_2(int x) // Callback Function 2
{
    printf("Hello, this is Callback_2: x = %d ", x);
    return 0;
}int Callback_3(int x) // Callback Function 3
{
    printf("Hello, this is Callback_3: x = %d ", x);
    return 0;
}int Handle(int y, int (*Callback)(int))
{
    printf("Entering Handle Function. ");
    Callback(y);
    printf("Leaving Handle Function. ");
}int main()
{
    int a = 2;
    int b = 4;
    int c = 6;
    printf("Entering Main Function. ");
    Handle(a, Callback_1);
    Handle(b, Callback_2);
    Handle(c, Callback_3);
    printf("Leaving Main Function. ");
    return 0;
}
```


运行结果：

```
Entering Main Function.
Entering Handle Function.
Hello, this is Callback_1: x = 2
Leaving Handle Function.
Entering Handle Function.
Hello, this is Callback_2: x = 4
Leaving Handle Function.
Entering Handle Function.
Hello, this is Callback_3: x = 6
Leaving Handle Function.
Leaving Main Function.
```

可以看到，并不是直接把int Handle(int (*Callback)()) 改成 int Handle(int (*Callback)(int)) 就可以的，而是通过另外增加一个参数来保存回调函数的参数值，像这里 int Handle(int y, int (*Callback)(int)) 的参数 y。同理，可以使用多个参数的回调函数。

原文地址：[https://www.cnblogs.com/jiangzhaowei/p/9129105.html](https://www.cnblogs.com/jiangzhaowei/p/9129105.html)

[https://www.runoob.com/w3cnote/c-callback-function.html](https://www.runoob.com/w3cnote/c-callback-function.html)

## Design pattern 设计模式

当涉及到 C++ 中的设计模式时，有很多种不同的设计模式可以考虑。以下是一些常见的设计模式列表，它们可以帮助你更好地组织和理解代码：

1. 创建型模式（Creational Patterns）：

* 工厂模式（Factory Pattern）
* 抽象工厂模式（Abstract Factory Pattern）
* 建造者模式（Builder Pattern）
* 原型模式（Prototype Pattern）
* 单例模式（Singleton Pattern）

2. 结构型模式（Structural Patterns）：

* 适配器模式（Adapter Pattern）
* 桥接模式（Bridge Pattern）
* 组合模式（Composite Pattern）
* 装饰器模式（Decorator Pattern）
* 外观模式（Facade Pattern）
* 享元模式（Flyweight Pattern）
* 代理模式（Proxy Pattern）

3. 行为型模式（Behavioral Patterns）：

* 责任链模式（Chain of Responsibility Pattern）
* 命令模式（Command Pattern）
* 解释器模式（Interpreter Pattern）
* 迭代器模式（Iterator Pattern）
* 中介者模式（Mediator Pattern）
* 备忘录模式（Memento Pattern）
* 观察者模式（Observer Pattern）
* 状态模式（State Pattern）
* 策略模式（Strategy Pattern）
* 模板方法模式（Template Method Pattern）
* 访问者模式（Visitor Pattern）

## effective C++ 35 items

#### item 3 不要对数组使用多态；

数组名可以被看作指向其第一个元素的指针。然而，指针本身并不能支持多态，因为指针类型在编译时是固定的。

数组中的对象在内存中是连续存放的。不同类型的对象（如基类和派生类）在内存中的布局可能不同。如果直接在数组中存放多态对象，可能会导致内存对齐和对象大小的问题。

解决方案：

为了解决上述问题，通常会使用指针或智能指针来存储对象，这样可以保持多态性。

![1718683561149](image/CppNote/1718683561149.png)

在这个例子中，使用了 std::unique_ptr 来存储 Base 类型的对象，并在运行时决定调用哪个 func 函数。这保持了多态性，同时避免了数组的类型限制问题。

总结起来，C++ 中数组不能直接使用多态是由于数组类型在编译时固定、指针和数组的关系、对象内存布局不同等原因。为了实现多态，通常会使用指针或智能指针来存储对象

#### Item M4：避免无用的缺省构造函数

在《Effective C++》中，Scott Meyers 提出了一条重要的建议：避免无用的缺省构造函数（Item 4: Avoid unnecessary default constructors）。下面是这一建议的详细解释：

**背景**

缺省构造函数是指没有参数的构造函数。编译器会在没有定义任何构造函数的情况下，自动生成一个缺省构造函数。对于许多类来说，自动生成的缺省构造函数可能是无用的，甚至是有害的。

**问题**

1. 资源浪费

：缺省构造函数通常不会进行任何初始化操作，只是简单地分配内存。如果类的对象必须通过特定的参数进行初始化，那么这个缺省构造函数的存在就显得多余，而且还会浪费资源。

2. 增加代码复杂度

：提供一个无用的缺省构造函数可能会使代码变得复杂，难以维护。如果其他程序员误用了这个构造函数，可能会导致逻辑错误或未初始化的对象。

3. 增加出错概率

：如果缺省构造函数创建的对象没有初始化，那么在使用这些对象时，可能会遇到未定义的行为。这会增加程序出错的概率，并且难以调试。

4. 违背设计意图

：有些类需要通过参数初始化，例如那些依赖于外部资源（如文件、数据库连接等）的类。定义一个缺省构造函数违背了类设计的初衷。

**建议**

**避免定义无用的缺省构造函数，**特别是在以下情况下：

* 类的对象在创建时必须初始化。
* 类依赖于外部资源或特定的参数来正确工作。
* 类的默认构造函数无法保证对象的有效状态

#### Item M5：谨慎定义类型转换函数

* 隐式转换

：类型转换函数可能会被隐式调用，从而在不经意间将一个对象转换为另一种类型。这种隐式转换可能导致代码行为出乎意料，增加了调试和维护的难度。

* 歧义和优先级问题

：在重载解析（overload resolution）过程中，隐式转换可能会引发歧义，特别是当多个转换路径存在时，编译器需要在多种转换方案中做出选择，可能导致编译错误或者选择了不合适的转换路径。

* 性能问题

：不必要的类型转换会增加运行时的开销，影响程序性能。尤其是在频繁调用的代码路径中，隐式转换可能引入不可忽视的性能损耗。

* 设计上的不一致

：过度使用类型转换函数可能使类的设计变得不清晰，导致类接口的不一致性，破坏代码的可读性和可维护性。

为避免这种情况，可以使用  explicit 关键字，强制要求显式转换

```cpp
double amount = static_cast(c);  // 必须显式转换
```

#### Item M6：自增(increment)、自减(decrement)操作符前缀形式与后缀形式的区别

* 前缀形式

（prefix form）：操作符放在变量前面，例如 ++i 或 --i。

* 执行顺序：先进行自增/自减操作，再返回操作后的值。
* 语法定义：T& operator++(); 和 T& operator--();
* 后缀形式

（postfix form）：操作符放在变量后面，例如 i++ 或 i--。

* 执行顺序：先返回变量的值，然后再进行自增/自减操作。
* 语法定义：T operator++(int); 和 T operator--(int);，其中 int 参数是一个哑参数，用来区分前缀和后缀形式。

性能差异

* 前缀形式：通常比后缀形式更高效。原因是前缀形式直接返回操作后的值，不需要创建临时对象。
* 后缀形式：因为需要先保存变量的当前值，再进行自增/自减操作，然后返回保存的临时值，所以可能涉及创建临时对象，尤其是对于自定义类型，这会增加额外的开销。

```cpp
classMyClass {
    public:    MyClass& operator++() {  
        // 前缀形式，直接自增并返回当前对象  
        ++value;  
        return *this;  
    }

    MyClass operator++(int) {  
        // 后缀形式，需要创建临时对象保存当前值  
        MyClass temp = *this;   
        ++value;  
        return temp;  
    }
private:  
    int value;
};
```

在这种情况下，使用后缀形式  obj++ 会创建一个临时的 MyClass 对象 temp，这会带来额外的开销。而前缀形式 ++obj 则直接修改并返回当前对象，没有临时对象的创建。

### Item M7：不要重载“&&”,“||”, 或“,”

### Item M10：在构造函数中防止资源泄漏

* 使用 RAII（资源获取即初始化）：在 C++ 中，RAII 是一种常用的技术。它的核心思想是在对象的生命周期内管理资源的分配和释放。通过在构造函数中分配资源，在析构函数中释放资源，可以确保资源不会泄漏。即使在构造函数中发生异常，析构函数也会被调用，从而释放资源。
* 使用智能指针：智能指针（如 std::unique_ptr 和 std::shared_ptr）在 C++ 中是管理资源的好工具。它们能够自动管理资源的生命周期，并确保资源在不再使用时被释放。

如果你用对应的auto_ptr对象替代指针成员变量，就可以防止构造函数在存在异常时发生资源泄漏，你也不用手工在析构函数中释放资源，并且你还能象以前使用非const 指针一样使用const指针，给其赋值。 在对象构造中，处理各种抛出异常的可能，是一个棘手的问题，但是auto_ptr(或者类似于auto_ptr 的类)能化繁为简。它不仅把令人不好理解的代码隐藏起来，而且使得程序在面对异常的情况下也能保持正常运行。

* try-catch 此外，还可以使用 try-catch 块来捕获异常并释放资源。

  ![1718683744479](image/CppNote/1718683744479.png)

[https://www.mwrf.net/tech/basic/2023/30081.html](https://www.mwrf.net/tech/basic/2023/30081.html)
