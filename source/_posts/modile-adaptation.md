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

---

## 展望未来 vm
基于rem布局的缩放布局都需要依赖JS去计算基准值，而使用vm则很简单的就能实现百分比缩放布局。
1vw表示百分之一的视窗宽度，同理10vw就是百分之十。但因兼容性方面限制，在国内目前是无法展示vw的身手了，相信未来vw会是主流。
查看兼容：[http://caniuse.com/#search=vw](http://caniuse.com/#search=vw)

---

## html+css基础布局
### 不要用 **fixed**
`position:fixed ` 这css属性在ios上就是个坑
* 滑动时会抖动
* 软键盘弹出来时，fixed失效，滚动页面，加了fixed的元素也会跟着滚动
* 滚动容器加了`-webkit-overflow-scrolling: touch` ，滚动时fixed不会跟着屏幕固定，停止滚动后才会固定

**解决方案**
* 使用div内滚动**（推荐）**
* 用position:absolute模拟，这个效果不佳，类似IE6的hack...
* 当input元素focus时，改成position:absolute，blur的时候再改回来
* 使用iscroll库

总之就是不要用 `position:fixed `，然后使用div内滚动。

### 不要使用**iScroll**之类的库。
**慎用模拟滚动容器**，因为性能！还有一点模拟的惯性滚动跟原生有体验习惯上的差异。如果是为了解决上拉刷新下拉加载又或是ios fixed的坑，都有基于原生滚动的解决方案。


### 弹层用fixed定位？
如果确保弹层上没有input，那倒随意（absolute或fixed），能定位就行，如果有input，那fixed又要开始坑了。

弹层主要解决的坑是：**滚动穿透**， 移动端弹出fixed弹层，在弹层上滑动会导致下层的页面跟着滚动。
**解决方案**
* 使用div内滚动**（推荐）**

* touchmove + preventDefault 阻止滚动
    缺点：需要配合JS

* body overflow: hidden
    缺点：需要配合JS

* body position: fixed
    缺点：需要配合JS

后面3种的解决方案，就不详细说了，看文章[https://segmentfault.com/a/1190000005617307](https://segmentfault.com/a/1190000005617307)。


### 最挂方案：使用内滚动布局
基于上述的一些问题，目前使用内滚动布局是最好的解决方案。
首先，内滚动那就先把window的滚动条干掉，然后给个div设置滚动取代window的滚动。
基本的html
```html
<body>
<div class="scrollWrapper">
    <div style="height:2000px;background:#ff9">内容<br><br><br>
    </div>
</div>
</body>
```
```css
html, body, .scrollWrapper {
  height: 100%;
  overflow: hidden;
}

.scrollWrapper {
  -webkit-overflow-scrolling: touch;
  overflow-y: auto;;
}
```
再模拟fixed元素及弹层
```html
<body>
<div class="layer" style="display:none;">
    我是弹层，滑动不会穿透
</div>
<div class="scrollWrapper">
    <div style="height:2000px;background:#ff9">内容<br><br><br>
        <input type="text"/>
        <button onclick="showLayer()">点击显示弹层</button>
    </div>
</div>
<footer class="footer">
    <a>menu1</a>
    <a>menu2</a>
</footer>
<script>
  function showLayer() {
    document.querySelector('.layer').style.display = 'block'
  }
</script>
</body>
```
```css
html, body, .scrollWrapper {
  height: 100%;
  overflow: hidden;
}

.scrollWrapper {
  -webkit-overflow-scrolling: touch;
  overflow-y: auto;;
}

footer.footer {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  background: #f00;
}

.layer {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, .8);
  z-index: 1;
}
```
例子完整代码：[https://github.com/fzred/example/tree/master/mobile-layout](https://github.com/fzred/example/tree/master/mobile-layout)

在手机上看看试试 ：[http://www.lred.me/example/mobile-layout/index.html](http://www.lred.me/example/mobile-layout/index.html) ，可以扫下面的二维码

<img width=250 src="http://ww3.sinaimg.cn/large/005FY9HCgw1fa9gitgs5ij30r80r874z.jpg"/>

---

## 关于flex

---

## 碰到的坑 
* 在android webview里，获取的font-size会受到系统设置字体大小的影响，可用下面这两行代码测试
```javascript
document.documentElement.style.fontSize = '100px';
console.log(window.getComputedStyle(document.documentElement).fontSize)
```
    对rem这种缩放布局，影响挺大。我也给 [lib-flexible](https://github.com/amfe/lib-flexible) 提了pr，但还没被合并。
    地址：[fix android webview 里 html font-size 因设置系统字体大小受到影响](https://github.com/amfe/lib-flexible/pull/79/commits/587ea50f48af8f480cc4bcac5adba4eae74fd8ad)
* 123 