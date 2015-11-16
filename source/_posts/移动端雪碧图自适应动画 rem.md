title: 移动端雪碧图自适应动画 rem
date: 2015-10-29 17:08:51
categories: 前端
tags: [angularjs,css]

---
![](/imgs/1446108265323000400.gif)
<!--more-->
一开始的方案是每一帧的图片分开，到哪一帧就显示哪一个img标签，写起来也是相对简单，但下图这个动画用了64张图片，导致加载很慢。
于是，使用雪碧图的方法来做。但在移动端上写起来并不简单，需要将图片放大或缩小来自适应各种设备。

### javascript
``` javascript
myapp.directive("pngAnimation", function ($timeout) {
    "use strict";
    return {
        restrict: 'AE',
        link: function (scope, element, attr) {
            var img,
                count = parseInt(attr.count),
                path = attr.path,
                countTime = parseInt(attr.time),
                curIndex = 0;
            img = new Image();
            img.onload = function () {
                execute();
            };
            img.src = path;
            element.css({
                "background-image": "url(" + path + ")"
            });
            function execute() {
                if (curIndex >= count) {
                    return
                };
                $timeout(function () {
                    element.css({
                        "background-position": ((curIndex + 1) / (count)) * 100 + "% 0%"
                    });
                    curIndex++;
                    execute();
                }, countTime / count);
            }
        }
    }
});
```

### html
```
<div class="png_animation" png-animation time="3000" count="63" path="http://static.52xiaoluo.com/1111animate_c.png"></div>
```
### css
```
.png_animation {
    width: 5.333rem;
    height: 5.067rem;
    background-size: 6400% 100%;
    background-repeat: no-repeat;
    margin: auto;
}
```
