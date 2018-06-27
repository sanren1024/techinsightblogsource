---
title: Kotlin基础——函数定义及调用

tags:
  - Kotlin

categories:
  - Kotlin
 
author: 散人

date: 2018-06-8 20:15:47

---

#### 前言
函数是一门语言中最基本也是最重要的环节之一。这里就来说说Kotlin的函数部分的一些内容。

#### 顶层函数
我们知道的是Java中的方法定义离不开类，即所定义的方法必须在某个类中，即使一个类中只有一个单一的方法。

有时会遇到这样的情况，针对两个不同对象的处理方式相差无几，这样的方法定义不想依赖于具体的示例对象，这个时候一般的做法是将对应的方法定义到一个Util方法中。

Kotlin针对这种情况，则提供了顶层函数的定义，即无需将函数定义到某一个类中，直接定义在代码文件的顶层。

如下的示例代码：
```Kotlin
package kt  
  
fun sayHelloWorld() {  
    println("hello world!")  
}
```

这就是一个最基本的顶层函数的定义。相应的，它被编译成Java就变成了

```Java
package kt;  

public final class TopMethodKt {  
   public static final void sayHelloWorld() {  
      String var0 = "hello world!";  
      System.out.println(var0);  
  }  
}
```

可以到，Kotlin文件TopMethod被编译成了TopMethodKt类，相应的顶层函数sayHelloWorld方法变成了静态方法。

Kotlin中顶层函数调用非常简单

```Kotlin
import kt.sayHelloWorld  
  
fun main(args: Array<String>) {  
    sayHelloWorld()  
}
```

这里可以看到，顶层函数像类一样被直接导入，然后直接调用即可。
若从Java中来调用这个Kotlin函数则需要导入类。

```Java
import static kt.TopMethodKt.sayHelloWorld;  
  
public class Main {  
    public static void main(String[] args) {  
        sayHelloWorld();  
  }  
}
```

另外由于Kotlin中顶层函数在编译后默认被放置在 [文件名Kt] 样式的类中，在Kotlin中调用没有任何问题，由于不需要前缀类方式的调用。若在Java中需要调用这个类，通常需要这个方法在一个与之相关的功能类名中。

这样就可能遇到这种情况：Kotlin中定义相对比较随意，kt文件直接命名为TopMethod等，Java中需要根据具体业务功能来命名方法所在的类。

这种情况，可以通过修改文件类名方式解决。就需要通过标注<font color='red'>@JvmName</font>进行解决，而且标注需要在文件第一行。

```Kotlin
@file:JvmName("Greet")  
package kt
  
fun sayHelloWorld() {  
    println("hello world!")  
}
```

然后重新编译，就会发现原来的TopMethodKt.class会变成Greet.class。
![原Kotlin文件编译后](/images/kotlin-in-action-basics-method/kotlin-in-action-method-prejvmname.png) ->![添加@JvmName标注编译后](/images/kotlin-in-action-basics-method/kotlion-in-action-method-jvmname.png)


