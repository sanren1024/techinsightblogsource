title: Flutter(四) 搭建布局
tags:
  - Flutter
  - Dart
categories:
  - Flutter
author: 散人
date: 2018-04-11 08:19:00
---
<table><tr><td bgcolor=#DCDCDC>
你将学习:
<p>&nbsp;&nbsp;- Flutter布局机制如何工作</p>
<p>&nbsp;&nbsp;- 如何竖直或横向展示组件</p>
<p>&nbsp;&nbsp;- 如何搭建Flutter布局</p>
</td></tr></table>

<!-- more -->

这篇文章说明Flutter搭建布局。我们将学习搭建布局，做种效果如下截图：
![](/images/flutter/flutter-building-layouts/flutter-building-layout-lakes.jpg)

这篇引导退一步来解释Flutter进行布局的方式，以及展示如何在屏幕上放置一个单独的组件。在学习完如何横向或竖向展示组件之后，我们会再看到些常用的布局组件。

- [搭建布局](#1)
  * [Step 0：创建](#2)
  * [Step 1：图解布局](#3)
  * [Step 2：实现标题行](#4)
  * [Step 3：实现按钮行](#5)
  * [Step 4：实现文本区域](#6)
  * [Step 5：实现图片区域](#7)
  * [Step 6：集成上述组件](#8)
- [Flutter布局方法](#9)
- [布局一个组件](#10)
- [横向及竖向布局多个组件](#11)
  * [组件对齐方式](#12)
  * [调整组件大小](#13)
  * [缩紧组件](#14)
  * [嵌套行与列](#15)
- [常用布局组件](#16)
  * [标准组件](#17)
  * [Material组件](#18)
- [资源](#19)

### <span id="1">搭建布局</span>
若你想理解“big picture”的布局原理，那么需要学习[Flutter布局方法](#9)。

#### <span id="2">Step 0：创建</span>
首页获取代码：

  - 确定已经[设置](https://blog.csdn.net/sz_chrome/article/details/79619980)好环境
  - [创建基本Flutter工程](https://flutter.io/get-started/test-drive/#create-app)

下来在工程中添加图片：

  - 在工程根目录创建images目录
  - 添加 [lake.jpg](https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg) 图片
  - 更新 [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/pubspec.yaml) 文件，添加 assets 标签

---

#### <span id="3">Step 1：图解布局</span>

第一步是将布局分解成基本元素:
 
   - 区分行与列。
   - 布局是否包含一个网格？
   - 是否有层叠元素？
   - UI是否需要tabs？
   - 注意需要对齐，内边据或者边框的区域。

首先，识别更大的元素。在这里，四个元素在同一列中：一个图片，两行和一个文本块。
![](/images/flutter/flutter-building-layouts/flutter-lakes-diagram.png)