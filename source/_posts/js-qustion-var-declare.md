---
title: Javascript变量提前声明的一道面试题
date: 2017-08-20
categories: 编程语言
tags: [javascript,closure]
---
写出下面语句执行结果并描述原理：

```
var a = "global";
!function(){
	console.log(a);
	var a = "local";
}();

!function(a){
	console.log(a);
	var a = "local";
}(a);
```

<!--more-->

这道题涉及到函数变量声明提前和函数作用域。

> 在函数内声明的所有变量（局部变量），在函数体内始终是可见的。

这是JS的函数作用域的特性。这样的特性决定了局部变量在整个函数体内始终要有定义。

因此第一个函数中a作为局部变量，在一开始就有定义，于是就遮盖了同名的全局变量，就好像函数体内将声明提前了一样：

```
(function show(){
	var a;
	console.log(a);
	var a = "local";
})();//所以输出结果是undefined
```

第二个函数中，a作为形参，是现成的局部变量。不管函数体内是否有对a的声明，它都是在函数体内始终有定义的。 因此函数体内的var变量只不过是第二次声明，并不会导致声明提前，所以输出结果是：global

 
