title: css 局部作用域
categories: 笔记
date: 2016-07-24 16:16:13
tags:
---
<!--摘要-->
<!--more-->
## 目前 CSS 最迫切需要的功能应该就是局部作用域了。

相信写css的人都会遇到class命名的问题：
* 这个class的命名好像不太贴切，其他组件有没有也用到这个class，要是冲突了怎么办。
* 改别人css代码的时候则会一直有个疑问：这个class到底是只在这个地方用了，还是其他地方都用了？

于是一般这么做：
* class命名写长一点吧，降低冲突的几率
* 加个父元素的选择器，限制范围
* 重新命名个class吧，比较保险

**因为CSS的作用是全局的**

"局域化 css"也是组件化很重要的一步，目前也有各种解决方案：
* 未来的 WebComponent 标准
* Polymer、Angular 2、Vue 等类似 Web Component 标准的MVVM框架
* 基于React的 CSS in JS

流行的解决方案，都需要webpack、browserify又或是需要高级的浏览器支持。

之前有个PC端的项目，为了解决SEO，需要服务端渲染，及支持IE8，于是放弃了各种前端渲染的MVVM框架，node+express+ejs做的UI渲染层。

通过include进来的部件是没有组件化的概念的，直接就成了HTML的一部分，针对page写的css可能就应用到widget上面，反过来widget也可能影响到page。
```ejs
<% include widget/header %>
```

于是...
### 方案1-加个父元素的选择器
给page的css加个父元素的选择器吧
```css
.page-index{
    h1{
        font-size:100px;
    }
}
```
然而还是避免不了冲突的问题

```html
<div class="page-index">
    <h1>hello</h1>
    <% include widget/header %>
</div>
```
如果**widget/header**也包含**h1**，很明显也会应用到page的样式。

所以也没有很好的解决冲突的问题

### 方案2-BEM命名
类似于：
```css
.block{}
.block__element{}
.block--modifier{}
```
问题： 
1. 这么长，影响书写效率
2. html和css的size会大一些
3. 不爽（这点对我来说很重要）


### 方案3-标签加唯一的属性
```html
<h1 _c86f0316>hello</h1>
```
```css
h1[_c86f0316] {
    font-size: 100px;
}
```
确保page跟wedget的唯一属性不一致，也就解决了样式冲突的问题。

这似乎跟css BEM命名方式类似了，就是确保选择器唯一，对的，一样的思路。但解决了BEM命名很长的问题，写起来也爽多了。

同样会有“html和css的size会大一些”的问题，不过这个在gzip面前，跟带来的开发体验上可以忽略。

这方案通过在项目的实践，证明是可行的。我也整理了下，写了个gulp的插件。详细的使用方式，看项目主页吧。

<div class="github-widget" data-repo="fzred/gulp-local-css"></div>
<script src="http://git.hust.cc/GitHub-Repo-Widget.js/GithubRepoWidget.js"></script>

我推崇下面的目录结构，**按模块划分**。所以插件默认也是只支持下面的方式。但如果的你的项目不是，glup-local-css 提供了参数可以配置，足以满足不同目录结构的项目。
```
src
    ├─pages
        ├─about
            index.html
            index.css
    ├─widget
        ├─header
            index.html
            index.css
```
这也推荐下云龙大神的一篇文章

[前端工程——基础篇](https://github.com/fouber/blog/issues/10)

![](https://github.com/fouber/blog/raw/master/201508/assets/components.png)


项目地址
* [gulp-local-css](https://github.com/fzred/gulp-local-css)

例子
* [simple](https://github.com/fzred/gulp-local-css/tree/master/examples/simple)

