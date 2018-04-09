---
title: Android资源获取方式
tags: [随笔, 资源]
categories: [Android]
date: 2017-08-04 16:41:51
---

Android v4包已经更新过多个版本，在不同版本也新增加诸多新API，方便了开发人员的使用，这里要说的是针对资源获取的新API。
新API给予了开发人员很大的方便，无须再像以前一样判断版本号，使用不同的方式去调用。

<!-- more -->  

> **ContextCompat**

完整限定名：android.support.v4.content.ContextCompat
此类是Context的帮助类，在API 22（即5.0【LOLLIPOP_MR1】）中添加，其中提供诸多静态方法以方便调用。

此间我关注的是资源的获取方法：

- static final int getColor (Context context, int id)
  此方法在6.0（M）时添加，获取对应id的颜色资源。
  此方法返回的是带有系统主题风格的颜色。


- static final Drawable getDrawable (Context context, int id)
   此方法在5.0添加，获取id对应的Drawable可绘制对象。

> **ResourcesCompat**

完整的限定名：	android.support.v4.content.res.ResourcesCompat
此类是Resources的帮助类。
此类中获取资源的所返回的资源可以是与系统主题风格无关的资源。

- static int getColor (Resources res, int id, Resources.Theme theme)
同样获取颜色值，但其theme参数可以传入null，即不与当前Context主题相关。

- static Drawable getDrawable (Resources res, int id, Resources.Theme theme)
此方法获取可绘制Drawable资源。
