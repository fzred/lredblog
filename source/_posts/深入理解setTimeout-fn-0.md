title: '深入理解setTimeout(fn,0)'
categories: 前端
date: 2015-11-29 11:29:31
tags: js
---
`setTimeout(fn,0)` 各种奇技淫巧之一，莫名其妙的解决一些问题，这里就不列举了。下面这段代码依次弹出1,3,2。
```javascript
alert(1)
setTimeout(function(){
	alert(2)
},0)
alert(3)
```
理解`setTimeout`之前你需要知道**JavaScript的单线程机制和浏览器的事件模型**
<!--more-->
## js是单线程，但浏览器不是
例如Webkit或是Gecko引擎，都有如下线程：
* javascript引擎线程
* 界面渲染线程
* 浏览器事件触发线程
* Http请求线程

他们之间是如何配合的呢？
### 浏览器UI线程
总的来说，大多数浏览器有一个单独的处理进程，它由两个任务所共享：
JavaScript任务和用户界面更新任务。每个时刻只有其中的一个操作得以执行，也就是说当JavaScript代码运行时用户界面不能对输入产生反应，反之亦然。
或者说，当JavaScript运行时，用户界面就被“锁定”了。管理好JavaScript运行时间对网页应用的性能很重。
```html
<html>
<head>
    <title>浏览器UI线程Example</title>
</head>
<body>
    <button onclick="handleClick()">点击我</button>
    <script type="text/javascript">
        function handleClick() {
            var div = document.createElement("div");
            document.body.appendChild(div);
            div.innerHTML = "Clicked!";
        }
    </script>
</body>
</html>
```
上面这例子，当点击button时，会增加两个任务:
1. 更新button样式（按下状态）
2. *执行handleClick()*

在`handleClick`执行过程中又创建了个 div ，会增加一个UI更新的任务。
流程如下图，可以看到，UI线程同一时间只能处理一个任务。
![](/imgs/20151129150856.jpg)

把代码改下，如下handleClick这任务死循环了，页面出现假死状态，导致无法执行接下来的任务，UI不会更新，新添加的 div 也不会出现。
```javascript
function handleClick() {
    var div = document.createElement("div");
    document.body.appendChild(div);
    div.innerHTML = "Clicked!";
    while (true) {
    }
}
```

### setTimeout
继续来说 `setTimeout` ,调用 `setTimeout()` 告诉JavaScript引擎等待一定时间然后将JavaScript任务添加到**UI任务队列**中。
这很好的解释了我们一开始那段代码，`setTimeout` 会创建一个新的任务，就算参数是 **0** 也不是在当前任务执行。
然而实际情况是参数是 **0** 也无法准确到立刻加载任务队列，JavaScript定时器延时往往不准确，快慢大约几毫秒。并不意味任务将在调用setTimeout()之后精确的指定时间后加入队列。所有浏览器试图尽可能准确，但通常会发生几毫秒滑移，或快或慢。正因为这个原因，定时器不可用于测量实际时间。
HTML5规范中要求setTimeout精确问题不超过**4ms**

### 如果JavaScript是多线程
如果js是多线程的方式来操作UIDOM，则可能出现UI操作的冲突；在多线程的交互下，处于UI中的DOM节点就可能成为一个临界资源。假设存在两个线程同时操作一个DOM，而线程1要求浏览器删除DOM节点，线程2却希望修改这个节点的某些样式风格。这个时候浏览器就无法裁决采用哪一种策略了。
搞WinForm的同学可能会说：**我们可以引入“锁”来解决这些冲突。**但为了避免引入了更大的复杂性，所以JavaScript从诞生开始就选择了单线程执行。
