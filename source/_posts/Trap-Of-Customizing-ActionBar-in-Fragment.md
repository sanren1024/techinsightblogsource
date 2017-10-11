---
title: Trap3——Fragment中定制ActionBar（同一个Activity内多个Fragment）
tags:
  - Trap
  - ActionBar
  - Fragment
categories:
  - Android
date: 2017-10-11 02:38:26
---
在最新的design提供了ToolBar来更加自由地定制，使用ActionBar，在布局文件中可以更加灵活地使用。

项目中实际遇上，首页三个tab页面内，ActionBar样式，功能均不同。
ViewPager内不同的Fragment，需求的样式不同，菜单功能不同。

但处于同一个Activity内，一般情况下，一个Activity内只布局一个ActionBar（ToolBar）即可，但出于灵活布局，减少Fragment，Activity之间依赖，考虑在每个Fragment内均布局一个ActionBar（ToolBar），在Fragment内通过与原有ActionBar相同的调用来设置Activity对ActionBar的支持及相关属性设置。

    @BindView(R.id.my_toolbar)
    Toolbar mActionBar;
    
     ((AppCompatActivity) getActivity()).setSupportActionBar(mActionBar);
        ActionBar actionBar = ((AppCompatActivity) getActivity()).getSupportActionBar();
        if (actionBar == null) {
            return;
        }
        actionBar.setDisplayShowTitleEnabled(false);
        actionBar.setDisplayShowCustomEnabled(true);
        actionBar.setDisplayShowHomeEnabled(false);
        actionBar.setDisplayUseLogoEnabled(false);
        actionBar.setDisplayHomeAsUpEnabled(false);

这样就实现了在Fragment中实现Activity对ActionBar的显示及相关定制。

但此处为解决的问题是，在三个Fragment中均设置了ActionBar，在对应Fragment显示的时候，除了首个ActionBar正确，其他Fragment的ActionBar均不正确。

尚待解决。。。。。