---
layout:     post   				    # 使用的布局（不需要改）
title:      原型链 				# 标题 
subtitle:   《前端JavaScript面试技巧》学习笔记(2) #副标题
date:       2019-09-29 				# 时间
author:     Charlene 						# 作者
header-img: img/js1.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - JavaScript面试
---
## 问题:
1: 说一下对变量提升的理解<br>
2: 说明 this 的几种不同使用场景<br>
3: 创建 10 个 < a > 标签,点击的时候弹出响应的序列号<br>
4: 如何理解作用域<br>
5: 实际开发中闭包的应用<br>

## 知识点

### 执行上下文

范围: 一段< script >或者一个函数中<br>
全局: 变量定义, 函数声明<br>
函数: 变量定义, 函数声明, this, arguments

PS:注意函数声明和函数表达式的区别

```
<script>
console.log(a); // undefind
var a = 100; 

fn('jiangdeng'); //jiangdeng 20
function fn(name) {
    // var age = undefind;
    age = 20;
    console.log(name,age);
    var age; //会被提到此函数内的顶部
}
<script>
```

解析 : 一个 < script > 标签内的所有代码,在一个< script >中定义一个全局的上下文;在执行第一行之前,首先会把所有的变量声明,函数声明拿出来;先把 var a 拿到上面来,这个时候还没给它赋值成100,先用undefind来代替一下;然后第五行有一个函数声明,把整个函数拿到上面来; 在执行这块的时候先做了两件事,把 a 的声明拿上来 并通过undefined赋值来占位,然后把函数拿到上面来;
**注意**: 写代码的时候要先声明,然后执行

### 函数声明方式

```
fn(); //可以执行;执行的时候会把声明的函数提到前面去
function fn(){
    //函数声明
    console.log('函数声明的方式,函数可以前置执行')
}

fn1(); // Uncaught TypeError: fn1 is not a function
//不能执行,走到 var fn1的时候fn1不是一个函数,这个时候fn1提到前面去就是 var fn1 = undefind;
var fn1 = function(){
    //函数表达式
    console.log('函数表达式的方式,函数不能前置执行')
}
```
```
console.log(a);
var a = 100; //会报错,原因同上
//相当于 var a = undefind;
//然后执行console.log(a)的时候 a 等于 undefind;>报错
//最后 a = 100;
```
```
// 用函数声明的方式相当于把fn拿到前面去了;
fn('jiangdeng'); //jiangdeng
function fn(name,age) {
    console.log(name,age);
}

```
```
fn('jiangdeng'); //jiangdeng 20
function fn(name) {
    age = 20;
    console.log(name,age);
    var age;
}
```
* 在函数里面也是一样的,在执行之前,后面的 var age 会被提前到前面去;
### this
* this 要在执行时才能确认值,定义时无法确认
```
var a = {
    name:'A',
    fn:function () {
        console.log(this.name)
    }
}
a.fn();//  "A"; this === a
a.fn.call({name:'B'}) //  "B"; this === {name:"B"}
var fn1 = a.fn;
fn1(); //  undefind; this === window;作为普通函数执行 这个时候的 this 是 window
```
#### 回答问题:说明 this 的几种不同使用场景?
* 作为构造函数执行
* 作为对象属性执行
* 作为普通函数执行 这个时候的 this 是 window
* call apply bind
#### 代码演示
* 构造函数中的this
```
function Foo(name,age) {
    this.name = name;
    this.age = age;
    return this;
}
var f = new Foo('jiangdeng',22);  
```
* 对象属性中的this
```
var obj = {
    name : 'A',
    printName : function(){
        console.log(this.name) 
    }
}
//把函数作为对象属性来执行
obj.printName() // A     this指向obj这个对象
```
* 普通函数
```
function fn() {
    console.log(this) // this === window
}
fn() 
```
* call apply bind
```
function fn(name,age) {
    alert(name);
    console.log(this);
}
var fn1 = fn.call({a:100},'jiangdeng',22); //this === {a:100};
```
* bind ()创建一个函数的实例,其this值会绑定到传给bind()函数的值
```
var fn = function(name,age){
    alert(name);
    console.log(this);  // {a: 100}
}.bind({a:100});
fn('jiangdeng',22)
```
### 作用域
* 没有块级作用域 (大括号是没办法约束里面的变量的)
* 只有函数和全局作用域
```
//没有块级作用域
if (true) {
    var a = 1000; //在括号里面和在括号外面声明变量没有区别
}
console.log(a)
```
```
//函数和全局作用域
var a = 1000;
function fn(){
    var a = 2000;
    console.log(a)
}
console.log(a) // 1000
fn() // 2000
```
#### 作用域链
```
var a = 1000;
function fn(){
    var b = 2000;
    //当前作用域没有定义的变量,即自由变量
    console.log(a)
    console.log(b)
}
fn() //1000,2000
```
**自由变量**:当前作用域没有定义的变量,即自由变量<br>
fn 作用域内没有定义a变量,就去它的父级作用域,即全局作用域去找 a ;<br>
函数的父级作用域是什么? 就是函数在定义的时候父级作用域,不是在执行时候的父级作用域;
```
var a = 1000;
function F1() {
    var b = 2000;
    function F2() {
        var c = 3000;
        console.log(a); //a 是自由变量
        console.log(b); //b 是自由变量
        console.log(c);
    }
    F2()
}
F1() //1000 2000 3000
```
不要管 F1 ,F2 在什么地方执行,要看它们在哪里定义的<br>
**作用域链**:一个自由变量不断的往它父级作用域找,形成一个链式结构
### 闭包
```
//闭包的使用场景：函数作为返回值
function F1() {
    var a = 1000;

    //返回一个函数(函数作为返回值)
    return function() {
        console.log(a) // a 是自由变量,去它的父级作用域寻找
    }
}
//f1 得到一个函数
var f1 = F1(); //F1执行后返回的是一个函数,把它赋值给f1
var a = 2000;
f1(); //1000

//补充一点知识:F1()执行返回的结果是 {console.log(a)},
//为什么不是1000呢?因为F1虽然执行了,但是里面的函数还没有执行,要想得到1000必须还得加一个(),如F1()();

```
* 一个函数的作用域和它的父级作用域在它定义的时候就确定好了,所以确定函数的作用域不看它执行时候的作用域,要看它定义时候的作用域。
#### 闭包的使用场景:
* 函数作为返回值(如上面demo)
* 函数作为参数传递(把函数传到另一个函数中执行),跟上面类似
```
function F1() {
    var a = 100;
    return function() {
        console.log(a)// a 是自由变量,去它的父级作用域寻找
    }
}
var f1 = F1();
function F2(fn) {
    var a = 200;
    //虽然fn()执行的结果(f1传入后)是返回下列注释的函数,但并没有什么卵用,因为函数的作用域不看它执行时候的作用域,看它声明定义时候的作用域
    fn() // >>function () {console.log(a)}
}
F2(f1) // 100
```
## 题目解答:
1: 说一下对变量提升的理解<br>
考察的是对执行上下文的理解,主要内容是:

