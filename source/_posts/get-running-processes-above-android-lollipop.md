---
title: Android获取运行进程解决方案分析
tags: [多进程]
categories: [Android]
date: 2017-06-07 17:26:10
---

在Android中获取**运行进程**这种需求在许多场合需要被使用到，但实际情况是在Android Lollipop即5.0后，Google开始收紧对底层权限控制。
下面就涉及的方法及我现在找到的解决方案来分析下，是如何解决这个问题的。

<!-- more -->

可以从原有的几个方法的API变化及调用返回看出。
- [getRunningAppProcesses()](https://developer.android.com/reference/android/app/ActivityManager.html#getRunningAppProcesses%28%29)在直到4.x， 5.0版本上工作良好（即便API中提示此方法仅用于debugging及编译管理UI之用），但从5.0+开始在一些OEM的系统中调用此方法进行测试会发现方法返回null。

- [getRunningTasks (int)](https://developer.android.com/reference/android/app/ActivityManager.html#getRunningTasks%28int%29)方法从5.0起正式被标记为deprecated（过时）。5.0+版本上的第三方应用无法在引用此方法。原因在于调用者可能利用此方法获取的私人信息，导致信息泄露。而为了向后兼容，在原有的版本中依然可以获取到至少调用应用本身的task信息及部分其他不敏感的信息。

从上述两个方法的变化可以看出在5.0+之后，想要获取运行进程越来越难。

在5.0~6.0版本上，利用此处的解决方法还可以获取当前运行的进程列表。
[AndroidProcesses](https://github.com/jaredrummler/AndroidProcesses "github地址")方案可以获取当前的运行进程列表。

这里的入口方法最常使用的是AndroidProcesses类的getRunningProcesses()。

在查看到具体实现后，就可以知道，这个解决方案一般的android开发人员是真想不到的，因为涉及到linux的文件系统，只有真正熟悉linux内核了解linux filesystem的开发人员才能想到此种办法。

这里主要涉及到文件系统中/proc目录。/proc文件系统目录是一个伪文件系统，它只存在内存当中，而不占用外存空间。它是在系统运行时产生于内存当中。linux通过这一伪文件系统，在运行时访问内核内部数据结构、改变内核设置的机制。

/proc目录下有这诸多的文件和目录
![/proc目录（gitbash示例）](/images/get-running-processes-above-android-lollipop/dir_proc.png)

在/proc目录下，进程名均是以数字命名的目录。因此获取运行进程信息，即是访问数字命名的目录，通过读取目录内特定文件来获取对应的进程名。

读取进程名分成以下几步：

 1. 读取进程目录下cmdline文件内容，android系统中该文件内包含的既是文件名。
![android-ps命令列举进程](/images/get-running-processes-above-android-lollipop/ps_list_processes_of_android.png)
![android-ps命令列举进程](/images/get-running-processes-above-android-lollipop/ps_list_processes_of_android_1.png)
通过ps命令可以查看到当前android中的进程。
然后再cat文件内容查看：
![cat cmdline文件内容](/images/get-running-processes-above-android-lollipop/cat_cmdline_content.jpg)

 2. cmdline内容也可能是空，具体原因不明。此种情况下，换做去读取/
 proc/pid/stat文件。
 stat内容格式：
 ![stat文件内容](/images/get-running-processes-above-android-lollipop/content_format_of_file_stat.png)
 内容是空格分割的多列，其中第二列既是当前进程的进程名。

 3. 以步骤1、2访问/proc目录内所有数字命名目录， 即可以获取到当前Android系统中正在运行的进程。

在获取进程之后，我们就可以根据需要进行某些操作了。

据说google在7.0上有加强了对/proc目录访问的控制，AndroidProcesses的解决方案就有问题了........

持续关注此问题后续可能的解决方案中......
