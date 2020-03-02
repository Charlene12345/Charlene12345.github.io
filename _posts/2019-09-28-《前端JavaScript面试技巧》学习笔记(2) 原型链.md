---
layout:     post   				    # 使用的布局（不需要改）
title:      原型链 				# 标题 
subtitle:   《前端JavaScript面试技巧》学习笔记(2) #副标题
date:       2019-09-28 				# 时间
author:     Charlene 						# 作者
header-img: img/js1.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - JavaScript面试
---
## 问题
1.如何准确判断一个变量是数组类型<br>
2.写一个原型链继承的例子<br>
3.描述new一个对象的过程

## 知识点

### 构造函数

```
function Foo (name,age) {
  this.name=name
  this.age=age
  this.class='class-1'
//return this  //默认有这一行
}
var f = new Foo('zhangsan',20)
//var f1=new Foo('lisi',22) //创建多个对象
``

构造函数的一个基本特征,函数名首字母大写;<br>
通过new可以实例出很多的构造函数出来;构造函数类似于一个模板的机制

### 构造函数-扩展

```
var a = {} // 把a赋值成对象,其实是 var a=new Object()的语法糖
var b = [] // 其实是var b=new Array()的语法糖
function Foo () {...} // 其实是var Foo=new Function(...)
// 使用instanceof判断一个函数是否是一个变量的构造函数
//如: 变量 instanceof Array();
```

* **instanceof** 用于判断引用类型属于哪个构造函数的方法<br>
所有的引用类型（对象、数组、函数）都有构造函数，a的构造函数是Object()，b的构造函数是Array()，Foo的构造函数是Function()。所以假如想要判断一个变量是否为数组就可以使用

### 原型的 5个规则和实例

1: 所有的引用类型(数组,对象,函数),都具有对象的特征,即可**自由扩展属性**( ' null ' 除外)

```
var obj = {}; obj.a = 1000;
var arr = []; arr.a = 1001;
function fn () {};
fn.a = 1002;
```

2: 所有的引用类型(对象,数组,函数),都有一个____proto____(隐式原型)属性,属性值是一个普通的对象

```
var obj = {}; obj.a = 1000;
var arr = []; arr.a = 1001;
function fn () {};
fn.a = 1002;

console.log(obj.__proto__); // {}
console.log(arr.__proto__); // []
console.log(fn.__proto__); // Function
```

3: 所有的函数(不包括数组,对象),都有一个 prototype (显示原型)属性,属性值也是一个普通对象

```
function fn () {};
fn.a = 1002;

console.log(fn.prototype); // fn {}
```

4: 所有的引用类型(对象,数组,函数)的____proto____属性值指向他的构造函数的 "prototype" 属性值;指向:即完全等于

```
var obj = {};  // 等于 var obj = new Object();
obj.a = 1000;
console.log(obj.__proto__ === Object.prototype); //true;  
//obj的构造函数就是Object; 
//Object是一个函数,所以可以有显示原型的属性
```

5: 当试图得到一个对象(引用类型)的某个属性时,如果这个对象本没有这个属性,那么会去它的____proto____( 即它的构造函数的prototype )中寻找

```
var obj={};obj.a=100;
var arr=[];arr.a=100;
function fn(){}
fn.a=100

console.log(obj.__proto__) //{}
console.log(arr.__proto__) // []
console.log(fn.__proto__) // Function

console.log(fn.prototype) // fn {}

console.log(obj.__proto__===Object.prototype)     //true
```

```
//构造函数
function Foo(name, age) {
    this.name = name;
};

Foo.prototype.alertName = function(){
    alert(this.name)
};
//实例
var f = new Foo("jiangdeng");
f.printName = function () {
    console.log(this.name)
};
//测试
f.printName(); // jiangdeng
f.alertName(); // jiangdeng
```

### 补充两个知识点:

**this**<br>

通过对象属性的形式执行函数的时候(如下),无论函数是自身的属性还是从原型中得到的属性,这个函数执行的时候这个this永远指向这个对象(f)自身

```
f.printName(this); //jiangdeng
f.alertName(this); //jiangdeng
```

**循环对象的自身属性**

```
//构造函数
function Foo(name, age) {
    this.name = name;
};

Foo.prototype.alertName = function(){
    alert(this.name)
};
//实例
var f = new Foo("jiangdeng");
f.printName = function () {
    console.log(this.name)
};
//测试
f.printName(); // jiangdeng
f.alertName(); // jiangdeng
//查找自身属性的方法
var item;
for (item in f) {
    if (f.hasOwnProperty(item)) {
        console.log(item)
    }
}
//   f 自身属性加上原型上的属性一共有三个,分别是:name,alertName,printName;
// 自身属性有name 和 printName
```

### 原型链

```
//构造函数
function Foo(name, age) {
    this.name = name;
};

