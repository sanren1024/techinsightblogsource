---
title: vbox下ubuntu共享文件夹流程整理

tags:
  - ubuntu
  - linux
  - virtualbox

categories:
  - linux

author: 散人

date: 2018-06-29 00:11:17

---
  
在windows下，安装了virtualbox，想使用linux来进行开发任务。

背景：一方面一些工具在windows下支持不是很好，或者会出现各种奇怪的bug。比如：最近使用windows版本的msysGit工具，安装之后在win7，win10会遇到打开命令行之后卡盾无比，敲击一行命令回车执行也特别慢。

另外还有编码问题，windows编码也是比较奇葩的，服务器基本的传输保存统一的是UTF-8，但若在windows下进行开发时候很多时候不是这个编码，导致上传后在服务端查看就是中文乱码。
<!-- more -->
等等问题。

#### 共享文件夹

1. 首先安装vbox，下载linux（ubuntu 17.10）镜像，然后安装就行，不用多说了。

2. 启动后就需要先打通vbox下ubuntu与host的windows的通道，是的两者之间数据共享，可以互相赋值粘贴。

> 启动ubuntu虚拟机之前，先要在 “设置 > 存储”项，可以看到如下图UI。

![设置共享文件夹](/images/linux-vbox-shared-folder/ubuntu_vdi_settings.PNG)  

在这里增加虚拟机增加功能包iso文件，默认位置可以看到红线标出。

\> 启动虚拟机，打开菜单“设备”项，如果上面一步忘了指定，可以在这里的分配光驱里边进行指定。

![](vbox下ubuntu共享文件夹流程整理_files/ubuntu_additions.png)

在“设备”菜单项下，选择点击“安装增强功能”，等待安装增强包完成。

\> 进入目录“/media/用户名/”，可以看到增强包目录“VBox\_GAs\_5.2.12”，进入该目录，执行命令

“sudo ./VBoxLinuxAdditions.run”，等待安装完成。

\> 完成上面一步之后，再进入到菜单项“设备”选择“共享文件夹”项，去添加共享文件夹。  
![添加共享文件夹](/images/linux-vbox-shared-folder/ubuntu_share_folder.PNG)

指定你在windows下作为共享文件夹的目录，并勾选“自动挂载”“固定分配”两项。设置完成后重启虚拟机。

\> 启动虚拟机之后，可以看到在ubuntu系统上挂载成功了。
![挂载成功](/images/linux-vbox-shared-folder/ubuntu_share_folder_mounted.PNG)

\> 去尝试打开挂载上的文件夹，但是这时问题将会是，没有权限展示文件夹中的文件，缺失权限。

这样还差一步，就是将用户添加到vboxsf组，执行命令“sudo adduser 用户名 vboxsf”。再次重启虚拟机，去打开挂载目录，可以看到其中的内容了。
![查看共享文件夹内容](/images/linux-vbox-shared-folder/ubuntu_share_folder_listed.PNG)

这样就可以进行vbox和windows的数据共享了，包括可以互相粘贴（当然需要在设置项中进行提前设置）。
