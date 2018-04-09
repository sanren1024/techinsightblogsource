---
title: Flutter简述
tags:
  - Flutter
  - Android
categories:
  - Flutter
date: 2018-03-23 08:36:00
---

Flutter是Google移动UI框架，用以创建高质量的native接口，真正跨平台，同时在iOS和Android上运行。Flutter是免费开源的，全球开发者及组织均可以使用。

<!-- more -->
Flutter有又几个特点：

1. 快速开发
   - 毫秒级别的加载速度，快速的渲染启动。

2. 极具表现力，灵活的UI
   - 接近native用户体验的特性。分层结构更是允许完全，从而可以非常快速的渲染且灵活。

3. native性能

   - Flutter组件包含所有主要平台的差异，例如滚动，导航，图标和字体，从而提供了在iOS和Android一样的native性能体验.


> 快速开发

Flutter热加载技术有助于你快速且简单地进行试验，构建UI，增加特性，并且快速修复bug。体验不到一秒的重新加载体验。

![hot-reload](/images/Flutter/flutter-introdution/hot-reload.gif)



> 漂亮的UI

Flutter内置MD设计风格及iOS组件，更有丰富的手势API，流畅的滚动体验和平台认同感会让用户感到愉悦。

![](/images/Flutter/flutter-introdution/screenshot-1.png)![](/images/Flutter/flutter-introdution/screenshot-2.png)![](/images/Flutter/flutter-introdution/E:\Flutter_Study\screenshot-3.png)![](/images/Flutter/flutter-introdution/ios-friendlychat.png)

查看[组件](https://flutter.io/widgets/)

> 现代的响应式框架（Modern，reactive framework）

利用Flutter响应式框架和丰富的平台，布局和功能组件是的UI构建非常简单。使用灵活并且强大的API（2D，动画，手势，性能等）可以解决在UI上各种问题。

```dart
class CounterState extends State<Counter> {
  int counter = 0;

  void increment() {
    // Tells the Flutter framework that state has changed,
    // so the framework can run build() and update the display.
    setState(() {
      counter++;
    });
  }

  Widget build(BuildContext context) {
    // This method is rerun every time setState is called.
    // The Flutter framework has been optimized to make rerunning
    // build methods fast, so that you can just rebuild anything that
    // needs updating rather than having to individually change
    // instances of widgets.
    return new Row(
      children: <Widget>[
        new RaisedButton(
          onPressed: increment,
          child: new Text('Increment'),
        ),
        new Text('Count: $counter'),
      ],
    );
  }
}
```


查看[组件](https://flutter.io/widgets/)及学习更多有关[reactive framework](https://flutter.io/widgets-intro/)

> 访问native特性和SDKs

通过使用平台APIs，第三方SDK和native代码，可以灵活地实现App。Flutter可以让你在iOS和Android上复用Java，Swift和Objective-C代码以及访问native特性和SDKs。

Flutter平台特性访问十分简单。下边是Flutter内[interop example](https://github.com/flutter/flutter/tree/master/examples/platform_channel)]示例：

```dart
Future<Null> getBatteryLevel() async {
  var batteryLevel = 'unknown';
  try {
    int result = await methodChannel.invokeMethod('getBatteryLevel');
    batteryLevel = 'Battery level: $result%';
  } on PlatformException {
    batteryLevel = 'Failed to get battery level.';
  }
  setState(() {
    _batteryLevel = batteryLevel;
  });
}
```

学习如何使用[packages](https://flutter.io/using-packages/)，或者[platform channels](https://flutter.io/platform-channels/)来访问native代码，APIs及SDKs。

> 统一的开发标准

Flutter拥有工具及库帮助你简单快速地在iOS和Android上实现你的想法。若你还没有任何移动开发经验，那么Flutter将会是你构建漂亮的移动APP的一种简单快速的额方式。若你是有经验的iOS或者Android开发人员，那么你可以使用Flutter组件，并且继续使用已有的Java/Objective-C/Swift程序。

- 构建
    - 漂亮的APP UI
       - 丰富的2D GPU加速APIs
       - 响应式框架
       - 动画/动作 APIs
       - 兼容Android Material组件及苹果组件样式
  - 流程的编码体验
      - 急速热加载技术
      - IntelliJ：重构，自动补足功能等
      - Dart语言及核心库
      - 包管理
  - 拥有App所有特性
      - 与移动OS APIs&SDKs互操作性
      - Maven/Java
      - Cocoapods/ObjC/Swift
- 优化
    - 测试
      - Unit测试
      - 继承测试
      - 无设备测试
   - Debug
      - IDE debug
      - 基于网络debug
      - 异步/唤醒感知
      - 表达式求值程序
   - 配置
      - 时间线
      - CPU和内存
      - 应用性能图标
- 部署
   - 编译
      - Native ARM程序
      - 消除无效代码
   - 发布
      - App市场
      - Play Store

可以在[技术概览](https://flutter.io/technical-overview/)了解更多Flutter的特殊性。

[原文地址](https://flutter.io/)

