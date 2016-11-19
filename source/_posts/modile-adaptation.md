title: 移动端适配浅谈
categories: 笔记
date: 2016-11-17 22:44:58
tags:
---
<!--摘要-->
<!--more-->

## 主流适配方案 rem
>针对不同手机屏幕尺寸和dpr动态的改变根节点html的font-size大小(基准值)。

应该是目前最好的适配方案，关于这方面的文章：

[移动端高清、多屏适配方案](http://www.html-js.com/article/Mobile-terminal-H5-mobile-terminal-HD-multi-screen-adaptation-scheme%203041)
[使用Flexible实现手淘H5页面的终端适配](http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)


## 展望未来 vm
基于rem布局的缩放布局都需要依赖JS去计算基准值，而使用vm则很简单的就能实现百分比缩放布局。
1vw表示百分之一的视窗宽度，同理10vw就是百分之十。但因兼容性方面限制，在国内目前是无法展示vw的身手了，相信未来vw会是主流。
查看兼容：[http://caniuse.com/#search=vw](http://caniuse.com/#search=vw)

## html+css基础布局
### 不要用 **fixed**
`position:fixed ` 这css属性在ios上就是个坑
* 滑动时会抖动
* 软键盘弹出来时，fixed失效，滚动页面，加了fixed的元素也会跟着滚动
* 滚动容器加了`-webkit-overflow-scrolling: touch` ，滚动时fixed不会跟着屏幕固定，停止滚动后才会固定

总之就是不要用 `position:fixed `。别急，下面有其他替代fixed的解决方案。

### 不要使用**iScroll**之类的库。
因为性能！还有一点模拟的惯性滚动跟原生有体验习惯上的差异。上拉刷新下拉加载又或是解决ios fixed的坑，都有基于原生滚动的解决方案。
**模拟的滚动容量慎用**

### 弹层怎么搞

### 最挂方案：使用内滚动


## 关于flex

## 碰到的坑 
* 在android webview里，获取的font-size会受到系统设置字体大小的影响，可用下面这两行代码测试
```javascript
document.documentElement.style.fontSize = '100px';
console.log(window.getComputedStyle(document.documentElement).fontSize)
```
    对rem这种缩放布局，影响挺大。我也给 [lib-flexible](https://github.com/amfe/lib-flexible) 提了pr，但还没被合并。
    地址：[fix android webview 里 html font-size 因设置系统字体大小受到影响](https://github.com/amfe/lib-flexible/pull/79/commits/587ea50f48af8f480cc4bcac5adba4eae74fd8ad)
* 123 