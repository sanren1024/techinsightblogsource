---
title: Ubuntu常用软件安装流程整理

tags:
  - ubuntu
  - linux

categories:
  - linux

author: 散人

date: 2018-06-29 00:18:14

---

每次安装ubuntu之后，需要查找安装常用软件，现在整理一下常用软件及其安装。我的ubuntu版本是ubuntu mate 17.10，整理的软件也是安装成功的。

> [google拼音输入法](#1)
> [美化终端，安装zsh](#2)
> [安装shadowsocks](#3)
> [安装Chrome浏览器](#4)

----------

### <span id='1'>google拼音输入法</span>

google拼音输入法需要在fcitx框架下安装 。
<!-- more -->
1\. 打开终端，执行如下命令。
```Shell
sudo apt-get install fcitx fcitx-googlepinyin  
```

安装fcitx框架及google拼音。

2\. 打开系统设置菜单“Language Support”。
![Language Support](/images/linux-ubuntu-common-softwares/ubuntu_add_input_language_menu.PNG)  

打开“Language Support”设置窗口。
![Language Support设置](/images/linux-ubuntu-common-softwares/ubuntu_install_simple_chinese.PNG)  

其中“Keyboard input method system”项设置为上一步安装的“fcitx”项。

3\. 安装语言支持。

点击“Install/Remove Languages...”，在弹出的语言选择框中找到“Chinese(Simplified)”项，确认后，开始进行安装。

![安装语言支持](/images/linux-ubuntu-common-softwares/ubuntu_support_languages.PNG)

安装完成之后在则会在“Language Support”对话框中显示已经安装的“汉语(中国)”项。如果没有找到此项，则可以先进行重启操作。

4\. 配置google拼音。

在配置支持google拼音输入法之前，先进行Layouts添加。

选择菜单 “System > Preferences > Hardware > Keyboard”打开“Keyboard Preferences” 打开设置对话框。

![添加中文支持](/images/linux-ubuntu-common-softwares/ubuntu_add_chinese_input_method_layout.PNG)  

选择"Layouts"选项卡，点击“Add”按钮，打开添加“Chinese”布局。

最后添加Google拼音输入法，在菜单中找到并打开“Fictx Configuration”，“Add input method”对话框。

![添加输入法](/images/linux-ubuntu-common-softwares/ubuntu_input_method_configuration.PNG)

点击“+”号，打开“Add input method”对话框。

![添加google输入法](/images/linux-ubuntu-common-softwares/ubuntu_add_google_pinyin.PNG)  

找到开始时安装的“Google Pinyin”，点击OK后，添加完成。

<font color='red'>PS: 添加成功后可能会遇到一个问题，候选词区是黑色，文字看不到，可能是因为多安装了 fcitx-module-kimpanel，因此删除这个软件。</font>

```Shell
sudo apt remove fcitx-module-kimpanel
```

之后再输入的时候可以看到候选词了。

----------

### <span id='2'>美化终端，安装zsh，一款强大的Shell工具</span>

安装

```Shell
sudo apt-get install zsh
```

配置成为默认shell工具

```Shell
chsh -s /bin/zsh
```

```Shell
chsh -s /bin/bash
```

**安装oh-my-zsh**

由于zsh太强大并且复杂，因此安装on-my-zsh扩展工具。这里提前需要安装git工具，负责无法完成安装进度。

安装

*curl方式*

```Shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"  
```

*wget方式*

```Shell
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"  
```

其本质就是下载github上提供的install.sh脚本。

当然如果没有curl，wget就需要先安装这两个工具。  

----------

### <span id='3'>安装shadowsocks客户端</span>

直接执行一下三条命令即可  

```Shell
sudo add-apt-repository ppa:hzwhuang/ss-qt5  
sudo apt-get update  
sudo apt-get install shadowsocks-qt5
```

安装完成之后就可以配置了。

----------

### <span id='4'>安装Chrome浏览器</span>

执行如下命令  

```Shell
sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
```

```Shell
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
```

```Shell
sudo apt-get update
```

```Shell
sudo apt-get install google-chrome-stable
```

这样就安装成功了。

----------
