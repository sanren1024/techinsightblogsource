---
title: Android获取运行进程解决方案分析
tags: []
categories: []
date: 2017-06-07 17:26:10
---

在Android中获取**运行进程**这种需求在许多场合需要被使用到，但实际情况是在Android Lollipop即5.0后，Google开始收紧对底层权限控制。
下面就涉及的方法及我现在找到的解决方案来分析下，是如何解决这个问题的。

<!-- more -->

可以从原有的几个方法的API变化及调用返回看出。
- [getRunningAppProcesses()](https://developer.android.com/reference/android/app/ActivityManager.html#getRunningAppProcesses())在直到4.x， 5.0版本上工作良好（即便API中提示此方法仅用于debugging及编译管理UI之用），但从5.0+开始在一些OEM的系统中调用此方法进行测试会发现方法返回null。

- [getRunningTasks (int)](https://developer.android.com/reference/android/app/ActivityManager.html#getRunningTasks(int))方法从5.0起正式被标记为deprecated（过时）。5.0+版本上的第三方应用无法在引用此方法。原因在于调用者可能利用此方法获取的私人信息，导致信息泄露。而为了向后兼容，在原有的版本中依然可以获取到至少调用应用本身的task信息及部分其他不敏感的信息。

从上述两个方法的变化可以看出在5.0+之后，想要获取运行进程越来越难。

在5.0~6.0版本上，利用此处的解决方法还可以获取当前运行的进程列表。
[AndroidProcesses](https://github.com/jaredrummler/AndroidProcesses, "github地址")方案可以获取当前的运行进程列表。

这里的入口方法最常使用的是AndroidProcesses类的getRunningProcesses()。

在查看到具体实现后，就可以知道，这个解决方案一般的android开发人员是真想不到的，因为涉及到linux的文件系统，只有真正熟悉其filesystem的开发人员才能想到此种办法。


