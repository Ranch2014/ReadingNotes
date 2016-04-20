OC 语言由 Smalltalk (消息型语言的鼻祖) 演化而来。OC 使用“消息结构”（messaging structure），而非“函数调用”（function calling）。

>关键区别：
消息结构语言：运行时应执行代码由**运行环境**决定；
函数调用语言：由**编译器**决定。

OC 的重要工作都由“运行期组件”（runtime component）而非编译器完成。

OC 是 C 的“超集”（superset），C 语言的所有功能在 OC 依然适用。

OC 中的指针是指示对象的。想要声明一个变量，令其指代某个对象，语法如下：

```Objective-C
NSString *someString = @"The string"; 
//声明一个名为 someString 的变量，其类型是 NSString*, 即此变量为指向 NSString 的指针。
```

>对象所占内存分配在“堆空间（heap space）”，绝不会分配在“栈空间（stack space）”，不能在栈中分配 OC 对象。

someString 变量指向分配在堆里的某块内存，其中含有一个 NSString 对象。若再创建一个变量，令其指向同一地址，并不拷贝该对象，只是两个变量同时指向该对象：

``` Objective-C
NSString *someString = @"The string";
NSString *anotherString = someString;
```

只有一个 NSString 实例，两个变量指向此实例，都是 NSString* 类型。说明当前“栈帧”（stack frame）里分配了两块内存，每块大小都能容下一枚指针（32位架构的计算机上是4字节，64位计算机是8字节），两块内存里的值一样，都是 NSString 实例的内存地址。

![内存布局图](http://upload-images.jianshu.io/upload_images/147260-28b506ac0916c4be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

分配在堆中的内存必须直接管理，而分配在栈上用于保存变量的内存则会在其栈帧弹出时自动清理。OC 将堆内存管理抽象出来了，不需要 malloc 和 free 来分配和释放内存。OC 运行期环境把这部分工作抽象为一套内存管理架构，名为“引用计数”。

OC 中有些不含 * 的变量，可能会用到“栈空间”（stack space），这些变量保存的不是 OC 对象，例如 CoreGraphics 中的 CGRect:

``` c
CGRect frame;
frame.origin.x = 0.0f;
frame.origin.y = 1.0f;
frame.size.width = 100.0f;
frame.size.height = 150.0f;
```

CGRect 是 C 结构体，其定义：

``` c
struct CGRect {
    CGPoint origin;
    CGSize size;
};
typedef struct CGRect CGRect;
```

整个系统框架都在使用这种结构体。
原因：若改用 OC 对象来做的话，性能会受影响（与创建结构体相比，创建对象还需额外开销）。

要点：
- OC 为 C 语言添加了面向对象特性，是其超集。OC 使用动态绑定的消息结构，即，运行时才会检查对象类型。收到一条消息后，该执行何种代码，由运行期环境而非编译器决定。
- 理解 C 语言的核心概念有助于写好 OC 程序。尤其要掌握内存模型和指针。

>主要来源：[《Effective Objective-C 2.0》](http://book.douban.com/subject/25829244/)

PS: 初读这么有含量的书，加之 C 语言基础有点薄弱，觉得信息量好大。许多不理解的暂且记下，待日后慢慢消化吸收。