* 变量定义
* 函数声明 ,函数表达式
* 应用的场景,一是script标签内,一是构造函数中 ,变量的声明和定义都会被提前<br>

2: 说明 this 的几种不同使用场景

* 作为构造函数执行
* 作为对象属性执行
* 作为普通函数执行 这个时候的 this 是 window
* call apply bind
```
  //构造函数
  function Foo(name){
    this.name=name
  }
  var f=new Foo('zhangsan')
  
  //对象属性
  var obj={
    name:'zhangsan',
    printName:function(){
      console.log(this.name)
    }
  }
  obj.printName()
  
  //普通函数
  function fn(){
    console.log(this);
  }
  fn()

  //call apply bind
  function fn1(name,age){
    alert(name)
    console.log(this)
  }
  fn1.call({x:100},'zhangsan',20)

  var fn2=function (name,age){    //bind在函数声明的形式后不可用，必须是函数表达式
    alert(name)
    console.log(this)
  }.bind({y:200})
  fn2('zhangsan',20)
```
3: 创建 10 个 a 标签,点击的时候弹出响应的序列号
```
//这是错误的写法
var i;
for (i = 0; i < 10; i++) {
   var a = document.createElement('a');
    a.innerHTML = i + '<br>';
    a.addEventListener('click', function(e) {
        e.preventDefault();
        alert(i); // i 自由变量,要去父作用域去寻找
    })
    document.body.append(a);
}
```
错误写法的结果是每个a标签上点击的时候,弹出来的都是10<br>
错误的原因: alert(i) 里面的 i 是一个自由变量;会去 function 的父作用域找,这个时候 i 早就成 10了 (本来是9,自增1)
```
//正确写法
var i;
for (i = 0; i < 10; i++) {
    (function(i) {
        var a = document.createElement('a');
        a.innerHTML = i + '<br>';
        a.addEventListener('click', function(e) {
            e.preventDefault(); 
            alert(i);
         })
        document.body.appendChild(a);
    })(i)
}
```
把 i 作为参数传进函数里面,让它作为函数的作用域中的一个变量;当 i 等于0的时候,存储这个变量,然后执行...这里相当于生成了十个函数,每次执行的参数都不同

4: 如何理解作用域<br>
* 自由变量
* 作用域链 , 即自由变量的查找
* 闭包的两个场景

5: 实际开发中闭包的应用

* 闭包实际应用中主要用于封装变量,收敛权限;
* 在下面的例子中,在isFirstLoad函数的中能用到的就只有 return 回的那一部分;而_list在外面是无法修改到的
```
//闭包实际应用中主要用于封装变量,收敛权限
function isFirstLoad() {
    var  _list = [];
    return function (id) {
        if (_list.indexOf(id) >= 0) {
            return false
        }else{
            _list.push(id)
            return true
        }
    }
}
// 使用
var firstLoad = isFirstLoad();
firstLoad(10); //true
firstLoad(10);//false
firstLoad(20);//true
firstLoad(20);//false
//在isFirstLoad函数外，无法修改_list的值
```
## 资源
http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html
<br>
https://www.cnblogs.com/wangfupeng1988/p/3977924.html
