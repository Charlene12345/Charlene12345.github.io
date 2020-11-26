---
layout:     post   				    # 使用的布局（不需要改）
title:      前端资料 				# 标题 
subtitle:   《babel-polyfill》 #副标题
date:       2019-10-07 				# 时间
author:     Charlene 						# 作者
header-img: img/js1.jpg 	#这篇文章标题背景图片
catalog: false 						# 是否归档
tags:								#标签
    - babel-polyfill
---
### babel-polyfill的引用和使用

在做vue去哪儿项目开发、联调时，在手机上可能显示不出来，因为一些低版本的浏览器对es6新语法并不支持，需要添加babel-polyfill。

polyfill翻译为：腻子脚本。具体是指一段可以给老版本浏览器（ie9以前的版本）带来新特性的javascript脚本代码。如轻量级的脚本代码或Modernizr,Modernizr除了能让ie支持html5新元素之外，还能够基于一系列特新测试来有条件的加载更高级的腻子脚本（polyfill）、css文件以及额外的javascript文件。

Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而可以在现有环境执行，所以我们可以用ES6编写，而不用考虑环境支持的问题；<br>
有些浏览器版本的发布早于ES6的定稿和发布，因此如果在编程中使用了ES6的新特性，而浏览器没有更新版本，或者新版本中没有对ES6的特性进行兼容，那么浏览器就会无法识别ES6代码，例如IE9根本看不懂代码写的let和const是什么东西？只能选择报错，这就是浏览器对ES6的兼容性问题；<br>
Babel默认只转换新的JavaScript语法（syntax），如箭头函数等，而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码；因此我们需要引入babel-polyfill来模拟实现这些对象、方法；<br>
因为这是一个 polyfill （它需要在源代码之前运行），我们需要让它成为一个 dependency（上线时的依赖）,而不是一个 devDependency（开发时的依赖）；

安装命令如下。

`$ npm install --save babel-polyfill`

然后，在脚本头部，加入如下一行代码。

```
import 'babel-polyfill';
// 或者
require('babel-polyfill');
```