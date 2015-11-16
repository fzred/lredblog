title:  hexo+github pages搭建博客
date: 2015-11-16 22:22:50
tags:
render_drafts: true
---
顺利的将原来go写的博客迁移到了github pages上，用hexo生成的静态页，主要是懒得去维护服务器了，之前写的一些文章，大部分都没什么价值了，就不迁到新博客了，重新开始。
之前也一直在这迁移博客的想法，一直没去行动，这次是因为原博客不支持markdown，懒得开发又刚好有时间，就折腾了下，也遇到点坑，也就有了这篇笔记。

<!--more-->

写这篇文章时，hexo的版本是 3.1.1 ，网上有一些文章说的是2.*，2到3是有些地方不兼容的，文章也没有特别标注，所以看的时候注意看下文章发表的时间，以免踩坑。
网上文章也很多，但我还是要再写一篇，因为我看网上文章来的时候，还是碰到不少坑，也是看了多篇文章才解决了。本文也是方便hexo的入门，更权威的请查看[官方文档](https://hexo.io/)
## 目录索引
* [hexo介绍](#hexo介绍)
* [hexo安装](#hexo安装)
* [写篇文章](#写篇文章)
* [更换主题](#更换主题)
* [主题的修改](#主题的修改)
* [自定义页面](#自定义页面)
* [404页面](#404页面)
* [图片放哪](#图片放哪)
* [评论插件](#评论插件)
* [文章搜索](#文章搜索)
* [处理原先的文章地址](#处理原先的文章地址)
* [部署到github pages上](#部署到github pages上)

## hexo介绍
Hexo 是一个简单地、轻量地、基于Node的一个静态博客框架。不需要数据库，写完md文件直接生成html文件，文章评论使用社会化评论插件，文章搜索配合swiftype也可以很好解决。markdown的写作方式很适合程序员，我表示用起来非常爽。。。

## hexo安装
使用*npm*
```bash 
npm install -g hexo
```
安装完在一个空目录执行
```bash
hexo init
npm install
hexo s
```
然后打开 [127.0.0.1:4000](http://127.0.0.1:4000) ，就可以看到默认的网页界面了。
此时如果你很想看到在外网访问的样子，就直接跳到 [部署到github pages上](#部署到github pages上) 。

## 更换主题
[有那些好看的hexo主题？](http://www.zhihu.com/question/24422335)，看了前十几个，都挺好看，但我还是觉得默认的主题好看点。。。
可以找下如果有喜欢的主题可以放到根目录的 themes 文件夹下，然后更改 *_config.yml* 文件
```
 theme: 主题名称
 ```
 更改完 *_config* 文件记得重新执行 `hexo s` 才能看到效果哦。 
 
 ## 主题的修改
 
 
 ## 写篇文章
 ```bash
 hexo new post 文章标题
 ```
 当然你也可以在 *sourc/_posts* 目录新建 .md 文件。
```bash
title: git sourcetree 解决冲突  #文件标题
date: 2015-04-16 14:55:48	   #文章生成时间
categories: 笔记               #分类
tags: [git,sourcetree]         #标签 数组形式 
layout: post				   #使用的布局，在做自定义页面时会很有用，默认 post
link: http://baidu.com		  #文章自定义链接 默认是_config文件配置的 :year/:month/:day/:title/

---  #文章内容用 --- 分割
这里是文章摘要
<!--more-->   #<!--more-->来分割文章摘要，下面是余下全文
以下是余下全文

```  

**！注意冒号 : 后面一定要带空格**

如果不打算发布，还只是草稿，可以将md文件放到 *source/_drafts* 目录，就不会出现到列表了，执行`hexo s --draft`命令可以预览。


## 自定义页面



## 文章搜索
[利用swiftype为hexo添加站内搜索](http://www.jerryfu.net/post/search-engine-for-hexo-with-swiftype.html)
