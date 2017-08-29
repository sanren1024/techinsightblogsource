---
title: Android多进程app中Application回调onCreate()方法被执行多次分析及解决
tags: [Application, 多进程 ]
categories: [Android]
date: 2017-05-28 15:41:00
---

>  问题描述

最近工作中碰到一个问题，在优化app，使用DDMS查看Application log过程中看到，app启动了三个进程，一个主进程，两个附带的进程。如下图可看到一个app启动的三个进程。

<!-- more -->

![启动三个进程](/images/android-multiprocess-oncreate-executed-serval-times/processes_app_starts.png "启动进程")
自定义Application回调方法onCreate()被执行了3次。开始不知是何原因。

##### 相关知识

> android:process

从Android开发者文档中的manifest中进程配置***android:process***可以获知:
正常情况下，应用程序的所有组件运行在一个默认的进程名下，因此不需要使用这个属性。但在需要的情况下，可以通过使用这个属性来覆盖默认进程，这样一个app就跨越多个进程。

如果这个属性值以冒号（“:”）开始，说明新进程相对于应用程序是一个私有进程，且组件运行在此进程中。若属性值以小写字符开始，那么新进程即是一个全局进程，组件运行在这个全局进程中。这也意味着其他应用程序组件可以与此进程进行通信，减少资源使用。

标签*&lt;application&gt;*的*android:process*属性可以为整个app内组件设置一个默认的运行进程。

manifest中组件标签*&lt;activity&gt;*， *&lt;service&gt;*， *&lt;provider&gt;*,  *&lt;receiver&gt;*都支持配置*android:process*，即每个组件均可以创建运行在自己的一个新进程中。

> Application类

我们可以自定义继承Application类来实现自己的Application，然后在其中的onCreate()方法中进行一定的初始化工作。

若自定义了Application类，那么需要注意的就是这个类在当app中有多个进程时，每个进程启动时都会初始化一次Application。在Android中很不幸的就是我们无法为每个新创建的进程来分别创建一个Application类。
![多个进程启动的Application状态](/images/android-multiprocess-oncreate-executed-serval-times/android_multiprocesses_application.jpg)

---
#### 解决方案

> 第一：getRunningAppProcesses()

每个进程对应一个application，这样可以通过针对特定进程名，进行相应的初始化工作，避免资源浪费，执行时间消耗。

**因为Application的执行时间影响着首个activity，service等的启动时间。**即Application执行时间越长，首个组件（activity）启动时间越晚，给用户造成的感觉就是应用启动速度特别慢。
![3个进程启动耗时](/images/android-multiprocess-oncreate-executed-serval-times/original_processes_start_time_long.png "3个进程创建耗时")

可以看出，Application回调方法onCreate()被执行3次，均执行耗时操作，这样造成了在点击应用logo后，到看到进入app，首个页面（Activity）启动，耗时将近6s，外加处理器速度，在较慢的机器上，这个时间可能更长，甚至超过10s。

目前较多采用的方法既是所提到的根据具体进程来进行相应的初始化工作，核心的获取对应进程的方法如下：

    /**
     * 获取进程名。
     * 由于app是一个多进程应用，因此每个进程被os创建时，    
     * onCreate()方法均会被执行一次，
     * 进行辨别初始化，针对特定进程进行相应初始化工作，
     * 此方法可以提高一半启动时间。
     *
     * @param context 上下文环境对象
     *
     * @return 获取此进程的进程名
     */
    private String getProcessName(Context context) {
        ActivityManager am = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningAppProcessInfo> runningAppProcesses = am.getRunningAppProcesses();
        if (runningAppProcesses == null) {
            return "";
        }

        for (ActivityManager.RunningAppProcessInfo runningAppProcess : runningAppProcesses) {
            if (runningAppProcess.pid == android.os.Process.myPid()
                    && !TextUtils.isEmpty(runningAppProcess.processName)) {
                return runningAppProcess.processName;
            }
        }
        return "";
    }
    
这样经过测试，在不同的进程被创建时，进行不同的工作，执行时间可以缩短一半。
![修改3个进程启动耗时](/images/android-multiprocess-oncreate-executed-serval-times/processes_start_timelong_after_modification.png "修改后3个进程创建耗时")

***<font color="red">虽然这种方法在某些版本上可以奏效，在但Android 5.0+版本中，由于Google开始收紧对Android底层权限管理，在趋势上方法[getRunningAppProcesses()](https://developer.android.com/reference/android/app/ActivityManager.html?hl=zh-cn#getRunningAppProcesses())将会被毙掉。因为已经在一些版本的环境中，此方法返回null。</font>***

> 第二：[UsageStatsManager](https://developer.android.com/reference/android/app/usage/UsageStatsManager.html)

使用类UsageStatsManager来获取运行的apps列表，但是使用这个类需要添加一个权限[PACKAGE_USAGE_STATS](https://developer.android.com/reference/android/Manifest.permission.html#PACKAGE_USAGE_STATS)，而此权限是系统权限，要使用必须到Settings应用中去针对应用进行授权（我们的app用户肯定不会愿意多次一步）。另外，据称有些OEM厂商已经删除了此项设置，换言之在Settings中找不到授权入口。

因此这个途径也就被毙掉了。

> 最终方案

经过几天google方案及针对可能的解决方法进行测试，下边的这个感觉比较靠谱。

这是一个开源项目，项目地址[点击这里](https://github.com/jaredrummler/AndroidProcesses)。

在我们的Application中集成并测试了该方法。

源码中添加一个方法，类似于使用getRunningAppProcesses()方法一样：
![解决方案代码](/images/android-multiprocess-oncreate-executed-serval-times/application_oncreate_call_several_times_solution.png)

以下是在三个不同的Android版本进行测试结果：

![Android 4.4.2测试结果](/images/android-multiprocess-oncreate-executed-serval-times/android_multi_processes_solution_test_on_4_4_2.png)

![Anroid 5.1.1测试结果](/images/android-multiprocess-oncreate-executed-serval-times/android_multi_processes_solution_test_on_5_1_1.png)

![Android 6.0测试结果](/images/android-multiprocess-oncreate-executed-serval-times/android_multi_processes_solution_test_on_6_0.png)

可以看到，针对这三个版本是可以达到相关初始化代码只执行一次的效果。这样可以缩短启动消耗的时间。

更多版本类型测试大家可以自行进行测试。

<font color="red">时间原因，此方案的代码及解决方法还没有来得及跟踪，有时间在做分析......</font>

这种方案也有限制：

-  一些版本的系统应用不包括在内，因为他们具有更高级别的[SElinux context](http://blog.csdn.net/innost/article/details/19299937)；

-  这种方法也不是[getRunningAppProcesses()](https://developer.android.com/reference/android/app/ActivityManager.html?hl=zh-cn#getRunningAppProcesses())完全的替代，因为它无法给出集成的[pkgList](https://developer.android.com/reference/android/app/ActivityManager.RunningAppProcessInfo.html#pkgList)，[lru](https://developer.android.com/reference/android/app/ActivityManager.RunningAppProcessInfo.html?hl=zh-cn#lru)和[importance](https://developer.android.com/reference/android/app/ActivityManager.RunningAppProcessInfo.html?hl=zh-cn#importance)信息；

- 此库在7.0开发者预览版本上是无法起作用的。

  <font color="red">下面的一篇文章就来分析下此方案是如何解决读取进程的——>《Android获取运行进程解决方案分析》</font>