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

**这一切都是因为CSS的作用是全局的**

"局域化 css"也是组件化很重要的一步，目前也有各种解决方案：
* 未来的 WebComponent 标准
* Polymer、Angular 2、Vue 等类似 Web Component 标准的MVVM框架
* 基于React的 **CSS in JS**



