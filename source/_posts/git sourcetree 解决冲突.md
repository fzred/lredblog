title: git sourcetree 解决冲突
date: 2015-04-16 14:55:48
categories: 笔记
tags: [git,sourcetree]
---
学习git，比较麻烦的问题就是解决冲突了，再高级点就是分支管理。命令行学起来太麻烦，学习成本高，推荐新手用Sourectree来管理git仓库。本文假设你已经会git的基本使用了。在团队开发中，冲突是不可避免的。
<!--more-->

## 冲突的产生

很多命令都可能出现冲突，但从根本上来讲，都是merge 和 patch（应用补丁）时产生冲突。而rebase就是重新设置基准，然后应用补丁的过程，所以也会冲突。git pull会自动merge，repo sync会自动rebase，所以git pull和repo sync也会产生冲突。当然git rebase就更不用说了。

冲突有多种类型，最常碰到的就是*内容冲突*了，两个用户同时修改了同一个文件的同一行（一块区域）的代码，就会产生冲突，接下来介绍的就是如何解决。

## 使用工具

* Beyond Compare (用于编辑冲突)

* SourceTree （管理git仓库）

## 产生冲突

现在服务器文件内容如下
![](/imgs/gitsource/1.png)
本地文件还未pull
![](/imgs/gitsource/2.png)
现在修改本地文件
![](/imgs/gitsource/7.png)
这种情况如果pull合并一定是冲突的。

拉取下来报错了
![](/imgs/gitsource/3.png)
提示"文件1.txt"冲突。


现在推送你的提交，也是报错的，因为冲突未解决.

## 解决办法

在冲突的记录上右键>合并
![](/imgs/gitsource/4.png)
![](/imgs/gitsource/5.png)

查看文件状态，显示有冲突的文件
![](/imgs/gitsource/6.png)
现在看资源管理器，产生了的文件，是合并的辅助文件，合并完如果没自动删除，可以手动删。
![](/imgs/gitsource/8.png)

打开 “文件1.txt” 看到，多了一些像是乱码的东西
![](/imgs/gitsource/9.png)
如果冲突地方不多，可以直接编辑文件到正确的状态。也可以打开外部合并工具（如果有），如果安装了Beyond_Compare，会打开Beyond_Compare来合并。
![](/imgs/gitsource/10.png)

因为我电脑安装了VS,所有默认打开了VS来合并（egg pain，这个怎么改默认设置）。但VS跟Beyond_Compare合并都一样，具体怎么使用琢磨下就会了。


如果没有使用工具，是手动合并，就要告诉SourceTree解决。
![](/imgs/gitsource/11.png)

合并完后直接提交吧
![](/imgs/gitsource/12.png)

osc@git 报500，又挂了，提交不上去，最后还是看不了最终文件。就到这了，休息了。
