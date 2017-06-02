---
title: 使用hexo+github搭建个人微博——手把手教
tags: []
categories: []
date: 2017-05-31 17:17:58
---

> 先说说为什么想到搭建个人微博

工作多年，没有认真整理过自己工作中的经验，遇到的问题及解决方案。因此想着认真整理下过去到现在过程中的问题。废话不多说，下来看看我是如何来搭建我的个人微博的。

> 前提条件

* 需要有个github账号。
  一般做为一个开发人员，github肯定是访问过的，相信绝大多数开发人员会注册拥有一个github账号，因为这是全球知名的代码托管网站。全世界均可访问到它。若还没有可以到[Github](https://github.com/ "Github")注册。

* 基本软件NodeJS，Git，Hexo
  在机子上需要使用到NodeJS进行部署，安装插件等。下载[NodeJs](https://nodejs.org/ "NodeJs")。
  安装Git，进入Git[下载页面](https://git-scm.com/download/)选择合适的版本进行下载。
  如果不清楚Hexo是什么？？她是一个快速，简介，高效的博客框架。更多详情可以到[Hexo官网](https://hexo.io/zh-cn/docs/index.html)读下这个文档就知道了。O(∩_∩)O哈哈~

<!-- more -->

安装完成Git及NodeJs后，那么就可以开始准备安装Hexo了。
打开GitBash，使用npm命令来安装Hexo程序。
![git bash命令行](/images/how-to-setup-personal-blog-width-hexo-and-github/bit_bash_command_ui.png)

回车，等待安装，安装完成后有如下信息：
![安装提示](/images/how-to-setup-personal-blog-width-hexo-and-github/hexo_install_hint_first.png)
........
![提示最后](/images/how-to-setup-personal-blog-width-hexo-and-github/hexo_install_hint_last.png)

这样表示Hexo安装成功了。
到此，搭建个人微博需要的3个软件就安装完成了。

接下来就需要进行相关配置及插件安装了。
所有的操作均在GitBash命令行中进行操作。

> 建站

首先简历一个简单的站，创建初始化一个简单的文件夹。

    $ hexo init myGitPages
    $ cd myGitPages
    $ npm install

这样就可以新建所需要的文件。
安装完成后，可以看到如下的几个主要文件。
![安装结构](/images/how-to-setup-personal-blog-width-hexo-and-github/hexo_site_installed_hierarchy.png)

其中:
_config.xml      可以配置网站信息。可以参考[配置](https://hexo.io/zh-cn/docs/configuration.html)
package.json    应用程序信息。有默认配置一些组件，可以自己根据需要添加或者移除。
scaffolds          模板文件夹。当新建文章时，Hexo会根据scaffold来建立文件。
source              资源文件夹，存放用户资源的地方。
themes             主题文件夹。Hexo会根据主题来生成静态页面。

> 主题

Hexo可以有很多主题，在[Github](https://github.com/)首页搜索框内输入"hexo theme"，可以搜索到很多主题，目前我使用的是litten的[yilia](https://github.com/litten/hexo-theme-yilia)主题。

可以在进入到建立的站点文件夹下（我的是myGitPages）下，下载yilia主题

    $ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia

下载完成后，打开站点个目录下（myGitPages）的_config.xml文件，修改其中的theme属性成

    theme: yilia

这样就可以在发布文章的手看到修改的主题，或者通过hexo server进行查看。

> 发布

发布文章前一定要确认已经安装了hexo-deployer-git插件，否则无法正常发文章到git上的。

    $ npm install hexo-deployer-git --save

来安装git插件。

到目前基本上需要的从博客站点建立，到发布所需要的软件准备工作都就绪了。

> ###### PS：yilia要显示所有文章还需要进行一个配置，下文会写到。

在开始写文章发布之前可以，先查本地运行查看Hexo运行情况，运行

    $ hexo server

启动服务器。默认情况下，端口地址是4000。打开浏览器，访问：http://localhost:4000
查看效果。<font color="red">若访问出错，没有打开页面，那么可能是端口被占用导致。此时可以使用</font>

    $ hexo server -p 5000

<font color="red">修改端口，然后重新输入端口号进行查看。</font>
若访问成功，默认看到的是hello-wold.md（即source/_post文件夹下的默认创建文件）文件发布后的效果。

> 准备工作结束，可以开始写文章发布

上述工作都结束后，可以进入到***站点文件夹/source/_post/***目录下新建md文件，使用Markdown标记语言写一些内容。

Markdown标记语言使用比较简单，可以在[这里](http://wowubuntu.com/markdown/#img)学习简单的使用，或者[markdown.cn](http://markdown.cn/)学习使用。

在写完文章之后就需要将写的文章部署到GitHub上去了。
来看下这个过程。

由于要发布博客到GitHub实际上使用了GitHub Pages功能，因此可以到[这里](https://pages.github.com/)来查看相关的介绍。其中详细介绍了Pages概念，及如何建立自己的站点。

一下先简绍下我自己的建立过程。

> 建立GitHub仓库

建立仓库，用以部署Hexo生成的博客。如果还没有GitHub账号就需要注册了。
有GitHub账号的小伙伴可以到[Github](https://github.com/)网站[创建一个新仓库](https://github.com/new)，如下显示。
![如图显示](/images/how-to-setup-personal-blog-width-hexo-and-github/create_a_new_repository_on_github.png)

其中仓库名有所讲究，它必须是*username.github.io* ，这里的username有两种情况，其一是你的用户名即注册时使用的名称，其二是组织名称（此处组织名称并未尝试）。如下图可以看下位置。
![新仓库名臣](/images/how-to-setup-personal-blog-width-hexo-and-github/new_repository_name_rule.png)

> Hexo发布前需要配置_config.xml

在使用Hexo发布博客之前，当然还需要让Hexo知道要发布到什么地方。

在站点根目录下（myGitPages），找到并编辑_config.xml文件，如下部分：

    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repository: git@github.com:techinsight/techinsight.github.io.git
      branch: master
      message:

将新建的仓库信息及分支信息配置到_config.xml，让Hexo知道部署到GitHub的那个仓库。

> 发布

最后，就需要发布了。可以在站点根目录下（myGitPages）下运行一下命令：

    $ hexo clean #清除原有生成的相关文件
    $ hexo generate # 重新生成静态页面
    $ hexo deploy # 进行部署

在运行部署命令时，会弹出ssh密码输入确认框，输入*username.github.io*仓库密码，点击OK即可进行顺利发布。

然后就去访问你的个人站点吧。你会看到发布的文章。

### <font color="red">PS: 不同的主题可能在主题中还需要进行一定的配置，这个看个人喜好使用哪个主题，我使用的是yilia主题，其在初次查看所有文章时会发现展示的不是文章列表。那么按照主题作者提示进行配置后，重新部署就可以了。</font>

1. nodejs版本大于6.2（最新的nodejs肯定符合）。
2. 在博客根目录下（不是yilia根目录下）执行如下命令： npm i hexo-generator-json-content --save
3. 在根目录_config.xmlw文件内配置： 

    jsonContent: 
        meta: false
        pages: false 
        posts: title: true 
        date: true 
        path: true 
        text: false 
        raw: false 
        content: false 
        slug: false 
        updated: false 
        comments: false 
        link: false 
        permalink: false 
        excerpt: false 
        categories: false 
        tags: true



