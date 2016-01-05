title: html5版的坦克大战
categories: 前端
date: 2015-12-20 18:00:04
tags: [typescript,游戏]
---
十月份国庆徒步回来后没啥事，就花了几天把14年写的坦克大战重新写一篇，之前是用原生javascript的，那代码简直不能看。刚好那段时间在折腾typescript，so..改用ts写。
先放上git地址 **[https://github.com/fzred/Battle-City](https://github.com/fzred/Battle-City)**
<!--more-->
简单说下重要的几个类
* [spirit](#spirit_精灵)
* [playing](#playing_游戏场景)
* [imgSource](#imgSource_资源管理)

## spirit 精灵
每个游戏引擎都有的概念，描述精灵的外观及动作。
tank（坦克）、missile（子弹）、terrain（地形，障碍物）类继承自 spirit 。
[源码 https://github.com/fzred/Battle-City/blob/master/tsjs/spirit.ts](https://github.com/fzred/Battle-City/blob/master/tsjs/spirit.ts)


## playing 游戏场景
管理游戏生命周期、事件、spirit、游戏绘制，对象碰撞检测。
[源码 https://github.com/fzred/Battle-City/blob/master/tsjs/playing.ts](https://github.com/fzred/Battle-City/blob/master/tsjs/playing.ts)

## imgSource 资源管理
管理游戏中用的资源，一般在游戏开始前加载。
游戏中只用到了图片资源，所以只有load.image
[源码 https://github.com/fzred/Battle-City/blob/master/tsjs/loadresource.ts](https://github.com/fzred/Battle-City/blob/master/tsjs/loadresource.ts)





![](/imgs/20151215164203.png)