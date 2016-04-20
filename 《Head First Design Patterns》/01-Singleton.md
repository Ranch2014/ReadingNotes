#1. 为什么需要单例模式
有些对象我们只需要一个，例如：线程池、缓存、对话框、注册表、日志等。而且，这类对象只能有一个实例，如果制造出多个实例，就会导致许多问题，例如：程序的行为异常、资源使用过量、或者前后不一致的结果。

#2. 单例模式定义
>The Singleton Pattern ensures a class has only one instance, and provides a global point of access to it.

>单件模式确保一个类只有一个实例，并提供一个全局访问点。

##2.1 经典的单例模式
Java 代码如下：

``` java
public class Singleton {
    public static Singleton uniqueInstance;

    // other useful instance variables here

    private Singleton() {}

    public static Singleton getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }

    //other useful methods here

    public String getDescription() {
        return "I'm a classic Singleton!";
    }
}
```
这种方式又称“懒汉式”（惰性实例化），即声明变量的时候不进行实例化，只有调用方法时才进行实例化。

>注意：   
1. 这里的构造方法是 private 类型的，外部类无法对其实例化。   
2. 若有多个线程执行 getInstance() 方法，则会带来线程安全问题，即可能实例化多个对象。

解决方法：使用同步（synchronized）即下面的线程安全的单例模式。

##2.2 线程安全的单例模式
Java  示例代码如下：

``` java
public class Singleton {
    private static Singleton uniqueInstance;

    //other useful instance variables here 

    private Singleton() {}

    // 注意此处加了 synchronized 关键字
    public static synchronized Singleton getInstance(){
        if(uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }

    //other useful methods here

    public String getDescription() {
        return "I'm a thread safe Singleton!";
    }   
}
```

同步能解决线程安全问题，但也带来了新的问题：程序性能会降低。因此每次执行该方法时都需要进行同步。   
而且，该方法只有第一次执行时才真正需要同步，一旦设置好 uniqueInstance 后就不再需要同步这个方法了。

如何解决：

- 不作处理   
适用情况：对性能无特别要求。此时可不作处理。  
- 改“延迟”实例化（又称“懒汉式”）为“急切”实例化（又称“饿汉式”） 
示例代码：

``` Java
public class Singleton {
    public static Singleton uniqueInstance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return uniqueInstance;
    }
}
```

这种方式又称“饿汉式”，即声明变量时就进行实例化。

- 双重检查加锁  
这里进行了两次判断，若没有实例化才进行同步，然后实例化；以后就无需进入同步，直接返回该对象即可。因此只需进行一次同步。 
示例代码：

``` java
public static Singleton getInstance() {
    if(uniqueInstance == null) {
        synchronized (Singleton.class) {
            if (uniqueInstance == null) {
                uniqueInstance = new Singleton();
            }
        }
    }
    return uniqueInstance;
}
```

>[《Head First Design Patterns》](https://book.douban.com/subject/1400656/)  笔记