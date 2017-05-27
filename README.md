# techinsight.github.io

techinsight 个人技术博客文章工程管理。
---

使用litten-yilia主题，在查看“所有文章”前需要保证：

1. nodejs版本大于6.2（最新的nodejs肯定符合）。
2. 在博客根目录下（不是yilia根目录下）执行如下命令：
    npm i hexo-generator-json-content --save
3. 在根目录_config.xmlw文件内配置：
jsonContent:
    meta: false
    pages: false
    posts:
      title: true
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

