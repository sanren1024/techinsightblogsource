---
title: 
tags: []
categories: []
date: 2017-05-28 07:41:00
---

#### Android多进程app中Application回调方法onCreate()被执行多次分析及解决

>  问题描述

最近工作中碰到一个问题，在优化app，使用DDMS查看Application log过程中看到，app启动了三个进程，一个主进程，两个附带的进程。
![启动三个进程](/images/android-multiprocess-oncreate-executed-serval-times/processes_app_starts.png "启动进程")
自定义Application回调方法onCreate()被执行了3次。开始不知是何原因。

##### 相关知识

> android:process

从Android开发者文档中的manifest中进程配置***android:process***可以获知:
正常情况下，应用程序的所有组件运行在一个默认的进程名下，因此不需要使用这个属性。但在需要的情况下，可以通过使用这个属性来覆盖默认进程，这样一个app就跨越多个进程。

如果这个属性值以冒号（“:”）开始，说明新进程相对于应用程序是一个私有进程，且组件运行在此进程中。若属性值以小写字符开始，那么新进程即是一个全局进程，组件运行在这个全局进程中。这也意味着其他应用程序组件可以与此进程进行通信，减少资源使用。

标签*&lt;applicatoin&gt;*的*android:process*属性可以为整个appn内组件设置一个默认的运行进程。

> Application

当在manifest中的*&lt;activity&gt;*或者*&lt;service&gt;*标签中配置了*android:process*，在创建新进程，每个进程对应了一个Dalvik虚拟机，因此新的Application实例会被创建。

因此在我的app中启动了两个额外的service服务集成，因此Application被执行3次。

---
#### 解决方案

> 获取进程名

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

***<font color="red">虽然这种方法可以奏效，在但Android 5.0后的版本，此方法不是每次都起到效果</font>***

> 具体方案还需要测试。。。。节后更新。端午节快乐！
> 后续继续此方案解决测试