title: 百度地图API 移动端Click失效Bug 解决方法
date: 2015-9-15 18:05:34
categories: 笔记
tags: [js,移动端]
---
部分机型click事件失效，这问题在"API大众版" "API极速版" 都会出现，原因不太清楚，解决办法是用 touchstart touchend 来模拟。
```javascript
/* #region 解决部分机型点击事件失效 */
var timeout = 0, tempPoint;
map.addEventListener("touchstart", function (e) {
    $mapInput.blur();
    timeout = +new Date();
    tempPoint = e.point;
});
map.addEventListener("touchend", function (e) {
    var cur = +new Date();
    if (cur - timeout < 200) {
        selectPoint(tempPoint);
        local.clearResults();
    }
});
/* #endregion*/
```