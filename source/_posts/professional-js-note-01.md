---
title: 《JavaScript高级程序设计》读书笔记-01
date: 2016-03-03
categories: 编程语言
tags: [javascript]
---
前三章知识点整理
<!--more-->
## 数据类型

JavaScript有5个基本数据类型：undefined boolean number string null
和1个复杂数据类型：object

```
undefined == undefined
null == undefined
NaN != NaN
```

Number()函数的转换方式

```
true=>1 false=>0 //boolean 转换
null => 0
undefined=NaN
0x100 => 256 //十六进制=>十进制
Object => valueOf()==NaN ? toString() : valueOf();
```

null和undefined用途完全不同。
null是个空对象指针，对于未赋值的空对象应当设置为null，而不是用缺省值undefined
对于使用完，又有循环引用的对象应当赋值null，保证垃圾收集器能将其释放。
但任何时候都无需将变量设为undefined。

字符串单引号双引号效果一样，一样转义反斜杠（\），一样不解析变量。一样不能换行。

### Object

Object的每个instance有下列属性和方法：

1.  constructor：保留着用于创建当前对象的函数即构造函数；
2.  hasOwnProperty(propertyName)：用于检查给定的属性在当前对象实例中是否存在；
3.  isPrototypeOf(object)：用于检查传入的对象是否是传入对象的原型；
4.  propertyIsEnumerable()：是否可用于for in 循环
5.  toLocaleString()；返回对象的字符串表示，该值与执行环境地区对应
6.  toString()：返回对象的字符串表示；
7.  valueOf()：返回对象的字符串、数值或布尔值表示；

代码表示如下：

```
function test(){this.name="test";}
var x = new test();
function test2(){}
test2.prototype = x;
x.constructor //function test(){this.name="test";}
x.hasOwnProperty('name') //true
x.isPrototypeOf( new test2 ); //true
x.hasOwnProperty('name') //true
x.toLocaleString() //"[object Object]"
x.toString();//"[object Object]"
x.valueOf();//{name: "test"}
```

## switch语句

switch 比较特殊，case语句中甚至可以使用表达式

```
var num = 20;
switch(num){
	case "hello"+"world":
	break;
	case num &lt; 10:
	break;
	case num > 10 &amp;&amp; num &lt;=20:
	break;
	case '20':
		console.log('matched');
	break;
}
//无法输出 matched 因为switch使用全等操作符，不会发生类型转换
```

## 函数

arguments 不是数组，只是能按索引获取参数，但是push join shift等方法不可用

```
(function (x){
   console.log(x); 
   arguments[0]=20;
   console.log(x);
   x=30;
   console.log(arguments[0]);
})(10);//输出 10,20,30
```

可见arguments内的元素和参数是**相互影响的**，**并不是书上说的单向影响**。
如果换成严格模式：

```
(function (x){
	"use strict";
   console.log(x); 
   arguments[0]=20;
   console.log(x);
   x=30;
   console.log(arguments[0]);
})(10);//输出10,10,20
```

可见严格模式下互相不影响。

### 值传参or引用传参

这里书上写了一句：

> ECMAScript中所有参数传递的都是值，不可能通过引用传递参数。

事实上是这样的

```
var a={'name':'Tom'};
-function(y){
	y.name = 'Jim';
}(a);
console.log(a.name); // Jim
//a的属性被改了

-function(y){
	y = {};
}(a);
console.log(a); //Object {name: "Jim"}
//a的值并未改变
```

由此可见JS中，函数传参本身不可被改变，但是如果参数是对象，则其属性可以被更改。

数组也是对象，其属性也可以更改。但是字符串不可被更改

```
var b = [1,2,3];
(function(x){x[0]=5;})(b);
console.log(b); //[5, 2, 3]
var s = "good";
s[0] = 'd';
console.log(s); //good
```

