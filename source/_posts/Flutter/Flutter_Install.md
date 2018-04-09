---
title: Flutter(一) 环境准备
tags: [Android, Flutter, Dart]
categories: [Android, Flutter]
date: 2018-03-15 00:32:51
---
> 安装Flutter
    
在国内安装使用Flutter，首先阅读下这篇[文章](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)，由于国内网络大环境问题，需要提前进行一点配置。

在国内安装Flutter需要首先需要一个值得信任的国内镜像。在镜像上边保存着Flutter需要的依赖及相关库，包等。为了使用Flutter，需要使用一个备用存储位置，我们需要配置环境变量。
配置环境变量名：PUB\_HOSTED\_URL和FLUTTER\_STORAGE\_BASE_URL。

<!-- more -->  

在windows系统中，需要在环境变量设置中添加：
PUB\_HOSTED\_URL ： https://pub.flutter-io.cn
FLUTTER\_STORAGE\_BASE_URL ： https://storage.flutter-io.cn

然后运行Git命令（前提是安装了GitBash工具）：
git clone -b dev https://github.com/flutter/flutter.git Flutter
#### <font color=red>Flutter文件夹需要注意：文件夹存放的路径上不要出现空格，否则在IDE中进行工程创建后会有警告，SDK环境路径上存在分隔符。</font>

在clone完成之后，即flutter sdk下载完毕，还需要配置Flutter环境： xxxx/Flutter/bin目录下。

重新打开一个命令行，在其中输入命令

    flutter doctor

进行环境及缺失的依赖检查，并下载需要的依赖。
运行效果如下图：
![运行flutter doctor](/images/Flutter/flutter-install/run_flutter_doctor.png)

在环境及相关依赖检查完成之后，可以开始在Android  Studio中进行创建工程行为。

#### <font color=red>注意：Android Studio 预览版中无法保证运行Flutter成功。因此需要使用稳定版AS，且需要3.0版本以上。</font>
Android Studio中需要安装Flutter Plugin，Dart Plugin两个插件。

Dart SDK也需要手动安装，直接下载zip包免安装。

成功准备好IDE环境之后，就可以创建Flutter Project了，默认创建Flutter Application就可以了，按照IDE创建提示一直到最终完成。

** 需要注意：同样由于网络环境，直接运行Flutter Project是不可行的，UI会一直停留在Gradle正在初始化工程。这时需要修改build.gradle配置中的中央Maven库到一个可信赖的公共Maven库。 这里我修改成Ali的Maven库 **

    buildscript {
        ext.kotlin_version = '1.1.51'
        repositories {
            maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
            google()
        }
        // ......
    }
    
    // ......
    
    allprojects {
        repositories {
            maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        }
        google()
    }
    // ......


然后再次sync工程，进行运行。

首个创建的Flutter Project工程结构如下：
![flutter project结构](/images/Flutter/flutter-install/android_studio_flutter_project.png)

再来看看运行效果：
![flutter工程运行效果](/images/Flutter/flutter-install/flutter_app_runtime.png)

至此，Flutter，Dart环境均准备结束了。

