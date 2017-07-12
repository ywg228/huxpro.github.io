---
layout:     post
title:      JavaScript预解析、变量提升和函数提升
subtitle:   学习笔记 
date:       2017-07-10
author:     Ywg
header-img: img/home-bg.jpg
catalog:    true
tags: JavaScript
---
## 一、作用域 
只会对某个范围产生作用，而不会对外产生影响的封闭空间。在这样的一些空间里，外部不能访问内部变量，但内部可以访问外部变量。<br>
在ES5中，js只有两种作用域：全局作用域和函数作用域 没有块级作用域！！<br>
块级作用域：任何一对花括号（｛和｝）中的if for while等语句集都属于一个块，在这之中定义的所有变量在代码块外都是不可见的，我们称之为块级作用域。   <br> 
在ES6中，新增了一个块级作用域（最近的大括号涵盖的范围），但是仅限于let声明的变量。

## 二、预解析
在当前作用域中，JS代码执行之前，浏览器解析器首先会默认的把所有带var和function声明的变量进行提前的声明或者定义。

#### 2.1 变量的声明和定义
如果一个变量只声明没定义(有var) 则返回undefined <br>
如果一个变量没有声明(没var)不会预解析 会自动加入到全局作用域中变为全局变量 提前使用未声明变量则返回Uncaught ReferenceError: XX is not defined 会报错！
 
#### 2.2 var声明的变量和function声明的函数在预解析的区别
var声明的变量只是声明提前，而function声明的函数会将声明和定义整个都提前。

#### 2.3 预解析只发生在当前作用域下
开始只对window下的进行预解析，只有函数执行的时候才会对函数中的进行预解析。 
             
## 三、变量提升
会将变量声明提升到当前作用域的顶部，**只是声明提升**，赋值定义不会提升。 
```
//变量声明提升
console.log(a); //undefined
var a = 10;
```

## 四、函数提升
会将函数声明形式的函数**声明和定义整个提升**到当前作用域的顶部，函数表达式形式的函数不会被提升。

``` 
//函数声明和定义整个提升
fn1();
function fn1() {
  console.log(num); //undefined
  num = 20;
  console.log(num); //20
  var num;
}
```  

## 五、注意点

#### 5.1 不管条件是否成立 都会进行预解析

``` 
if(false) {
   var num = 10;
}
console.log(num); //undefined
``` 
	
#### 5.2 函数表达式形式的函数不会提升

``` 
fn(); //Uncaught TypeError: fn1 is not a function
var fn = function () {};
``` 

#### 5.3 函数体中return下面的代码虽然不再执行，但还是会进行预解析
	
``` 
function fn() {
   console.log(num); //undefined
   return function () {};
   var num = 66;
}
fn();
``` 

#### 5.4 立即执行函数定义的function在全局作用域中不进行预解析，当代码执行到这个位置时定义和执行一起完成
``` 
(function(num){
  console.log(num);
})(66);
``` 
