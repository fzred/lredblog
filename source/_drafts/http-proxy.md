title: 前端与后台接口跨域联调
categories: 笔记
date: 2016-09-05 20:55:47
tags:
---
现在业内前后端分离基本是共识，同时也有不少人对于分离后如何调用后台接口时都有不少疑问。
开发时，后台接口与前端的html页面一般是分开的，不在同一个域名下，如果直接访问接口一般会存在跨域问题。这里总结下常用的几种方式。

## 1. 反向代理
反向代理的方式很多，常用的有通过nginx，apache实现的，也有自己写代理服务的。

### Fiddler
没错，我常常用fiddler来调试接口，可能很多同学用这工具只是抓包用，没注意到`AutoResponder`这个功能。
![](http://ww3.sinaimg.cn/large/005FY9HCgw1f7j1mt95pbj30f704iwfg.jpg)
**举个例子**
后台接口地址：http://api.douban.com/v2/movie/top250
本地环境：http://127.0.0.1:1045/index.html
通过ajax去访问，很明显的404。
```
$.get('/v2/movie/top250', function (result) {
    console.log(result)
})
```
![](http://ww4.sinaimg.cn/large/005FY9HCgw1f7j234ltlgj30ex01at8r.jpg)
现在借助Fiddler实现代理
![](http://ww3.sinaimg.cn/large/005FY9HCgw1f7j24vrej0j30gh08ijtw.jpg)
现在再来访问`http://127.0.0.1:1045/v2/movie/top250`已经能拿接口的数据了。
`AutoResponder`的Rule也支持正则匹配。
比如希望将
`http://127.0.0.1:1045/v2/`
统一代理到
`http://api.douban.com/v2/`
可以这么配置
![](http://ww4.sinaimg.cn/large/005FY9HCgw1f7j2eev8naj30gf041aaw.jpg)
!注意，Fiddler的代理有点不完美，无法代理更改后台Response头信息中Set-Cookie中指定的域名。导致的问题是后台无法给浏览器设置cookie。
Fiddler就说到这里，更详细的可以看Fiddler的文档[AutoResponder](http://docs.telerik.com/fiddler/KnowledgeBase/AutoResponder)

### Nodejs
借助`node-http-proxy`这个库来实现反向代理。
```javascript
const http = require('http')
const express = require('express')
const httpProxy = require('http-proxy')

const proxy = httpProxy.createProxyServer({
    target: 'http://api.douban.com/',
    changeOrigin: true,
})
const app = express()

app.all('/v2/*', function (req, res) {
    proxy.web(req, res)
})

http.createServer(app).listen('1003', function () {
    console.log('启动服务器完成，访问试试：http://127.0.0.1:1003/v2/movie/top250')
})
```
`http-proxy`的详细配置看这里[https://github.com/nodejitsu/node-http-proxy](https://github.com/nodejitsu/node-http-proxy)


-----
## 2. Access-Control-Allow-Origin
需要后台返回的Response头信息中需要包含*Access-Control-Allow-Origin*，值一般为Request的`Origin`（即为前端页面的域名）。
```text
Access-Control-Allow-Origin: http://127.0.0.1
```
也可以直接设置为 *** 。
```text
Access-Control-Allow-Origin: *
```
一般还有个可选字段 `Access-Control-Allow-Credentials`。它的值是一个布尔值，表示是否允许发送Cookie。默认情况下，Cookie不包括在跨域请求之中。设为true，即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。这个值也只能设为true，如果服务器不要浏览器发送Cookie，删除该字段即可。
另一方面，前端也需要在AJAX请求中设置 `withCredentials` 属性。
```
var xhr = new XMLHttpRequest();
xhr.withCredentials = true
```