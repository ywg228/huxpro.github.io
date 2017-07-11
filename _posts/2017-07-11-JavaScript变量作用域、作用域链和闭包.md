---
layout:     post
title:      JavaScript变量作用域和作用域链
subtitle:   学习笔记 
date:       2017-07-11
author:     Ywg
header-img: img/home-bg.jpg
catalog:    true
tags: JavaScript
---

## 作用域
定义：只会对某个范围产生作用，而不会对外产生影响的封闭空间。在这样的一些空间里，外部不能访问内部变量，但内部可以访问外部变量。这就是作用域。<br>
在ES5中，js只有两种作用域：全局作用域和函数作用域 没有块级作用域！！<br>
块级作用域：任何一对花括号（｛和｝）中的if for while等语句集都属于一个块，在这之中定义的所有变量在代码块外都是不可见的，这就是块级作用域。 <br>
在ES6中，新增了一个块级作用域（最近的大括号涵盖的范围），但是仅限于let声明的变量。<br>

## 函数体内部，局部变量的优先级高于同名的全局变量
``` 
var a = 10    //定义全局变量 
function fn(){
  var a = 20;    //定义局部变量
  console.log(a);
}
fn(); //20
console.log(a); //10
``` 

## ES5中没有块级作用域
``` 
if(true) {
  var b = 20;
}
console.log(b); //20
``` 
```
var scope = {};
  if (scope instanceof Object) { //true
    var j = 1;
    for (var i = 0; i < 10; i++) {
         //console.log(i);
    }
    console.log(i); //10
  }
console.log(j); //1
```
## 变量声明提前数内声明的变量在函数体内始终是可见的
```
var a =10；
function fn() {
    console.log(scope);
    var a = 20;
}
fn(); //undefined
```
输出是undefined，并没有报错，这是因为变量声明提前以及局部变量的优先级高于同名的全局变量 <br>
上面的代码等效于：
```
var a = 10;
function fn() {
    var a;
    console.log(a);
    a = 20;
}
fn(); //undefined
```

## 未使用var关键字定义的变量都是全局变量。
如果变量如果不加var，那么变量就被声明为全局变量，自动加入到全局作用域中。
```
function fn() {
    a = 100;  
}
fn();
console.log(a); //100
```

## 全局变量都是window对象的属性
```
var a = 100 ;
console.log(window.a); //100
```

## 作用域链
在javascript中，每个函数都有自己的执行上下文环境，当代码在这个环境中执行时，会创建变量对象的作用域链，作用域链是一个对象列表或对象链，它保证了变量对象的有序访问。 <br>
作用域链的前端是当前代码执行环境的变量对象，常被称之为“活跃对象”，变量的查找会从第一个链的对象开始，如果对象中包含变量属性，那么就停止查找，如果没有就会继续向上级作用域链查找，直到找到全局对象中。 <br>
作用域链的逐级查找，也会影响到程序的性能，变量作用域链越长对性能影响越大，这也是我们尽量避免使用全局变量的一个主要原因。
