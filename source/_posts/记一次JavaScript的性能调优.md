title: 记一次JavaScript的性能调优
categories: 笔记
date: 2017-08-18 10:43:58
tags:
---
之前写了个JS浮点运算的库 [https://github.com/fzred/calculatorjs/](https://github.com/fzred/calculatorjs/) ，被人吐槽运算速度慢，写的时候确实没关心性能问题。

calculatorjs 运算过程分三步
1. 词法分析
2. 语法分析
3. 数值运算

分析后发现**数值运算**这个阶段性能比我想象中的差得多，所以优先从这个地方下手。

优化前100W次的加法运算 0.22+0.33 耗时2秒，单纯的运算这效率确实太慢了。
```bash
console.time('precisionCalc add')
for (let i = 1; i < 1000000; i++) {
  precisionCalc.add(l, r)
}
console.timeEnd('precisionCalc add')
```
```bash
precisionCalc add: 2006.562ms
```

[优化前代码](https://github.com/fzred/calculatorjs/blob/eabff6822803a26765632b7ad81ba5bd4eb5d967/src/precisionCalc.js)
[优化后代码](https://github.com/fzred/calculatorjs/blob/8e9c5c86016c9f07137efc350459501e43d27128/src/precisionCalc.js)

开始分析代码
优化前代码逻辑里，一次运算会调用4次**split**，用来分割整数跟小数，这个完全可以优化到只调用2次。看一下100W次`'0.22'.split('.')` 需要多少时间 **202.155ms**，一次运算调用4次就是**800ms**左右，太慢。
所以自己实现的分割整数跟小数的split，调整了下逻辑，使其只调用两次。

看26行 `Array(f + 1).join('0')`，作用是给小数点移位是填充0的。测试下 `Array(5).join('0')` 执行100W次平均需要 **280ms**，一次运算会调用两次，这也是可优化的点。所以我干脆直接用十个0的字符串代替 **'0000000000'** ，偷懒了，留下一个隐患，小数超过10位运算就会出现问题。

主要消耗性能的就是这两个地方，还有其他的一些调整就不一一赘述了。[看commit](https://github.com/fzred/calculatorjs/commit/8e9c5c86016c9f07137efc350459501e43d27128#diff-7475cae84adc3e34ada13fffd8556173)

最终优化后跑一开始的测试代码的执行时间
```bash
precisionCalc add: 346.614ms
```
相比一开始的**2006.562ms**，这是个很大的提升。同时也反映了一开始的代码确实写得不好。

来看看big.js的运算速度
```
console.time('big add')
for (let i = 1; i < 1000000; i++) {
 new Big(l).plus(r)
}
console.timeEnd('big add')
```
```bash
big add: 1076.701ms
```

运算上，calculatorjs是要比big.js快3倍的。

词法分析，语法分析比较消耗性能，所以我单独把运算的API开放出来，来应对一些需要高性能的场景。
```
calc.add(0.1, 0.2) // 0.3
calc.sub(0.1, 0.2) // -0.1
calc.mul(0.1, 0.2) // 0.02
calc.div(0.1, 0.2) // 0.5
```
