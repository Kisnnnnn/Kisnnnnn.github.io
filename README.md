## 2019年02月13日
最近看到很多大牛开始跳槽，我自己想了想，就现有技术而言，万一公司把我开了，好像自己也没地方可以去了，因为自己太菜了，我之前在网上看到一段话，也送给现在正在踌躇是否继续专研技术还是转管理的人
> 「在公司红利期的成就和业绩并不能代替你现有的成绩，想想自己跳出红利平台和舒适区是否还能继续你的辉煌？」

如何在线预览
------------------------------------
```
sudo gem install jekyll
sudo gem install bundler
sudo gem install jekyll-paginate
sudo gem install jekyll-gist


cd prpject
jekyll serve 
```
如何写文章
------------------------------------

在`LessOrMore/_posts`目录下新建一个文件，可以创建文件夹并在文件夹中添加文件，方便维护。在新建文件中粘贴如下信息，并修改以下的`titile`,`date`,`categories`,`tag`的相关信息，添加`* content {:toc}`为目录相关信息，在进行正文书写前需要在目录和正文之间输入至少2行空行。然后按照正常的Markdown语法书写正文。

``` bash
---
layout: post
#标题配置
title:  标题
#时间配置
date:   2016-08-27 01:08:00 +0800
#大类配置
categories: document
#小类配置
tag: 教程
---

* content
{:toc}


我是正文。我是正文。我是正文。我是正文。我是正文。我是正文。
```

执行
------------------------------------

``` bash
jekyll server
```

效果
------------------------------------
打开浏览器并输入URL`http://localhost:4000/`,回车。

