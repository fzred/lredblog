title:  hexo+github pages搭建博客
date: 2015-11-16 22:22:50
categories: 笔记
tags: [hexo,git]
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
* [github pages域名绑定](#github pages域名绑定)
* [常用插件](#常用插件)

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
 
 ## 写篇文章
 ```bash
 hexo new post 文章标题
 ```
 
 当然你也可以在 `sourc\_posts` 目录新建 .md 文件。
 
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
这里是余下全文
```

**！注意冒号 : 后面一定要带空格**

如果不打算发布，还只是草稿，可以将md文件放到 `source\_drafts` 目录，就不会出现到列表了，执行`hexo s --draft`命令可以预览。

## 更换主题
[有那些好看的hexo主题？](http://www.zhihu.com/question/24422335)，看了前十几个，都挺好看，但我还是觉得默认的主题好看点。。。
可以找下如果有喜欢的主题可以放到根目录的 themes 文件夹下，然后更改 *_config.yml* 文件
```bash
theme: landscape #主题名称，对应 \themes 下文件夹的名称
```
更改完 *_config* 文件记得重新执行 `hexo s` 才能看到效果哦。 
 
 ## 主题的修改
 持续更新中~

## 自定义页面
在 `/source` 文件夹下新建 md 文件都会生成对应的 `md文件名.html` 。
比如需要一个 *关于我* 的页面，路径是 [www.lred.me/about/index.html](http://www.lred.me/about/index.html) 则在`/source/about/` 新建 `index.md` 文件。
但发现新建的页面布局跟博客文章的布局一样，如果需要更改可以参考 [Pacman主题下给Hexo增加简历类型](http://blog.zanlabs.com/2015/01/02/add-resume-type-to-hexo-under-pacman-theme/)


## 404页面
GitHub Pages 自定义404页面非常容易，直接在source根目录下创建自己的404.html就可以。但是自定义404页面仅对绑定顶级域名的项目才起作用，GitHub默认分配的二级域名，跟你本地的127.0.0.1是不起作用的。
所以我们在 `/source/` 新建 `404.md` 。

## 图片放哪
1. 可以使用七牛，将图片上传至七牛空间后拿到图片地址，就可以插入到文章了
2. 图片放在hexo，发布到github上，我现在是这么干的，在国内访问速度肯定是没七牛的快，不过我觉得够用了，上传到七牛，分开两个地方，维护又变麻烦了。
我是将图片都放 `/source/imgs` 目录下，使用时直接写
```md
![](/imgs/1.png)
```

## 评论插件
我使用的是 [多说](http://duoshuo.com/) ，注册申请后做以下修改
* 在 *_config.yml* 最后添加以下代码
```bash 
# Duoshuo
duoshuo_shortname: lred #你在 多说 的域名 比如我说是 lred.duoshuo.com  就填 lred
```
* 打开文件 `\themes\landscape\layout\_partial\after-footer.ejs` 最后添加
```html
<!-- 多说公共js代码 end -->
<% if (config.disqus_shortname){ %>
<script>
  var disqus_shortname = '<%= config.disqus_shortname %>';
  <% if (page.permalink){ %>
  var disqus_url = '<%= page.permalink %>';
  <% } %>
  (function(){
    var dsq = document.createElement('script');
    dsq.type = 'text/javascript';
    dsq.async = true;
    dsq.src = '//' + disqus_shortname + '.disqus.com/<% if (page.comments) { %>embed.js<% } else { %>count.js<% } %>';
    (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
  })();
</script>
<% } %>
```


## 文章搜索
[利用swiftype为hexo添加站内搜索](http://www.jerryfu.net/post/search-engine-for-hexo-with-swiftype.html)


## 处理原先的文章地址
之前的博客，有几篇文章有点访问量，搬家后url又跟之前不兼容，又不想让收藏了文章的人进来后发现404了，可以参考下这么干。
比如我之前文章链接是 [http://www.lred.me/article/30](http://www.lred.me/article/30) 
新文章链接是 [http://www.lred.me/2015/04/16/git%20sourcetree%20解决冲突/](http://www.lred.me/2015/04/16/git%20sourcetree%20%E8%A7%A3%E5%86%B3%E5%86%B2%E7%AA%81/)
在 `/source/article/30/` 新建 `index.md` 文件
```html
title: 文章已经搬家了，正在跳转
---
<script>location.href="/2015/04/16/git%20sourcetree%20解决冲突/"</script>
```
进来后就自动跳转到新的地址了。

## 部署到github pages上
1. 在github创建Repository
创建的时候注意Repository的名字。比如我的Github账号是**fzred**，那么我应该创建的Repository的名字是：**fzred.github.io** 。
然后copy ssh url，注意是 **ssh** 的，不然 `hexo d` 无法上传到github上。
2. 修改 `_config.yml` 
```bash
deploy:
  type: git
  repository: git@github.com:fzred/fzred.github.io.git  #Repository的ssh url
  branch: master
  message: hexo deploy
```
3. 设置SSH keys
怎么设置[参考](http://jingyan.baidu.com/article/a65957f4e91ccf24e77f9b11.html),
验证是否成功
```bash
ssh -T git@github.com
```
4. 最后使用hexo发布到github上
``` bash
hexo g
hexo d
```
5. 到这就能访问了 [fzred.github.io](http://fzred.github.io) , fzred换成你自己的名字

## github pages域名绑定
1. 添加两条域名A记录的域名解析IP分别是 **192.30.252.153** 、**192.30.252.154** 
2. 上传CNAME
github pages绑定需要在，代码的根目录下新建一个名为CNAME的文件把你的域名写进去
比如我的域名是 [www.lred.me](http://www.lred.me) ，既在 `/source/` 新建 `CNAME` 文件，内容写上 **www.lred.me**
3. 部署到github pages
```bash
hexo g
hexo d
```

## 常用插件
1. hexo-generator-feed rss订阅功能
```bash
npm install hexo-generator-feed --save
```
  之后重新部署，访问/atom.xml。
2. hexo-generator-sitemap  sitemap
```bash
npm install hexo-generator-sitemap --save
```
  之后重新部署，访问/sitemap.xml,可以把sitemap提交到搜索引擎的站长平台来增加收录。