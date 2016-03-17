title: webpack-dev-server结合后端服务器
categories: 笔记
date: 2016-03-17 16:05:37
tags: [webpack,nodejs]
---
<!--摘要-->
<!--more-->

>webpack-dev-server是一个小型的node.js Express服务器,它使用webpack-dev-middleware中间件来为通过webpack打包生成的资源文件提供Web服务。它还有一个通过Socket.IO连接着webpack-dev-server服务器的小型运行时程序。webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。

Webpack及webpack-dev-server的简单使用就不说了。这里介绍当HTML不是由 **webpack-dev-server** 输出时，要怎么与 **webpack-dev-server** 配合

### 例子
开发时启用了 **127.0.0.1:9000** 的服务器，现在加入了webpack，并且希望能够支持热更新（Hot Module Replacement），而 **webpack-dev-server** 就提供了热更新的功能。
>Hot Module Replacement，即模块热替换 HMR，在前端代码变动的时候无需整个刷新页面，只把变化的部分替换掉。

 **webpack-dev-server** 支持两种方式启动
* 命令行
* nodejs API

这里只说nodejs API的方式，代码多点，但更灵活。
