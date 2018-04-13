title: Kotlin基础
tags:
  - Kotlin
  - Android
categories:
  - Kotlin
author: 散人
date: 2018-04-13 17:38:00
---

Kotlin是一门静态类型语言且支持类型推导，允许维护正确性与性能的同时保持源码简洁。Kotlin支持面向对象和函数式两种编码风格，通过头等函数使更高级别的抽象成为可能，通过支持不可变值简化了测试和多线程开发。

<!-- more -->

Kotlin在服务器应用程序上也可以运行的很好，全面支持所有现存的Java框架。且Kotlin在Android上也可以工作，这得益于对Android API特殊的编译器支持以及丰富的库，为常见Android开发任务提供了Kotlin友好的函数。

它是免费和开源的，全面支持主流的IDE和构建系统。

<table><tr><td bgcolor=#A9A9A9>
本文内容：
<br/>- 声明函数、变量、类、枚举以及属性
<br/>- Kotlin中的控制结构
<br/>- 智能转换
<br/>- 抛出和异常处理
</td></tr></table>

### 函数和变量
#### 函数
首先看著名的hello world代码：
```kotlin
fun main(args: Array<String>) {
    println("Hello World!")
}
```

这里给出了最基本的函数声明。
- 关键字fun用来声明一个函数。
- 参数类型写在变量之后。
- Kotlin中每行代码结尾省略了分号。

再看一段函数声明：
```kotlin
fun compare(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```

在函数后边紧跟着函数返回类型，返回类型与函数声明之间用冒号分隔。

这里可以看到，if是一个表达式，而不是语句。语句与表达式的区别在于，表达式有值，语句并没有自己的值。在Kotlin中，除了循环（for, while, do..while）之外，大多数控制结构都是表达式。

提到函数，Kotlin中有一个表达式函数体的概念。 即函数体由单个表达式构成，可以用这个表达式作为完整的函数体，并去掉花括号和return语句。
```kotlin
fun compare(a: Int, b: Int): Int = if (a > b) a else b
```
如果函数体在花括号中，这个函数是代码块体函数。

这里的compare函数还可以进一步简化，如下:
```kotlin
fun compare(a: Int, b: Int) = if (a > b) a else b
```
即省略函数返回类型。那么会有疑问，为什么函数可以不要返回值类型？？作为一门静态语言，Kotlin应该要求所有的表达式应该在编译期间就有类型？事实上，Kotlin的每个变量和表达式独有类型，每个函数也都有返回类型。而对表达式函数体来说，编译器会分析作为函数体的表达式，并把它的类型作为函数的返回类型，即使没有显示地显示出来。这种分析叫类型推导。

#### 变量
了解Java的开发人员度知道，java在声明变量的时候会以类型开始。Kotlin中不需要这么麻烦，许多的变量声明的类型都可以省略。Kotlin中变量的声明是以关键字开始，变量名称最后加上类型（不加也可以）。

例如：
```kotlin
val number = 3
val literal = "Hello World!"
```

可以看到这里的变量声明也没有加上类型声明，为何？这个原理跟表达式函数体一样，编译器会分析表达式值得类型，并把它的类型作为变量的类型。

<font color=red size=4>但是不是所有的情况都可以省略变量类型，最直接的情况就是在没有对变量进行初始化的情况下，这个时候就需要在声明中加上变量的类型。</font>

例如：
```kotlin
val number
number = 4
```
上边这样的声明方式，编译器会提示错误
```
Error: This vairable must either have a type annotation or be initialized.
```

这就说明了Kotlin中变量声明要么一开始就进行初始化而不加类型，要么不进行初始化但必须要加上变量类型。

因为如果不能提供可以赋给这个变量的值得信息，编译器就无法推导出它的类型，进而无法知道变量的类型。

Kotlin中的变量可分为可变变量和不可变量：
  - val —— 不可变引用。val声明的变量赋值之后不可更改。与Java的final对应。
  - var —— 可变引用。var声明的变量的值可变。与Java中的一般的变量相当。

默认情况下，Kotlin中尽可能的将变量声明为val，只在需要的时候换成var。

另外，声明了val变量的代码块中，如果编译器分析后可以确保变量在整个执行过程中只会有唯一一次的赋值行为，那么也是可以的。
```kotlin
val average: Int
if (isMain) {
    average = 8
} else {
    average = 5
}
```

另外还有与Java类似的是，val引用本身是不可变的，但其指向的对象是可以改变的。
```kotlin
val programs = arrayListOf("Java")
programs.add("Kotlin")
```

val关键字目前就是这么些注意的内容，再来谈谈var。
var声明的变量虽然是可变的，但是变量的类型是不可变的。 编译器只会根据变量初始化器来推导变量的类型，在确认类型之后不会再考虑后续赋值操作。
```kotlin
var count = 1
count = "hello"
```
这段变量声明是不合法的，变量count的类型在初始化为1的时候已经确认了是Int类型，再去给其赋值为String类型，这是编译器回给出错误提示。
```
Type mismatch: Infer type is String but Int was expected
```

顺便提下，Kotlin中的字符串格式化：字符串模板
先看一段简单的代码
```kotlin
fun main(args: Array<String>) {
    val language = if (args.size > 0) args[0] else "Kotlin"
    println("Hello $language!")
}
```

这段代码中可以看到，变量language前边有$符号，这种写法就是需要说的Kotlin中的新特性，字符串模板。字符串模板的作用即是可以在程序的字符串中引用变量。这种表达写法相当于在Java中使用StringBuilder方式的append()方法，或者是Java中"Hello " + language + "!"写法。但是相比之下，Kotlin的写法可读性更好。

字符串模板可以有更加复杂的表达方式——${表达式语句}
可以将上述代码进行一定的修改
```kotlin
fun main(args: Array<String>) {
    println("Hello ${if (args.size > 0) args[0] else "Kotlin"}!")
}
```

这样的写法也是对的，其中可以看到花括号中又有一对引号。在Kotlin中可以在双引号中嵌套双引号，但需要在表达式内部（即在花括号内）。