Foo.prototype.alertName = function(){
    alert(this.name)
};
//实例
var f = new Foo("jiangdeng");
f.printName = function () {
    console.log(this.name)
};
//测试
f.printName(); // jiangdeng
f.alertName(); // jiangdeng
f.toString()    //要去f.__proto__.__proto__中查找
```

所有的引用类型都有`__proto__`属性，且`__proto__`属性值指向它的构造函数的 prototype 的属性值，所以当 f 不存在 toString 时，便会在`f.__proto__`即Foo.prototype中查询，而Foo.prototype中也没有找到toString。由于Foo.prototype也是一个对象，所以它隐式原型`__proto__`的属性值便指向它的构造函数Object的prototype的属性值。

![原型链](https://user-gold-cdn.xitu.io/2019/9/25/16d6641b69e9dffd?w=1064&h=465&f=png&s=112872)

```
//试一试
console.log(Object.prototype) 
console.log(Object.prototype.__proto__)    //为了避免死循环，所以此处输出null
```

 **instanceof:**

f instanceof Foo的判断逻辑:<br>
f的__proto__一层一层往上，能否对应到Foo.prototype<br>
再试着判断f instanceof Object<br>
f 的隐式类型一层一层网上找,看能否对应到Foo的显示类型,如果没有找到,再试着去找到Object的显示类型

## 解题

1.如何准确判断一个变量是数组类型<br>
使用instanceof

```
var arr=[]
arr instanceof Array    //true
typeof arr    //object    typeof无法准确判断是否是数组
```

2.写一个原型链继承的例子

```
//首先定义一个构造函数Animal 
//动物
function Animal() { 
    this.eat = function() {
        console.log('animal eat')
    }
}
//狗
function Dog() {
    this.bark = function() {
        console.log('dog bark')
    }
}
//Dog构造函数的的显示类型指向 Animal的对象,Animal对象有一个属性eat
Dog.prototype = new Animal();

//哈士奇
var hashiqi = new Dog();
hashiqi.eat;
?:哈士奇有什么属性?Dog本身有bark属性,hashiqi肯定有bark属性;hashiqi.eat能执行,因为他会从Dog的显示类型中去取;Dog的显示类型就是一个new Animal,已经有了eat属性的这么一个对象
```

**注意**: 面试的时候千万不要写这个例子,因为会显得特备low;

**高级例子**

```
//原型继承的实例,封装一个DOM节点查询的例子,然后获取让他的内容,改变内容,绑定节点
function Elem(id) {
    this.element = document.getElementById(id);
    return this;
}
//扩展原型,在原型上添加一个html的方法,该方法接受一个值val;
Elem.prototype.html = function(val) {
    //获取到上面那块儿,然后把它放到变量(ele)里;
    var ele = this.element;
    //如果获取的元素ele有内容,就赋值成val;如果没值的话,就打印出标签dom结构
    if (val) {
        ele.innerHTML = val;
        return this //为了链式操作
    } else {
        return ele.innerHTML;
    }
}

//扩展一个方法on
Elem.prototype.on = function(type, fn) {
    var ele = this.element;
    ele.addEventListener(type, fn)
}

var div1 = new Elem('mobile-bar');
//打印目录
//console.log(div1.html());
//修改内容
div1.html('我我想做成一件事,哪怕只是一件');
//添加事件
div1.on('click', function() {
    alert('fuck');
})
```

3.描述new一个对象的过程

```
function Foo (name,age) {
 // this = {}
    this.name = name
    this.age = age
    this.class = 'class-1'
  //return this
}
var f = new Foo ('zhangsan',20)
//var f1 = new Foo ('lisi',22) //创建多个对象
```

创建一个新对象<br>
this指向这个新对象 this={}<br>
执行代码，即对this赋值 this.xxx=xxx<br>
返回this return this

版本2:<br>
new一个构造函数返回一个对象的过程:

```
function Foo(name,age) {
   this.name = {}; // this 变成一个空对象
   return this;
}
var f = new Foo('jiangdeng'); 
```

首先new的时候把参数传进去(当然也可以不传),new函数执行的时候构造函数里面的this会变成一个空对象,然后是this.name,this.age开始赋值,赋值完成后,默认把this给return回来;return回来后再赋值给f,f 现在具备了this.name等于'zhangsan'属性
## 资源

https://www.cnblogs.com/wangfupeng1988/p/3977924.html