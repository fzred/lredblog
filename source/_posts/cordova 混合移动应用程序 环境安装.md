title: cordova 混合移动应用程序 环境安装
date: 2015-10-19 17:08:51
categories: 前端
tags: [cordova,js]

---
### 什么是混合移动应用程序?

混合应用程序只是一个普通的移动优化的网站，用CSS，HTML和JavaScript编写，在webview上显示（它基本上是一个精简的Web浏览器），必要时需要由native提供原生的接口。在大多数情况下，不用修改就可以在 Android，iOS 和 Windows Phone 上运行。
关于[混合移动应用开发的各种技术](http://www.oschina.net/translate/comparing-the-top-frameworks-for-building-hybrid-mobile-apps-1)
<!--more-->

## [Cordova/PhoneGap](http://cordova.apache.org/)

相对于其他解决方案，PhoneGap要更成熟，拥有丰富的插件。

## [Ionic](http://www.ionic.wang/)

HTML5 手机应用开发框架，配合PhoneGap开发混合移动应用前端框架（不是必须），完美的融合了AngularJS。

## 开始

* 安装JDK
* 安装ANT
* 安装Android SDK
* 运行 Android SDK Manager，下载特定 API 版本的库及工具
		
```bash
npm install -g cordova ionic
ionic start myApp
cd myApp
ionic platform add android
ionic build android
ionic emulate android
```

## 调试

* [Ripple Emulator](http://incubator.apache.org/projects/ripple.html)
基于 Google Chrome 的移动设备模拟器
		
```bash
npm install -g ripple-emulator
cordova platform add android
ripple emulate
```

* [Weinre](http://www.tuicool.com/articles/mAzmq2) 
Web Inspector Remote、是基于WebKit（比如Chrome、Safari）的调试工具。 

* [GapDebug](http://www.raymondcamden.com/2014/7/2/GapDebug-a-new-mobile-debugging-tool)


类似于Weinre

* [PhoneGap Developer App](http://app.phonegap.com )
不需要编译就能在真机上测试应用。通过phonegap serve指令起一个服务器，通过WiFi与一台移动设备上的PhoneGap配对。这台服务器监控代码的变动，并把它们自动地发送到那台设备上，而不用执行本地编译。 

* Android AVD & 真机调试
模拟器太卡了，简直没法用了，还是用真机靠谱

* Visual Studio & WebStorm
对Corodva有很好的支持，集成Ripple,模拟器，**推荐**

* [Fiddler](http://www.cnblogs.com/tugenhua0707/p/4623317.html)
抓包代理，Web开发必备了



## 总结

以上，环境的搭建比较折腾，当碰到网络问题时，可能只是需要科学上网。需要会的东西大概有gulp,sass,ionic,angularjs。想要让应用快速跑起来，Visual Studio 是最佳的开发工具。