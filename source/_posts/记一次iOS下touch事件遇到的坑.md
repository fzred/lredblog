title: 记一次iOS下touch事件遇到的坑
categories: 笔记
date: 2015-12-10 06:09:41
tags: [移动端]
---
<!--摘要-->
<!--more-->
上星期项目有个拖动的效果，小意思啦，分分钟搞定，然而在测试时发现iOS下表现却不正常。
如下代码，android、ios运行结果不一样
```html
<body style="background:#333;height:100%">
</body>
<script type="text/javascript">
    var startPoint = {};
    document.body.addEventListener("touchstart", function (e) {
        startPoint = e.touches[0];
        console.log("touchstart", startPoint.pageX);
    });
    document.body.addEventListener("touchmove", function (e) {
        console.log("touchmove", startPoint.pageX);
    });
    document.body.addEventListener("touchend", function (e) {
        console.log("touchend", startPoint.pageX);
    });
</script>
```

android
![](/imgs/20151210062429.png)

iOS
![](/imgs/20151210062755.png)

可以看到iOS下startPoint.pageX是跟着touchmove,touchend事件在变，看到这里，我在想
touchstart事件的 *e.touches[0]* 跟 touchmove touchend 里的会不会是同一个？
好吧，试一下就知道了。
```html
<body style="background:#333;height:100%">
</body>
<script type="text/javascript">
    var point = {};
    document.body.addEventListener("touchstart", function (e) {
        point = e.touches[0];
        console.log("touchstart", point.pageX);
    });
    document.body.addEventListener("touchmove", function (e) {
        console.log("touchmove", point.pageX, e.touches[0] === point);
    });
    document.body.addEventListener("touchend", function (e) {
        console.log("touchend", point.pageX, e.changedTouches[0] === point);
    });
</script>
```

android
![](/imgs/20151210063349.png)

iOS
![](/imgs/20151210063500.png)

确实是这样。

## 解决办法
把 *pageX* 保存一下 `point.pageX = e.touches[0].pageX;`
```html
<body style="background:#333;height:100%">
</body>
<script type="text/javascript">
    var point = {};
    document.body.addEventListener("touchstart", function (e) {
        point.pageX = e.touches[0].pageX;
        console.log("touchstart", point.pageX);
    });
    document.body.addEventListener("touchmove", function (e) {
        console.log("touchmove", point.pageX);
    });
    document.body.addEventListener("touchend", function (e) {
        console.log("touchend", point.pageX);
    });
</script>
```