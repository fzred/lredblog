title: 移动端适配浅谈
categories: 笔记
date: 2016-11-17 22:44:58
tags:
---
<!--摘要-->
<!--more-->

## 主流方案 rem
    针对不同手机屏幕尺寸和dpr动态的改变根节点html的font-size大小(基准值)。
关于这方面的文章

[移动端高清、多屏适配方案](http://www.html-js.com/article/Mobile-terminal-H5-mobile-terminal-HD-multi-screen-adaptation-scheme%203041)
[使用Flexible实现手淘H5页面的终端适配](http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)

## 展望未来 vm

## 不要用 fixed


## 碰到的坑 
1. 在android webview里，获取的font-size会受到系统设置字体大小的影响，可用下面这两行代码测试
```javascript
document.documentElement.style.fontSize = '100px';
console.log(window.getComputedStyle(document.documentElement).fontSize)
```
    对rem这种缩放布局，影响挺大。我也给 [lib-flexible](https://github.com/amfe/lib-flexible) 提了pr，但还没被合并。地址：[fix android webview 里 html font-size 因设置系统字体大小受到影响](https://github.com/amfe/lib-flexible/pull/79/commits/587ea50f48af8f480cc4bcac5adba4eae74fd8ad)
2. 