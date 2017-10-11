---
title: Trap2——Fragment中嵌有ViewPager（FragmentPagerAdapter）
tags:
  - Trap
  - ViewPager
  - Fragment
categories:
  - Android
date: 2017-10-11 01:14:49
---
项目需求中，需要实现app首页下部有三个tab，首个tab内部又有多个顶部tab效果。

首先考虑的是外部容器三个页面使用Fragment实现，首个tab内部又有多个tab对应的页面，即fragment。实现考虑使用ViewPager+TabLayout方式实现。

#### <font color="red">实现中问题：</font>
> app每次打开，ViewPager首次加载显示正常，在切换之后再返回ViewPager，会发现tab页面消失。

原因：ViewPager内放置Fragment使用FragmentPagerAdapter时，传入的FragmentManager对象有问题。
创建FragmentPagerAdapter时，代码如下：

    new HomeFragmentAdapter(mActivity.getSupportedFragmentManager(), fragments);

 需要注意，此刻创建FragmentPagerAdapter本身是在一个Fragment中创建的，即在Fragment使用FragmentPagerAdapter，所以造成了上述情况。

解决方法：
创建FragmentPagerAdapter时传入ChildFragmentManager对象，即使用容器Fragment来获取ChildFragmentManager对象。

    new HomeFragmentAdapter(Fragment.getChildFragmentManager(), fragments);

这样可以解决这个问题。

> 在打开app后，home返回桌面，使用360等工具查杀后再打开，Fragment状态错乱

在app被查杀时，Fragment等组件保存的页面状态只是部分信息，不会将页面内所有数据类似快照一样保存完整，因此再次打开app时，页面内部数据恢复，其他部分无法恢复，造成显示错乱。

目前解决办法：
在每次打开app时，将Fragment原先保存在保存在状态清除，恢复最初状态。
在super.onCreate(savedInstanceState );之前调用。

    // 再利用三方软件回收，查杀进程后，删除activity中fragment包含的状态
        if (savedInstanceState != null) {
            String FRAGMENTS_TAG = "android:support:fragments";
            savedInstanceState.remove(FRAGMENTS_TAG);
        }

这样可以解决在突然被查杀后，再次打开时显示错乱问题。