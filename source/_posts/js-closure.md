---
title: javascript中的闭包
date: 2017-07-12
categories: 编程语言
tags: [javascript,js,closure]
---
理解闭包，关键在于理解作用域。

<!--more-->

闭包是一种使用方式。
 令一个函数（函数a）的返回值是一个函数（函数b），这个返回值函数（函数b）被调用时，函数内部的变量仍维持原有的作用域链。这就形成了闭包。

## 作用域链

每一段JS代码都有一个与之关联的作用域链，链上元素是一个个对象（object），每个对象定义了这段代码作用域的变量。当JS需要变量X的值时，会从最内层对象开始找，直到找到。如果找不到则抛出错误。

变量的作用域链在定义的时候就决定了。

我觉得理解成圆环套圆环会更为形象一些，在圈子里找不到就到外一层圈子找。每层函数都是一个圆环，每层字典对象也是一个圆环。

变量的作用域链在定义的时候就决定了。有这个概念能理解很多代码。

## 判断作用域

```
var name = 'global';
function showName(){
    var name = 'local';
    var f = function(){
        return name;
    }
    return f;
}
var f = showName(); f();//showName()()
```

&nbsp;f是返回值函数，f被调用时，内部变量仍维持定义时的作用域。f被定义时，内部变量name来自showName函数，此时name的值就是local。因此f的返回值是local。

## 循环绑定事件

```
for(var i=0; i<3; i++){
    $('#btn'+i).click(function(){alert(i)});
}
```

理想的结果是三个button分别alert 1,2,3。事实上却是alert 3,3,3

因为绑定到click事件的回调函数中，变量i的作用域链在定义的时候已经决定了，他是在最外层定义的，是全局变量。
所以click事件调用回调函数时，i还是全局变量，也就是for循环执行结束后i的值：3。

如果改为

```
for(var i=0; i<3; i++){
    (function(i){
        $('#id'+i).click(function(){alert(i)});
    })(i)
}
```

这样才会alert 1,2,3

绑定到click事件的回调函数中，i是传进来的参数。当外部执行完，留在内存中的回调函数中i就是当时传进来的值。所以回调函数执行能达到理想效果。
