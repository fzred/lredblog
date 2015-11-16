title: angularjs控制器之间通信，事件通知服务
date: 2015-5-15 23:32:36
categories: 前端
tags: [angularjs]
---
<!--22-->
<!--more-->

最近在使用angular时，发现控制器间的通信还是挺常见的，然后总结写了个简单的service，事件通知服务。

关于angular里service的概念，这里就不分享了，可以看下这篇文章： Angular.js Services

service要记住一点就是所有的services都是singleton(单例)的，service更多的是做一些业务逻辑，数据交互。当然，利用单例这特点也可以用来做不同控制器间的通信。控制器间的通信也有多种做法：AngularJS控制器controller如何通信？。

利用作用域继承的方式。即子控制器继承父控制器中的内容

基于事件的方式。即$on,$emit,$boardcast这三种方式

服务方式。写一个服务的单例然后通过注入来使用

第一种还是有些局限性，第二种用起来并不太方便（或者个人不习惯），于是利用service实现了基于事件的通知方式。

### 上代码
```javascript
//提供给不同控制器的通信
app.factory("EventService", function () {
    var onEventFunc = {};
    return {
        on: function (type, f) {
            //事件绑定
            onEventFunc[type] = f;
        }, trigger: function (type, data) {
            //触发事件
            for (var item in onEventFunc) {
                if (item == type)
                    onEventFunc[item](data);
            }
        }
    }
});
 
//使用
function test1Controller(EventService) {
    EventService.on("newMess", function (data) {
        //console.log(data);
    });
}
function test2Controller($scope,EventService) {
    $scope.send = function (data) {
        //这里是我在项目中使用的一个例子，当发送了新消息，通知另一个控制器
 
        //触发事件，通知
        EventService.trigger("newMess", data);
    }
}
```

可以基于这思路去扩展实现更复杂的业务。找个时间分享下利用这思路实现的webscoket事件分发。


时候不早，洗澡去，明天又要上班，最近忙成dog。


![](/imgs/1431267587649000000.png)