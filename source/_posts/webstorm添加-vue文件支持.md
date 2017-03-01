title: webstorm添加*.vue文件支持
categories: 笔记
date: 2016-01-07 00:03:37
tags: [vuejs,webstorm]
---
<!--摘要-->
<!--more-->

更新于2017-03-01

这篇文章写了有1年多了，发现这篇文章关注的人不少。现在webpack要支持vue已经有了更方便的方式。避免误导，还是来更新下。

## 方法1：安装 Vue.js 插件
打开 Settings->Plugins 搜索安装

### css预处理器支持
在我写这文章的时间，还没有支持预处理的方案，不过现在已经很简单了。
给 style 标签加上 **rel="stylesheet/scss"** 属性即可支持 **scss** 语法，看规则可以改成 **less** **stylus** 之类的。
```html
<style rel="stylesheet/scss" lang="sass" scoped>
</style>
```

## 方法2：Webstorm EAP 版已经原生支持vue文件
看这里 https://blog.jetbrains.com/webstorm/2017/02/webstorm-2017-1-eap-171-2822/ EAP版可能不太稳定，不过也能用。
或者等过段时间发布的 Webstorm 2017.1 稳定版。
不得不说vue真是越用越厉害了啊，Webstorm也开始支持了。最后 **原生支持才是最好的**

---

# 以下内容过时，针对旧版本的Webstorm

webstorm是前端开发神器，但我一直都不喜欢webstorm，就因为那很挫的配色和那大光标。
上阵子开始玩 Vuejs，在 Vue 中，可以 .vue 文件实现组件化，但各种编辑器都支持不好，作者也给sublime开发了相关的vue插件。我觉得用sublime就是在浪费生命啊，花那么多时间来装插件配环境，我选择IDE！
坚持用sublime写了一个月的vue，没有智能提示（而对重度依赖提示），不能对代码进行格式化，手动调缩进，尼玛，能坚持这么久也不容易。所以折腾了下webstorm看怎么支持，所以就有这篇笔记。

## vue支持
打开 Settings => File Types 找到 HTML 添加 *.vue
![](/imgs/20160107002257.png)
这样vue文件就相当于html文件，可以编辑css，js，也都有智能提示。
我一般用的是 es6 ，所以vue写es6的代码,webstorm还是会报错。

## vue里ES6支持
将script标签添加 **type="es6"** 属性
```html
<script type="es6">
</script>
```
然后打开 Settings => Language Injections 添加 XML Tag Injection，内容如下图。
![](/imgs/20160107003210.png)

## *.js 支持ES6
webstorm默认js文件是ES5语法
打开 Settings => Languages & Frameworks => Javascript
把 Javascript Language version 改为 ECMAScript 6
![](/imgs/20160107004135.png)


PS:要在vue文件里写sass,stylus之类的css预处理，webstorm就不支持了，我也尝试了添加 Injection ，代码高亮正常，但却是临时的，只要一改动代码，又会划很多红线了，经过google，这似乎是webstorm的一个已知的Bug。