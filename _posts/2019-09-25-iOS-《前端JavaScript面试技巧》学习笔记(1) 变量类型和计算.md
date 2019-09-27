---
layout:     post   				    # 使用的布局（不需要改）
title:      变量类型和计算 				# 标题 
subtitle:   《前端JavaScript面试技巧》学习笔记(1) #副标题
date:       2019-09-27 				# 时间
author:     BY 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - JavaScript面试
---


## 问题
> 1.JS中使用 typeof 能得到哪些类型
>
> 2.何时使用 '==='，何时使用 '=='
>
> 3.JS有哪些内置函数
>
> 4.JS变量按照存储方式分为哪些类型，并描述其特点
>
> 5.如何理解 JSON

## 知识点

### 1: 数据类型
#### 值类型: 5种 string,number,bolean,null,undefind
```
//值类型
var a = 100
var b = a
a = 200
console.log(b) //100
```
#### 引用类型: 对象, 数组, 函数
```
//引用类型
var a = {age:20}
var b = a
b.age = 21
console.log(a.age) //21
```
特点:引用类型可以无限制的扩展属性,在引用类型中变量是指针,指向这个对象,所以可以减少内存的占用;
由于引用类型占用内存空间较大，所以会出现公用内存空间的情况，变量a,b是指向某个内存空间的指针。而var b=a时，其实是b又指向了与a相同的内存空间，{age:20}仅存在一份。
### 2: typerof 运算符详解
```
typeof undefined//undefined
typeof 'abc' //string
typeof '123' //number
typeof NaN //number
typeof true //boolean
typeof {} //object
typeof [] //object
typeof null //object
typeof console.log //function
```
typeof 不能详细区分引用类型(对象、数组)的详细类型，但是可以详细区分function以及所有值类型。
### 3: 会发生类型转换的场景
> 字符串拼接
> 
> == 运算符
>
> if 语句
>
> 逻辑运算
*    字符串拼接
```
var a = 100 + 10 // 110
var b= 100 + '10' // '1010'
```
*    == 运算符
```
100 == '100'  // true; 前面一个100 被转换成了字符串
0 == ' ' // true // true
null == undefind // true 
```
**注意**: '==' 需要慎用，因为会试图进行类型转换使前后相等
*    if语句
```
var a=true
if(a){
//....执行
}
var b=100
if(b){
//....执行
}
var c=''
if(c){
//....不执行
}
```
if 语句中为 false 的情况:
```
if(0) {},
if(false) {},
if(NaN) {},
if(undefind) {},
if(" ") {},
if(null) {};
```
*    逻辑运算

```
console.log(10 & 0) // 0  转换为 true && 0
console.log(' ' || 'abc') // abc  转换为 false||'abc'
console.log(!window.abc) // true  !undefined 为 true
```
## 解题
1: JS中使用typeof能得到哪些类型 ?<br>
> string, number, boolean, object, function,undefined

2: 何时使用 == 和 = = = ?
> 当判断对象的属性是否是 null 和 undefind 的时候,使用 == ;其他情况全部用 = = =;
```
    //仅有这种情况使用 '=='
    if (obj.a == null) {
      //此时条件相当于obj.a === null||obj.a === undefined,简写形式
      //这是jQuery源码中推荐的写法
    }
```
3: js中有哪些内置函数 -- 数据封装类对象<br>
> Number
>
> String
>
> Boolean
>
> Object
>
> Array
>
> Function
>
> Date
>
> RegExp
>
> Error

js中内置对象: Math,JSON

4: JS变量按照存储方式分为哪些类型，并描述其特点

> 分为值类型和引用类型

* 值类型 : 将变量值分块儿储存在内存中(给变量赋值后,变量值不会改变)

* 引用类型 : 好几个变量共用一个内存块儿,可以节省内存空间(给变量赋值后,变量值会改变)
```
// 值类型
var a = 100
var b = a
a = 200
console.log(b) //100

// 引用类型
var a = {age:20}
var b = a
b.age = 21
console.log(a.age) //21
```
5: 如何理解JSON?

* JSON只不过是一个 JS对象而已;
* JSON是一种数据格式;
* JSON最常用的两个API(如图):
```
JSON.stringify({ a:10, b:20 }) //将对象转换为字符串
JSON.parse('{ "a":10, "b":20 }') //把字符串转换为对象
```
第一个将字符串变成对象;JSON.stringify({a:10, b:20})

第二个将对象变成字符串:JSON.parse('{"a":"10" , "b":"20"}')
