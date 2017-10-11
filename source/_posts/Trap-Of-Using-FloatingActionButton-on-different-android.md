---
title: Trap1——在不同版本上使用FloatingActionButton
tags:
  - Trap
  - Widget
  - FloatingActionButton
  
categories:
  - Android
date: 2017-08-30 07:39:23
---

由于项目需求，要实现一个比较吸引人的交互设计。
设计类似效果：
![悬浮View设计](/images/Trap-Of-Using-FloatingActionButton-on-different-android/origin_design.png  "理想布局设计")

在实现前就考虑到design包内提供有一个FloatingActionButton组件可以来实现这个效果。
FAB组件继承自ImageView，可以使用ImageView的所有属性。
使用组件遇到问题：

> 组件内icon太小问题

除了layout_width， layout_height属性外，FAB又另外提供app:fabSize属性来设置FAB大小，且只支持此属性来改变FAB大小，其有"auto" "mini" "normal"三个属性值。

* auto 基于窗口大小来确定FAB大小
* mini FAB最小大小
* normal FAB正常大小

尝试设置layout_width, layout_height来自定义FAB大小，同时设置fabSize属性，最终效果内，icon会被撑大或者看不到边缘，因此自定义width，height大小值来改变FAB大小是行不通的。

1. 目前的方法是通过仅设置android:scaleType=“center”来避免系统在渲染时对icon进行大小改变操作（centerInside，centerCrop等属性存在icon大小改变情况）。
2. 重写（override）normal，mini对应的dimen值，即fab_size_normal和fab_size_mini的值。

方法2暂未测试，却是可行的——dimens.xml

    <resources>
        <dimen name="fab_size_normal">100dp</dimen>
    </resources>

在引用是会引用到重写后的值。

#### PS：FAB组件需要配合CoordinatorLayout使用