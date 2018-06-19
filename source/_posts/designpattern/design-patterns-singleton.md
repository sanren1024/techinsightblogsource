title: 设计模式之单例模式
tags:
  - Kotlin
  - 设计模式
categories:
  - 设计模式
author: 散人
date: 2018-06-19 21:47:00
---

单例模式应该是所有设计模式中最有名的设计模式了，原理简单：一个类型的实例在全局中只有一个对象，要调用这个实例的方法必须经由这个单例来完成。

最简单的Java实现方式:

```Java
public class Singleton {  
    private static final Singleton INSTANCE = new Singleton();  
    private Singleton() {}
    public static Singleton getInstance() {  
        return INSTANCE;  
    }  
}
```

对应Kotlin实现：

```Kotlin
object Singleton
```

这里是最简单的实现方式，但对于不想一开始就创建对象，即延迟加载（仅在第一次使用它时才初始化）。

```Java
public class Singleton {  
    private static Singleton INSTANCE;  
  
    private Singleton() {}  
  
    public static Singleton getInstance() {  
        if (INSTANCE == null) {  
            INSTANCE = new Singleton();  
        }  
        return INSTANCE;  
  }  
}
```

对应Kotlin实现：

```Kotlin
class Singleton private constructor() {  
    companion object {  
        val INSTANCE: Singleton by lazy { Singleton() }  
    }  
}
```

#### 为什么要延迟初始化呢？？
在《Java设计模式》(Steven John  Metsker, William C. Wake)一书中这样给出答案：

 1. 静态初始化时，没有足够信息对单例对象进行初始化。如工厂单例需要真正的工程类型才能建立起通信通道;
 2. 延迟加载也和资源获取有关，如数据库连接，尤其是在一个特定的会话中，它包含的应用程序并不需要该单例对象时。

换言之，采用延迟加载一般场景下均可以采用，而在某些场景下需要知道具体类型信息后才能创建对应对象时则必须采用延迟加载。而饿汉式加载方式在普通场景下均可以使用。

#### 单例和多线程
要在一个有多线程需求的项目中使用单例模式则需要小心，很可能因为线程运行调用流程造成系统死锁或者其他混乱情况。
假设有线程A跟线程B，同时需要访问单例Singleton（以上述懒汉单例为例）对象来进行某项活动，获取Singleton对象并执行某些行为。

```Java
public class Singleton {  
    private static Singleton INSTANCE;  
  
    private Singleton() {}  
  
    public static Singleton getInstance() {  
        if (INSTANCE == null) {  
            INSTANCE = new Singleton();  
        } 
        //.... 
        return INSTANCE;  
  }  
}
```

当线程A调用getInstance()并执行到if语句判断INSTANCE == null时，线程B也执行到此处，而事前并为针对此种情况添加访问锁，最终出现两个线程初始化同一个实例的情况。
这时就需要添加锁机制来进行排他性执行，即当线程A调用方法getInstance()进行INSTANCE == null判断并可能实例化时，线程B进行等待，待线程A执行完毕后，释放锁，线程B再进行执行。

```Java
public class Singleton {
    private static final Object LOCK = new Object(); // LOCK = Singleton.class
    private static Singleton INSTANCE;

    private Singleton() {}

    public static Singleton getInstance() {
        synchronized (LOCK) {
            if (INSTANCE == null) {
                INSTANCE = new Singleton();
            }
            return INSTANCE;
        }
    }
}
```

对应的Kotlin实现：

```Kotlin
class Singleton private constructor() {
    companion object {
        val INSTANCE: Singleton by lazy(LazyThreadSafetyMode.SYNCHRONIZED) { Singleton() }
    }
}
```

这样添加了锁之后，当线程A执行方法getIntance()时，线程B就发现方法被锁了，从而等待执行，当线程A执行完毕并释放锁LOCK时，线程才开始执行getInstance()方法。

PS：延迟加载和同步锁即是单例模式的双重检查。

