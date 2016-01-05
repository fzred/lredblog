title: a标签的黑魔法
categories: 前端
date: 2016-01-05 16:07:18
tags: [js]
---
<!--摘要-->
<!--more-->
## 问题
在Js中需要根据相对路径拿到绝对路径时，因为网站目录不固定，有本地地址，测试地址，生产地址
```
http://127.0.0.1:8088/index.html
http://test.site.com/test/test/index.html
http://test.site.com/test/test/
http://site.com/test/test/
```
`path+'./static/img.png'`
希望拿到当前目录 + './static/img.png'
所以拼接字符串做起来并不容易，代码容易写得很丑。

## a标签出场
```javascript
function qualifyURL(url) {
	var a = document.createElement('a');
	a.href = url;
	return a.href;
}
qualifyURL('./static/img.png')
```
似乎这样只能拿到当前网站的绝对路径，这时可以配合 [base](http://www.w3school.com.cn/tags/tag_base.asp) 标签
这标签我几乎没用过，[介绍看这里<<](http://www.w3school.com.cn/tags/tag_base.asp)

```javascript
function absolutize(url,base) {
    base=base || location.href;
    var d = document.implementation.createHTMLDocument();  //创建新的DOM
    var b = d.createElement('base');      
    d.head.appendChild(b);      //将base添加到新创建的DOM
    var a = d.createElement('a');
    d.body.appendChild(a);
    b.href = base;          //设置base 规定页面中所有相对链接的基准 URL。
    a.href = url;
    return a.href;
}

absolutize('./static/img.png')
absolutize('./static/img.png',"http://baidu.com")
```

这里也是做下笔记，在碰到这问题之前，我甚至都不知道还有 [document.implementation](https://developer.mozilla.org/en-US/docs/Web/API/DOMImplementation/createHTMLDocument) 这东西

**ps：**
这a标签的用法在这里IE6这老古董有点兼容问题，这里就不阐述了。看这文章 
[Getting an absolute URL from a relative one.](http://stackoverflow.com/questions/470832/getting-an-absolute-url-from-a-relative-one-ie6-issue)