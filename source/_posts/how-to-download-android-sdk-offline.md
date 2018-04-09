---
title: 离线下载Android SDK（100%成功）
tags:
  - android
  - sdk
categories:
  - Android
date: 2017-10-29 22:32:13
---

最近想在自己的电脑上搭建android开发环境了，由于电脑上很干净（没有任何开发环境），也就是从零开始，虽然知道sdk下载地址被墙了（那个墙很坑爹，好的资源都被墙）。

<!-- more -->

<font color='red' size=4>这里说的方法并非打开sdk-manager.exe的方式，因为这种方式只在android-sdk_r24.4.1-windows.zip之后就不再更新相同的方式了，算是里程碑了。再后边的版本只有一个tools文件夹了，换了文件格式，在windows下没有sdk-manager.exe了。</font>
<font color='blue' size=4>也不是打开Android Studio的settings里边的Android SDK项就可以更新了，因为更新地址被墙了。。。。。。。。即使打开了代理也未必成功！！！</font>

下面说的方式有点儿傻瓜式，但肯定能成功，这个方法还不能成功，那就是人品问题了。


开始吧！！！！！

1. 首先下载最新的[Android Studio](https://dl.google.com/dl/android/studio/ide-zips/3.0.0.18/android-studio-ide-171.4408382-windows.zip)，因为官网提供的Android Studio内不包含SDK环境，所以同时需要下载[SKD Tools](https://dl.google.com/android/repository/sdk-tools-windows-3859397.zip)
2. 第二打开Android Studio，进入Settings -> Appearance & Behavior -> System Settings -> Android SDK 选项卡，首先设置SDK路径，即下载的SDK Tools路径，如下：
![设置SDK路径](/images/how-to-download-android-sdk-offline/set_sdk_location.png "SDK路径")
按照顺序设置结束。
3. 在选择SDK Update Sites。
![选择SDK Update Sites选项卡](/images/how-to-download-android-sdk-offline/select_sdk_update_sites.png "sdk update sites")
查看图中红框中的Android Repository。
4. 将地址复制出来，[https://dl.google.com/android/repository/repository2-1.xml](https://dl.google.com/android/repository/repository2-1.xml "sdk下载配置")，改地址在不同版本发布时可能有所不同，在浏览器中打开xml文件（应该是需要翻墙的），如下图：
![android下载配置](/images/how-to-download-android-sdk-offline/android_repository_xml.png)
文件内数据太多，太大，往下拉，可以从打开的文档中可以看到，所有sdk配置，包含了tools，platform-tools等zip文件文件名（早起版本中提供了完整的下载地址），24版本之后就没有提供完整地址了。
5. 将https://dl.google.com/android/repository/ + “需要的zip文件名”拼接在一起，粘贴到浏览器地址栏中，或者打开迅雷的等下载工具可以直接下载。

这种方式很傻瓜式，有些笨，但100%可以成功，且现在的网络速度很快，相信花不了多少时间就可以完成整个SDK的下载。

在网上最多的还是
* 找到国内镜像地址进行配置更新，但这种方法还需要看服务器内数据是否是最新的，很可能更新不及时。
* 通过ping dl.google.com来找到相关的服务器的方式，但这种方式缺陷是可能找到的海外服务器地址，一个都没法使用。

所以现在的这个方式是最靠谱的。程序员也可以自己写段程序处理xml文件，输出所有的下载地址，然后在迅雷中批量下载！！！！OK了
