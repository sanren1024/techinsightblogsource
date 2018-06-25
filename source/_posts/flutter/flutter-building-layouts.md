title: Flutter(四) 搭建布局
tags:
  - Flutter
  - Dart
 
categories:
  - Flutter

author: 散人

date: 2018-04-11 08:19:00

---

> 你将学习:
> - Flutter布局机制如何工作
> - 如何竖直或横向展示组件
> - 如何搭建Flutter布局

这篇文章说明Flutter搭建布局。
<!-- more -->
我们将学习搭建布局，做种效果如下截图：
![](https://flutter.io/tutorials/layout/images/lakes.jpg)

这篇引导退一步来解释Flutter进行布局的方式，以及展示如何在屏幕上放置一个单独的组件。在学习完如何横向或竖向展示组件之后，我们会再看到些常用的布局组件。

- [搭建布局](#1)
  * [Step 0：创建](#2)
  * [Step 1：图解布局](#3)
  * [Step 2：实现标题行](#4)
  * [Step 3：实现按钮行](#5)
  * [Step 4：实现文本区域](#6)
  * [Step 5：实现图片区域](#7)
  * [Step 6：整合](#8)
- [Flutter布局原理](#9)
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
![](https://flutter.io/tutorials/layout/images/lakes-diagram.png)

接下来，图解每行。第一行，我们称其Title Section，有3个子组件：一列文本区域，一个星型图标，及一个数字。第一列子组件包含2行文本。且第一列占有较大空间，因此需要将两行文本放在Expanded组件中。
![](https://flutter.io/tutorials/layout/images/title-section-diagram.png)

第二行，我们称其Button section，同样有3个子组件：由三列组成，且每列均由一个图标和文本组成。
![](https://flutter.io/tutorials/layout/images/button-section-diagram.png)

在图解了布局之后，再从细节到整体来实现这个布局就容易了。为了让嵌套的代码看起来不那么混乱，我们将一些实现置于变量和函数中。

#### <span id="4">Step 2：实现Title Section</span>

首先需要在Title Section左侧创建一列。在Expanded组件中的Column组件使得当前列（column并非组件）可以覆盖真个Title section. 将Column组件的 *crossAxisAlignment* 属性设置为*CrossAxisAlignment.start* ，这样Column组件位于当前行的起始位置。

将第一行的文本组件放置于Container组件中以便添加Container内边据。第二个文本组件文字是灰色。

最后的2个组件包括一个红色星型图标和一个数字“41”的文本。将整个标题行（Title Section图解中的Row with 3 children）放置在一个Container组件中，并且设置Container组件32px的内边距。

<table><tr><td bgcolor=#87CEFA>
Note: 如何代码实现有问题，可以依据Github上的<a href="https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/main.dart">lib/main.dart</a> 来检查你的代码。
</td></tr></table>

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    Widget titleSection = new Container(
      padding: const EdgeInsets.all(32.0),
      child: new Row(
        children: [
          new Expanded(
            child: new Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                new Container(
                  padding: const EdgeInsets.only(bottom: 8.0),
                  child: new Text(
                    'Oeschinen Lake Campground',
                    style: new TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
                new Text(
                  'Kandersteg, Switzerland',
                  style: new TextStyle(
                    color: Colors.grey[500],
                  ),
                ),
              ],
            ),
          ),
          new Icon(
            Icons.star,
            color: Colors.red[500],
          ),
          new Text('41'),
        ],
      ),
    );
  //...
}
```

<table><tr><td bgcolor=#98FB98>
Tip: 粘贴代码到工程中时，代码缩进可能错乱。如果是在IntelliJ中，可以有单机选择Reformat with Dart Style。或者在命令行中使用<a href="https://github.com/dart-lang/dart_style">dartfmt</a>命令。
</td></tr></table>
<br/>
<table><tr><td bgcolor=#98FB98>
Tip: 为体验更快开发过程，尝试使用Flutter的热加载功能。热加载使得在修改代码同时快速地在查看到修改后的效果，而不用重运行app。
</td></tr></table>

#### <span id="5">Step 3：实现按钮行（Button Section）</span>

Button Section包含3列相同的布局——一个图标和一个文本。这行中3列均匀分布，并且文本和图标颜色是APP build()方法中设置的primary color。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // title section implementation

    return new MaterialApp(
      title: 'Flutter Demo',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),

    //...
}
```

由于创建每列的代码是相同的，最高效的办法就是创建一个嵌套函数，例如就定义为buildButtonColumn()，这个方法中创建包含一个图标和一个文本得组件，并且返回Column对象。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //...

    Column buildButtonColumn(IconData icon, String label) {
      Color color = Theme.of(context).primaryColor;

      return new Column(
        mainAxisSize: MainAxisSize.min,
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          new Icon(icon, color: color),
          new Container(
            margin: const EdgeInsets.only(top: 8.0),
            child: new Text(
              label,
              style: new TextStyle(
                fontSize: 12.0,
                fontWeight: FontWeight.w400,
                color: color,
              ),
            ),
          ),
        ],
      );
    }
  //...
}
```

这个创建方法中直接添加icon组件。将文本组件放于Container组件中来添加上边距，将icon与text分离开。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //...

    Widget buttonSection = new Container(
      child: new Row(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          buildButtonColumn(Icons.call, 'CALL'),
          buildButtonColumn(Icons.near_me, 'ROUTE'),
          buildButtonColumn(Icons.share, 'SHARE'),
        ],
      ),
    );
  //...
}
```

#### <span id="6">Step 4：实现多行文本（Text Section）</span>

由于文本太长，其实现我们赋值于一个变量。将文本放在Container中，四周边距设置32px。设置softwrap属性，这个属性表示当每行文本遇到句号或者逗号时是否需要换行。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //...

    Widget textSection = new Container(
      padding: const EdgeInsets.all(32.0),
      child: new Text(
        '''
Lake Oeschinen lies at the foot of the Blüemlisalp in the Bernese Alps. Situated 1,578 meters above sea level, it is one of the larger Alpine Lakes. A gondola ride from Kandersteg, followed by a half-hour walk through pastures and pine forest, leads you to the lake, which warms to 20 degrees Celsius in the summer. Activities enjoyed here include rowing, and riding the summer toboggan run.
        ''',
        softWrap: true,
      ),
    );
  //...
}
```

#### <span id="7">Step 5：实现头部图片（Image Section）</span>

目前只剩头部图片部分还未实现。这张图片基于Creative Commons协议在网上是可以[获取](https://images.unsplash.com/photo-1471115853179-bb1d604434e0?dpr=1&auto=format&fit=crop&w=767&h=583&q=80&cs=tinysrgb&crop=)到的（当然学习过程，可以自己比较随意的拿一张图片进行）。由于图片较大且网络加载慢，所以在[Step 0](#2)步骤中已经inlude进来并且修改了pubspec.yml文件，可以直接在本地进行访问。

```dart
body: new ListView(
  children: [
    new Image.asset(
      'images/lake.jpg',
      height: 240.0,
      fit: BoxFit.cover,
    ),
    // ...
  ],
)
```
BoxFit.cover 告诉Framework图片需要尽可能的小但是需要充满显示部分。

#### <span id="8">Step 6：整合</span>

最后一步，将删除个步骤中定义的组件最终整合在一起。所有组件放置于ListView中。

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    Widget titleSection = new Container(
      padding: const EdgeInsets.all(32.0),
      child: new Row(
        children: <Widget>[
          new Expanded(
              child: new Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              new Container(
                padding: new EdgeInsets.only(bottom: 8.0),
                child: new Text(
                  'Oeschinec lake Compground',
                  style: new TextStyle(fontWeight: FontWeight.bold),
                ),
              ),
              new Text(
                'Kandersteg, Switzerland',
                style: new TextStyle(color: Colors.grey[500]),
              ),
            ],
          )),
          new Icon(
            Icons.star,
            color: Colors.red[500],
          ),
          new Text('41')
        ],
      ),
    );

    // button section
    Column buildButtonColumn(IconData icon, String label) {
      Color color = Theme.of(context).primaryColor;

      return new Column(
        mainAxisAlignment: MainAxisAlignment.center,
        mainAxisSize: MainAxisSize.min,
        children: <Widget>[
          new Icon(icon, color: color),
          new Container(
            margin: const EdgeInsets.only(top: 8.0),
            child: new Text(
              label,
              style: new TextStyle(
                color: color,
                fontSize: 12.0,
                fontWeight: FontWeight.w400,
              ),
            ),
          ),
        ],
      );
    }

    Widget buttonSection = new Container(
      child: new Row(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: <Widget>[
          buildButtonColumn(Icons.call, 'CALL'),
          buildButtonColumn(Icons.near_me, 'ROUTE'),
          buildButtonColumn(Icons.share, 'SHARE'),
        ],
      ),
    );

    Widget textSection = new Container(
      padding: const EdgeInsets.all(32.0),
      child: new Text(
        '''
        Lake Oeschinen lies at the foot of the Blüemlisalp in the Bernese Alps. Situated 1,578 meters above sea level, it is one of the larger Alpine Lakes. A gondola ride from Kandersteg, followed by a half-hour walk through pastures and pine forest, leads you to the lake, which warms to 20 degrees Celsius in the summer. Activities enjoyed here include rowing, and riding the summer toboggan run.
        ''',
        softWrap: true,
      ),
    );

    return new MaterialApp(
      title: 'Flutter Demo',
      theme: new ThemeData(primarySwatch: Colors.blue),
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Top Lakes'),
        ),
        body: new ListView(
          children: <Widget>[
            new Image.asset(
              'images/lake.jpg',
              height: 240.0,
              fit: BoxFit.cover,
            ),
            titleSection,
            buttonSection,
            textSection,
          ],
        ),
      ),
    );
  }
}
```

### <span id="9">Flutter布局原理</span>

**重点**
- Flutter中组件（Widget）都是类（class），即有这些类来组成UI
- 布局（layout）和UI均称为组件
- 复杂UI由简单的组件组成

Flutter中核心的概念就是组建（Widget）——包括layout和widget。例如：图片，图标，文本等这些可见的元素是组件，同样不可见的元素如行（Row），列（Column），网格（Grid）也是组件。

通过组织这些简单组件来完成复杂的布局。如下图中三列，没列均有一个图标和一个文本组成。
![](https://flutter.io/tutorials/layout/images/lakes-icons.png)  ![](https://flutter.io/tutorials/layout/images/lakes-icons-visual.png)

第二张图显示了可见组件。

看看图解：
![](https://flutter.io/tutorials/layout/images/sample-flutter-layout.png)

图中多数是与预期的一样，那么粉红色的Container组件是干嘛的？Container组件可以用来定制组件样式，添加内边距，外边距，边框和背景色等。

这个示例中所有的文本组件均放置于Container组件中。而整个行（Row）也放置于一个Container组件中并添加了边距。

### <span id="10">布局一个组件</span>
**重点**
- app也是组件
- 创建组件并添加到布局
- 要在设备上展示组件，必须将组件添加到app组件
- Material组件库的[Scaffold](https://docs.flutter.io/flutter/material/Scaffold-class.html)使用，它提供了默认的banner，背景色，并且提供了添加drawers，snack bars和底部sheets的APIs
- 如果愿意，可以只使用标准组件库中的组件完成app

这部分内容就说说如何创建一个组件并且显示。
只需要几步就可以在屏幕上显示一个文本，图标或者图片。

1. 选择一个布局来存储这个对象。
    基于布局样式，如边距，对齐方式等限制条件，从[layout widgets](https://flutter.io/widgets/)中选择一个布局组件。
2. 创建一个组件来持有这个对象。

看看创建实例代码
   创建文本
```dart
new Text('Hello World', style: new TextStyle(ftontSize: 32.0))
```

   创建图片
```dart
new Image.asset('images/myimage.jpg', fit: BoxFit.cover)
```

   创建图标
```dart
new Icon(Icons.star, color: Colors.red[500])
```
3. 在布局组件中添加这个可见组件
    布局组件分两种：单个子组件的布局组件有一个*child*属性（如Center，Container）；多个子组件的布局组件拥有*children*属性（如Row，Column等）。
   
   下边这段代码在Center中添加Text
 ```dart
 new Center(child: new Text('Hello World', style: new TextStyle(ftontSize: 32.0)))
 ```

4. 在页面上展示组件
    Flutter app本身就是一个组件，且多数组件有一个[build()](https://docs.flutter.io/flutter/widgets/StatelessWidget/build.html)方法。在build方法中声明组件即是在设备上显示。
对 Material风格的app来说，在*body*属性值上直接设置Center组件值即可。
```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(widget.title),
      ),
      body: new Center(
        child: new Text('Hello World', style: new TextStyle(fontSize: 32.0)),
      ),
    );
  }
}
```
