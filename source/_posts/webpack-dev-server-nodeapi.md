title: webpack-dev-server 结合后端服务器 nodejs API
categories: 笔记
date: 2016-03-17 16:05:37
tags: [webpack,nodejs]
---
<!--摘要-->
<!--more-->

>webpack-dev-server是一个小型的node.js Express服务器,它使用webpack-dev-middleware中间件来为通过webpack打包生成的资源文件提供Web服务。它还有一个通过Socket.IO连接着webpack-dev-server服务器的小型运行时程序。webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。

Webpack及webpack-dev-server的简单使用就不说了。这里介绍当HTML不是由 **webpack-dev-server** 输出时，要怎么与 **webpack-dev-server** 配合

### 例子
开发时启用了java的服务器，没有使用webpack-dev-server内置的http服务，现在加入了webpack，并且希望能够支持热更新（Hot Module Replacement），而 **webpack-dev-server** 就提供了热更新的功能。
>Hot Module Replacement，即模块热替换 HMR，在前端代码变动的时候无需整个刷新页面，只把变化的部分替换掉。

 **webpack-dev-server** 支持两种方式启动
* 命令行
* nodejs API

这里只说nodejs API的方式，相对于命令行更灵活，当然代码也要多写点。


main.js
```javascript
document.write('hello')
require('./index.css')
```
index.css
```css
html {
    background: #fff;
    font-size: 100px;
}
```
webpack.config.js
```javascript
var path       = require("path");
module.exports = {
    entry : {
        main: './src/main.js'
    },
    output: {
        path      : path.resolve(__dirname, 'static'),
        filename  : '[name].js?[hash]',
        publicPath: '/static/'
    },
    module: {
        loaders: [
            {test: /\.css$/, loader: 'style!css'},
        ]
    }
}
```
上面这代码应该是webpack最简单的例子了。借助 webpack-dev-server 命令行工具也可以很简单的启动HMR服务
```bash
webpack-dev-server --hot --inline
```

---

现在深入学习下 webpack-dev-server ，使用 webpack-dev-server 的nodejs api 实现HMR


entry 需要改成
```javascript
['webpack-dev-server/client?http://127.0.0.1:8080/','webpack/hot/dev-server','./src/main.js']
```
添加 plugins
```javascript
new webpack.HotModuleReplacementPlugin()
```

完整的代码 hot-server.js
```javascript
var WebpackDevServer = require("webpack-dev-server")
var _                = require('underscore-contrib')
var config           = require('./webpack.config')
var webpack          = require('webpack')

_.map(config.entry, function (value, key) {
    config.entry[key] = [
        'webpack-dev-server/client?http://127.0.0.1:8080/',
        'webpack/hot/dev-server',
        value
    ];
})
config.output.publicPath = 'http://localhost:8080/static/'

config.plugins = (config.plugins || []).concat([
    new webpack.HotModuleReplacementPlugin(),
])

var compiler = webpack(config)

var server = new WebpackDevServer(compiler, {
    hot       : true,
    noInfo    : true,
    filename  : config.output.filename,
    publicPath: config.output.publicPath,
    stats     : {colors: true},
})
server.listen(8080, "127.0.0.1", function () {
    console.log('Listening at http://127.0.0.1:8080')
})
```
更多参数配置看这里
[http://webpack.github.io/docs/webpack-dev-middleware.html](http://webpack.github.io/docs/webpack-dev-middleware.html)

然后在后台服务器输出的html上加上script
```html
<script src="http://127.0.0.1:8080/static/main.js"></script>
```

---

[查看DEMO](https://github.com/fzred/webpack-demo/tree/master/webpack-dev-server)

