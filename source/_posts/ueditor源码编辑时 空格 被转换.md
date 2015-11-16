title: ueditor源码编辑时 空格 被转换成 &nbsp;
date: 2015-9-15 18:05:34
categories: 笔记
tags: [js,移动端]
---
我使用的是 1.3.6 的版本


我觉得源码编辑下已经是pre模式了，就不应该把空格给转义掉.


虽说ueditor很人性化,但这点没考虑到.


解决如下:


ueditor.all.js  8257行  isText方法修改为

```javascript
function isText(node, arr) {       
 arr.push(node.data)     
}
```