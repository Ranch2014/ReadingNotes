# 第 4 条：多用类型常量，少用 #define 预处理指令

若定义动画播放时间间隔为常量，通常做法：

``` Objective-C
#define ANIMATION_DURAION 0.3
```

这样的预处理指令没有给出类型信息。下述方法比预处理指令定义常量更好（包含类型信息）：

``` Objective-C
static const NSTimeInterval kAnimationDuration = 0.3; //清楚地描述了常量的含义
```

>常量命名法：若常量局限于某“编译单元”（translation unit，也就是“实现文件”，implementation file），则在前面加字母 k；若常量在类之外可见，则常以类名为前缀。

定义常量的位置也很重要。若不打算公开某个常量，则应将其定义在使用该常量的实现文件中。

变量一定要同时用 `static` 和 `const` 来声明。若试图修改由 `const` 修饰符所声明的变量，那么编译器就会报错。

若声明此变量时不加 `static`，编译器会为它创建一个“外部符号”（external symbol）。此时若另一个编译单元也声明了同名变量，则会报错。
若一个变量既声明为 `static`, 又声明为 `const`，则编译器不会创建符号，而是会想 `#define` 预处理指令一样，把所有遇到的变量都替换为（有类型信息的）常值。

有时候需要对外公开某个常量，例如使用通知（NSNotificationCenter）。
定义方式：

``` Objective-C
// 头文件“声明”
extern NSString *const EOCStringConstant; //注意 const 位置

// 实现文件“定义”
NSString *const EOCStringConstant = @"VALUE";
```
常量定义应从右至左解读，在这里：EOCStringConstant 就是“一个常量，而这个常量是指针，指向 NSString 对象”。
关键字 `extern` 告诉编译器，在全局符号表中将会有一个名为 EOCStringConstant 的符号。

若要将 `EOCAnimatedView` 类中的动画时长对外公布，可声明如下：

``` Objective-C
// EOCAnimatedView.h
extern const NSTimeInterval EOCAnimatedViewAnimationDuration;

// EOCAnimatedView.m
const NSTimeInterval EOCAnimatedViewAnimationDuration = 0.3;
```

要点：
- 不要用预处理指令定义常量（不含类型信息）。
- 在实现文件中用 `static const` 来定义“只在编译单元内可见的常量”（translation-unit-specific constant）
- 在头文件中使用 `extern` 声明全局变量，并在相关实现文件定义其值。


>主要来源：[《Effective Objective-C 2.0》](http://book.douban.com/subject/25829244/)