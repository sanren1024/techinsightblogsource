---
title: Flutter(二)  创建第一个Flutter APP
tags: 
 - Flutter
 - Dart

categories:
 - Flutter

date: 2018-03-29 11:36:22

---

这一章主要是创建一个Flutter App。如果你熟悉面向对象编程，有基本的编程概念（变量，循环，条件判断等），那么你不必要具备原有的Dart和移动开发经验，就可以轻松地理解完成这章内容。

<!-- more -->

- [Step 1：创建第一个flutter app](#1)
- [Step 2：使用外部包](#2)
- [Step 3：添加一个有状态的组件（Stateful widget）](#3)
- [Step 4：创建无限滚动ListView](#4)
- [Step 5：添加交互](#5)
- [Step 6：跳转到另一个屏](#6)
- [Step 7：利用主题改变UI](#7)

> 构造什么

你将为一家初创公司实现一个简单的移动app，主要功能是为这家公司推荐名字。用户可以选择或者取消名字，保存最好的名字。程序会一次性产生10个名字。在用户滚动时，新名字同时会产生出来。用户可以点击导航栏（appbar）的列表图标进入到一个新的列表页查看喜欢的名字。


最终的结果最后运行结果中可以看到。

> 你将会学到：
>  - Flutter app基本结构
>  - 查找并使用包来扩展功能
>  - 使用热加载加快开发周期
>  - 如何实现一个有状态组件（Stateful widget）
>  - 如何创建一个无限，懒加载列表
>  - 如何创建并路由到第二个屏幕
>  - 如何使用app主题修改外观

> 你将会使用：
> 需要安装：
> - Flutter SDK
>   Flutter SDK包含了Flutter引擎，framework，组件，工具，和Dart SDK。这份代码实验需要v0.1.4或者更高版本。
>   
> - Android Studio
>   这次代码实验需要Android Studio。也可以在命令行。
>
> - 需要IDE插件
>   IDE上必须分别安装Flutter和Dart插件。
>
>  查看[Flutter安装](https://flutter.io/get-started/install/)学习如何建立起你的Flutter环境
>  


> <span id="1">Step 1：创建Flutter App</span>

创建第一个简单的，IDE提供模板的Flutter App，可以[按照引导创建Flutter工程](https://flutter.io/get-started/test-drive/#create-app)。我将工程名命名为flutter_app，这个按照个人习惯吧，只要名称合法就行。

在这节程序实验中，最多编辑的会是lib/main.dart，其中就是Dart代码。

<table><tr><td bgcolor=LightGreen>Tips：当你粘贴代码到IDE中的时候，可能发生缩进不对齐的情况。你可以使用Flutter工具来解决这种问题：<br/>
 1.  Android Studio/IntelliJ IDEA：右单击Dart代码，选择<font color=Green size=4>Refactor code with dartfmt</font><br/>   2. VS Code：右单击选择 <font color=Green size=4>Format Document</font><br/>    3. Terminal：运行命令 flutter format  filename。
</td></tr></table>

1.  替换模板的lib/main.dart
    移除原有工程中的模板代码lib/main.dart。输入如下代码，可以查看到UI中间显示的“Hello World”。

```Dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
          title : 'Welcome to Flutter',
          home:new Scaffold(
    appBar:new AppBar(
      title:new Text('Welcome to Flutter'),
    ),
    body:new Center(
      child:new Text('Hello World'),
    ),
      ),
    );
  }
}
```
   
2. 运行app，可以看到如下执行效果

![Hello World](/images/flutter/flutter-create-your-first-app/flutter_hello_world.png)

**结果**

- 这个例子创建了一个Material app。Material是在移动端和web端上的设计标准。Flutter提供了丰富的Material组件。
- main()方法声明了胖箭头(=>)符号表示法，这种写法表示了man()方法是一个单行函数，即函数体只有一行代码构成。
- App继承StatelessWidget，这也使得App本身成为了一个组件。在Flutter中，几乎所有对象都被认为是一个组件，包含对齐方式，内边距和布局。
- Material组件库中的Scaffold提供了App默认需要的appbar，title，和body属性，body属性包含了home screen的组件树结构。组件的子组件结构可以相当复杂。
- 组件的主要工作就是提供build()方法，描述如何展示自己及其他组件。
- 这个例子中组件结构有一个Center组件中包含一个Text子组件组成。Center组件将其内的组件结构置于屏幕中央。

----

> <span id="2">Step 2：使用外部包</span>

这一步中，你将使用开源包english_words。这个包中包含了几千个最常用的英文单词和一些实用方法。

你可以在[pub.dartlang.org](https://pub.dartlang.org/flutter/)找到[english_words](https://pub.dartlang.org/packages/english_words)包，同时还有其他的开源包。

1.  文件pubspec.yaml管理者Flutter App的资源。在pubspec.yaml中，添加english_words（3.1.0或者更高）到依赖列表。如下代码中：

```dart
...
dependencies: 
    flutter:sdk:flutter  
  
    # The following adds the Cupertino Icons font to your application.  
    # Use with the CupertinoIcons class for iOS style icons. 
    cupertino_icons:^0.1.0  
     
    english_words:^3.1.0
  ...
```
原有模板文件中代码太多（包含注释），不全部展示出来。这里在depenencies下添加english_words包依赖。

2. 在Android Studio编辑器中查看pubspec.yaml文件时，可以看到编辑器右上方有命令操作栏，
![AS查看pubspec文件的命令操作栏](/images/flutter/flutter-create-your-first-app/flutter_external_page_pubspec_command.png)
点击**Package get**。这就是获取english_words包操作。你会在控制台命令行看到：
![获取english_words包控制台输出](/images/flutter/flutter-create-your-first-app/flutter_third_lib_config_package_get_output.png)

3. 在lib/main.dart中，添加对english_words包的导入，如下显示的导入语句：

```dart
import 'package:flutter/material.dart';  
import 'package:english_words/english_words.dart';
```

在你输入后，AS会因为你写的导入语句给出建议。表现在你输入的导入语句会变成灰色，这就是提示你导入的库还没有使用过。

4. 使用english_words包来生成文本，而不再显示“Hello World”
<table><tr><td bgcolor=LightGreen>Tips：“Pascal case”(大驼峰规则)意思是在一个字符串中的每个单词，包括第一个单词，都是以大些字母开头。因此，“uppercamelcase”就是"UpperCamelCase"。
</td></tr></table>

针对原有代码做出修改

```Dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final wordPair = new WordPair.random();
      return new MaterialApp(
            title:'Welcome to Flutter',
        home:new Scaffold(
              appBar:new AppBar(
            title:new Text('Welcome to Flutter'),
          ),
    body:new Center(
      child:new Text(wordPair.asPascalCase),
    ),
      ),
    );
  }
}
```

5. 如果App正在运行，使用热加载按钮![](/images/flutter/flutter-create-your-first-app/hot_reload_button.png)更新app。每次点击热加载按钮，或者进行保存时，你应该都能在运行的app上看到随机选取的不同的单词对。这是因为单词对是在build()方法中产生，build()方法每次在MaterialApp需要渲染或者在Flutter Inspector中打开Platform时被执行。
![PascalCase运行效果](/images/flutter/flutter-create-your-first-app/flutter_english_words_pair_pascal_case.png)

> <span id="3">Step 3：添加有状态组件</span>

无状态组件(Stateless widget)是不可变的，意味着他们的属性无法改变——所有值是final的。

有状态组件(Stateful widget)维持一个状态值，此状态值会根据组件生命周期而有所改变。实现一个有Stateful widget需要至少两个类：
1. StatefulWidget类用以创建实例；
2. State类。

StatefulWidget组件本身是不可变的，但是State类在整个组件声明周期过程中是始终存在的。

在这步中，你将会添加一个stateful widget，RandomWords以及创建对应的状态State类，RandomWordsState。State类实际上为组件保留着喜欢的单词对。

1. 在main.dart文件中添加RandomWords组件。这个类可以定义在文件内任意位置，但是我将其定义在文件末尾。

```dart
class RandomWords extends StatefulWidget {  
  
  @override  
  createState() => new RandomWordsState();  
}
```

2. 添加RandomWordsState类。大部分的app功能代码都会在这个类中。这个类同时保存用户滚动列表过程中产生的所有单词对，以及用户添加喜欢或者移除的单词对。
下面添加对基本的了定义，来保证类文件编译通过

```dart
class RandomWordsState extends State<RandomWords> {
}
```

3. 添加State类之后，IDE会提示缺少一个build()方法。下一步，需要将产生单词对的代码移至这个build()方法中。

在RandomWordsState的build()方法中添加代码，整个文件看起来像这样

```dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    return new MaterialApp(
      title:'Welcome to Flutter',
      home:new Scaffold(
        appBar:new AppBar(
          title:new Text('Welcome to Flutter'),
        ),
        body:new Center(
          child:new RandomWords(),
        ),
      ),
    );
  }
}

class RandomWords extends StatefulWidget {
  @override
  createState() => new RandomWordsState();
}

class RandomWordsState extends State<RandomWords> {
  @override
  Widget build(BuildContext context) {
    final wordPair = new WordPair.random();
    return new Text(wordPair.asPascalCase);
  }
}
```

4. 重启App运行。
目前修改的代码在运行起来之后，效果与之前的一样，只是将无状态组件换成了有状态组件。

-----

> <span id="4">Step 4：创建无限滚动列表</span>

在这步中，你将扩展RandomWordsState类来展示一个列表。列表随着用户的滚动无限增粘。ListView的builder工厂方法可以按需要进行懒加载。

1. 在RandomWordsState中添加 _suggestions 列表变量保存生成的候选单词对。变量以  (\_) 开头 ——在Dart中，下划线开头的变量强调私有

同时添加比变量 biggerFont 改变字体大小。

```dart
class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];

  final _biggerFont = const TextStyle(fontSize:18.0);
  ...
}
```

2. 在RandomWordsState类中添加 _buildSuggestions()方法。这个类主要功能就是产生需要展示的单词对列表。

Listview提供了builder属性itemBuilder 用以产生item及匿名函数的回调。BuildContext和行的迭代器索引 i ，这两个参数被传到ListView的buil()方法。迭代器索引从0开始增长，每次方法调用产生一个单词对的时候就会增长。这就使得列表在用户滚动时无限增长。
增加代码后，整个类看起来就是

```dart
class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];

  final _biggerFont = const TextStyle(fontSize:18.0);

  final _saved = new Set<WordPair>();

  .....

  Widget _buildSuggestions() {
    return new ListView.builder(
        padding:const EdgeInsets.all(16.0),
        // The itemBuilder callback is called once per suggested word pairing,
        // and places each suggestion into a ListTile row.
        // For even rows, the function adds a ListTile row for the word pairing.
        // For odd rows, the function adds a Divider widget to visually
        // separate the entries. Note that the divider may be difficult
        // to see on smaller devices.
        itemBuilder:(context, i) {
          // Add a one-pixel-high divider widget before each row in theListView.
          if (i.isOdd) return new Divider();

          // The syntax "i ~/ 2" divides i by 2 and returns an integer result.
          // For example:1, 2, 3, 4, 5 becomes 0, 1, 1, 2, 2.
          // This calculates the actual number of word pairings in the ListView,
          // minus the divider widgets.
          final index = i ~/ 2;
          // If you've reached the end of the available word pairings...
          if (index >= _suggestions.length) {
            // ...then generate 10 more and add them to the suggestions list.
            _suggestions.addAll(generateWordPairs().take(10));
          }
          return _buildRow(_suggestions[index]);
        }
    );
  }
}
```

3. 在 _buildSuggestions()方法中调用了 _buildRow()方法。这个函数作用是在ListTile组件中显示新的单词对。ListTile组件可以让你的每行看起来更加具有渲染力。

在RandomWordsState中添加 _buildRow()方法

```dart
class RandomWordsState extends State<RandomWords> {
  ...
  Widget _buildRow(WordPair pair) {
    return new ListTile(
      title:new Text(
        pair.asPascalCase,
        style:_biggerFont,
      ),
    );
  }
}
```

4. 最后来更新RandomWordsState入口函数build()。
整体文件最终代码：

```dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    return new MaterialApp(
      title:'Welcome to Flutter',
      home:new RandomWords(),
    );
  }
}

class RandomWords extends StatefulWidget {

  @override
  createState() => new RandomWordsState();
}

class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];

  final _biggerFont = const TextStyle(fontSize:18.0);

  @override
  Widget build(BuildContext context) {
    return new Scaffold (
      appBar:new AppBar(
        title:new Text('Startup Name Generator'),
      ),
      body:_buildSuggestions(),
    );
  }

  Widget _buildSuggestions() {
    return new ListView.builder(
        padding:const EdgeInsets.all(16.0),
        // The itemBuilder callback is called once per suggested word pairing,
        // and places each suggestion into a ListTile row.
        // For even rows, the function adds a ListTile row for the word pairing.
        // For odd rows, the function adds a Divider widget to visually
        // separate the entries. Note that the divider may be difficult
        // to see on smaller devices.
        itemBuilder:(context, i) {
          // Add a one-pixel-high divider widget before each row in theListView.
          if (i.isOdd) return new Divider();

          // The syntax "i ~/ 2" divides i by 2 and returns an integer result.
          // For example:1, 2, 3, 4, 5 becomes 0, 1, 1, 2, 2.
          // This calculates the actual number of word pairings in the ListView,
          // minus the divider widgets.
          final index = i ~/ 2;
          // If you've reached the end of the available word pairings...
          if (index >= _suggestions.length) {
            // ...then generate 10 more and add them to the suggestions list.
            _suggestions.addAll(generateWordPairs().take(10));
          }
          return _buildRow(_suggestions[index]);
        }
    );
  }

  Widget _buildRow(WordPair pair) {
    return new ListTile(
      title:new Text(
        pair.asPascalCase,
        style:_biggerFont,
      ),
    );
  }
}
```

5. 最终重新启动App运行。
![列表](/images/flutter/flutter-create-your-first-app/flutter_first_app_list_view.png)![滚动列表](/images/flutter/flutter-create-your-first-app/flutter_first_app_infinite_list_view_scrolling.gif)

----

> <span id="5">Step 5：添加交互</span>

这步中，你讲为每行item添加一个可点击的心形图标，在用户点击item时，对应的单词对（word pair）会被添加到收藏或者被移除。

1. 在RandomWordsState类中添加一个 _saved 集合（Set）变量。这个集合保存了用户喜欢并收藏的单词对。之所以使用集合是因为集合可以保证其中没有重复的单词对。

```dart
class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];

  final _biggerFont = const TextStyle(fontSize:18.0);

  final _saved = new Set<WordPair>();
  ...
}
```

2. 在函数_buildRow()中，添加变量alreadySaved来检查用户点击的wordPair是否已经保存。

```dart
  Widget _buildRow(WordPair pair) {
    final alreadySaved = _saved.contains(pair);
    ...
  }
```

3. 同样还需要在函数_buildRow()中，需要在ListTiles中添加心形图标来表示收藏状态。后边，你将会为次添加收藏取消功能的交互。

```dart
  Widget _buildRow(WordPair pair) {
    final alreadySaved = _saved.contains(pair);
    return new ListTile(
      title:new Text(
        pair.asPascalCase,
        style:_biggerFont,
      ),
      trailing:new Icon(
        alreadySaved ? Icons.favorite :Icons.favorite_border,
        color:alreadySaved ? Colors.red :null,
      ),
    );
  }
```

4. 重启App。就将看到列表中每行右侧添加了一个心形图标。
![添加心形图标](/images/flutter/flutter-create-your-first-app/flutter_first_app_list_with_star_icon.png)

5. 为每行添加可点击功能。即若被点击的item对应的单词对已经被收藏了，那么就会被取消收藏，反之就添加到收藏。当一个tile被点击，函数调用setState()通知framework状态发生改变。
添加的代码如下，在_buildRow()方法中进行添加

```dart
  Widget _buildRow(WordPair pair) {
    final alreadySaved = _saved.contains(pair);
    return new ListTile(
      title:new Text(
        pair.asPascalCase,
        style:_biggerFont,
      ),
      trailing:new Icon(
        alreadySaved ? Icons.favorite :Icons.favorite_border,
        color:alreadySaved ? Colors.red :null,
      ),
      onTap:() {
        setState(() {
          if (alreadySaved) {
            _saved.remove(pair);
          } else {
            _saved.add(pair);
          }
        });
      },
    );
  }
```

<table><tr><td bgcolor=LightGreen>Tips：在Flutter的响应框架中，调用setState()方法会出发调用State类的bulid()方法，这就导致了更新UI操作。
</td></tr></table>

重运行App。你应该可以通过点击来添加或者取消收藏。注意的一点是，在点击的时候可以看到一个放射性的点击效果，这是Material风格所致。如果有Android开发经验的程序员就会知道。
![可点击状态](/images/flutter/flutter-create-your-first-app/flutter_first_app_list_item_with_tappable_fun.png)![收藏状态改变](/images/flutter/flutter-create-your-first-app/flutter_first_app_infinite_list_item_tappable.gif)

----
> <span id="6">Step 6：导向新的一屏</span>

在这步中，你将添加一个新屏幕（在Flutter叫做route）来展示你的收藏。你将学习如何在home route和新route之间进行交互。

在Flutter中，导航器(Navigator)管理着所有app route的一个栈。向栈内push一个route就表示这将展示新的一屏。pop出栈表示向前显示一屏。

1. 在RandomWordsState类的build方法中，在AppBar上添加一个列表图标。当用户点击列表icon，包含收藏数据的新的route就会被推送的栈中，并且展示新的一屏。
<table><tr><td bgcolor=LightGreen>Tips：某些组件属性只包含一个子组件(child)，而某些属性（像action）拥有一组子组件(children)，这种方式通过方括号表示([])。
</td></tr></table>

在方法中添加icon和对应的action:

```dart
....
class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];

  final _biggerFont = const TextStyle(fontSize:18.0);

  final _saved = new Set<WordPair>();

  @override
  Widget build(BuildContext context) {
    return new Scaffold (
      appBar:new AppBar(
        title:new Text('Startup Name Generator'),
        actions:<Widget>[
          new IconButton(icon:new Icon(Icons.list), onPressed:_pushSaved),
        ],
      ),
      body:_buildSuggestions(),
    );
  }
  ....
}
```

2. 上边的代码中同时定义了press回到，因此同时需要定义方法

```dart
class RandomWordsState extends State<RandomWords> {
  ...
  void _pushSaved() {
  }
}
  ...
```

此处方法中并未添加任何代码。

2. 在用户点击appbar上的列表icon。系统构建一个route并且push到Navigator的栈中。这种操作改变了屏幕的显示，显示了新的route。
这个新页面的内容在MaterialPageRoute的builder的匿名函数中构建。

添加调用Navigator.push，将route推送的Navigator栈中。

```dart
...
class  RandomWordsState  extends  State<RandomWords>  {
  ...
  void  _pushSaved()  {
    Navigator.of(context).push(
    );
  }  
}
...
```

4. 添加MaterialPageRoute和对应的builder。现在，可以在push方法中添加对应的代码，展示收藏的单词对列表。使用toList()方法转换将最终的数据赋值给divided 变量，使其拥有最终数据。

```dart
void _pushSaved() {  
  Navigator.of(context).push(new MaterialPageRoute(  
    builder:(context) {  
      final tiles = _saved.map(  
            (pair) {     
          return new ListTile(  
            title:new Text(  
              pair.asPascalCase,  
              style:_biggerFont,  
            ),  
          );  
         },  
      );  
      final divided = ListTile  
            .divideTiles(  
              context:context,  
              tiles:tiles,  
             )  
            .toList();  
      },  
    ),);  
}

```

5. builder属性返回一个Scaffold（包含了新route的appbar）组件命名“Saved Suggestions”。新的route由ListTile组件组成的列表组成，ListTiles之间有分隔符分割。

这样整体的代码如下

```dart
void _pushSaved() {
  Navigator.of(context).push(
    new MaterialPageRoute(
      builder:(context) {
        final tiles = _saved.map(
          (pair) {
            return new ListTile(
              title:new Text(
                pair.asPascalCase,
                style:_biggerFont,
              ),
            );
          },
        );
        final divided = ListTile
          .divideTiles(
            context:context,
            tiles:tiles,
          )
          .toList();

        return new Scaffold(
          appBar:new AppBar(
            title:new Text('Saved Suggestions'),
          ),
          body:new ListView(children:divided),
        );
      },
    ),
  );
}
```

6. 重运行App。收藏几个单词对，然后点击appbar上的列表icon，将会出现新的一屏来展示收藏的单词对。注意新出现的一页上默认会又给返回按钮，这个Navigator默认添加的。这样你不用特意为返回另外写程序来执行Navigator.pop来返回。点击返回按钮就可以返回到之前的页面。
整体代码如下

```dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title:'Welcome to Flutter',
      home:new RandomWords(),
    );
  }
}

class RandomWords extends StatefulWidget {
  @override
  createState() => new RandomWordsState();
}

class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];

  final _biggerFont = const TextStyle(fontSize:18.0);

  final _saved = new Set<WordPair>();

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar:new AppBar(
        title:new Text('Startup Name Generator'),
        actions:<Widget>[
          new IconButton(icon:new Icon(Icons.list), onPressed:_pushSaved),
        ],
      ),
      body:_buildSuggestions(),
    );
  }

  void _pushSaved() {
    Navigator.of(context).push(
      new MaterialPageRoute(
        builder:(context) {
          final tiles = _saved.map(
            (pair) {
              return new ListTile(
                title:new Text(
                  pair.asPascalCase,
                  style:_biggerFont,
                ),
              );
            },
          );
          final divided = ListTile
              .divideTiles(
                context:context,
                tiles:tiles,
              )
              .toList();

          return new Scaffold(
            appBar:new AppBar(
              title:new Text('Saved Suggestions'),
            ),
            body:new ListView(children:divided),
          );
        },
      ),
    );
  }

  Widget _buildSuggestions() {
    return new ListView.builder(
        padding:const EdgeInsets.all(16.0),
        // The itemBuilder callback is called once per suggested word pairing,
        // and places each suggestion into a ListTile row.
        // For even rows, the function adds a ListTile row for the word pairing.
        // For odd rows, the function adds a Divider widget to visually
        // separate the entries. Note that the divider may be difficult
        // to see on smaller devices.
        itemBuilder:(context, i) {
          // Add a one-pixel-high divider widget before each row in theListView.
          if (i.isOdd) return new Divider();

          // The syntax "i ~/ 2" divides i by 2 and returns an integer result.
          // For example:1, 2, 3, 4, 5 becomes 0, 1, 1, 2, 2.
          // This calculates the actual number of word pairings in the ListView,
          // minus the divider widgets.
          final index = i ~/ 2;
          // If you've reached the end of the available word pairings...
          if (index >= _suggestions.length) {
            // ...then generate 10 more and add them to the suggestions list.
            _suggestions.addAll(generateWordPairs().take(10));
          }
          return _buildRow(_suggestions[index]);
        });
  }

  Widget _buildRow(WordPair pair) {
    final alreadySaved = _saved.contains(pair);
    return new ListTile(
      title:new Text(
        pair.asPascalCase,
        style:_biggerFont,
      ),
      trailing:new Icon(
        alreadySaved ? Icons.favorite :Icons.favorite_border,
        color:alreadySaved ? Colors.red :null,
      ),
      onTap:() {
        setState(() {
          if (alreadySaved) {
            _saved.remove(pair);
          } else {
            _saved.add(pair);
          }
        });
      },
    );
  }
}

```

![新route展示](/images/flutter/flutter-create-your-first-app/flutter_first_app_navigate_to_a_new_route.gif)

----

> <span id="7">Step 7：使用主题修改UI</span>

这是最后一步，你将使用theme。主题（theme）主要控制app看起来外表如何。可以使用默认的主题，从文章开始到目前使用的一直是默认的主题。主题不依赖与物理设备或者模拟器。你也可以自定义自己的主题来突显你自己的品牌。

1. 通过ThemeData类你可以很简单的修改app的主题。
像下边一样修改下代码，就可以改变页面的主标题主题

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title:'Startup Name Generator',
      theme:new ThemeData(
        primaryColor:Colors.white,
      ),
      home:new RandomWords(),
    );
  }
}
```

2. 重运行App看看效果。需要注意的是整个的页面背景是白色的，甚至appbar也是白色的。
![主题](/images/flutter/flutter-create-your-first-app/flutter_first_app_use_theme_to_change_ui.png)

----

好了，这章的内容就是这些了。

