---
layout:     post
title:      JavaScript变量作用域、作用域链和闭包
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
        var k = 20;
    }
    console.log(i); //10
    console.log(k); //20
  }
console.log(j); //1
```

## ES6中的块级作用域
但作用域限定在块级，let声明的变量不存在变量声明提升。<br>
ES6规定，如果代码块中存在let变量，这个区块从一开始就形成了封闭作用域。凡是在声明之前就使用，就会报错。即在代码块内，在let声明之前使用变量都是不可用的。
```
if(true) {
  let b = 20;
}
console.log(b); //访问不到，报错
```
```
for (let i = 0; i < 10; i++) {
    
}
console.log(i); //访问不到，报错
```
```
//let声明的变量不存在变量声明提升
function fn() {
    console.log(a) //a先使用后声明，报语法错
    let a;
}
```
### 注意点
#### 1. 不能重复声明
```
// 两个let重复声明
let age = 24;
let age = 30; //执行时会报错
```
#### 2. 有了let后，匿名函数自执行就可以去掉了
```
// 匿名函数写法
(function () {
  var fn = function() {};
  // ...
  window.fn = fn;
})();
// 块级作用域写法
{
  let fn = function() {};
  // ...
  window.fn = fn;
}
```
### 为什么需要块级作用域？
没有块级作用域，会带来很多不合理的场景：
#### 1. 第一种场景：内层变量可能会覆盖外层变量.
```
var tmp = new Date();
function f() {
  console.log(tmp); 
  if (false) {
    var tmp = 'hello world';
  }
}
f(); // undefined
```
由于变量声明提升，导致内层的tmp变量覆盖了外层的tmp变量。
#### 2. 第二种场景：用来计数的循环变量泄露为全局变量
```
var s = 'hello';
for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}
console.log(i); // 5
```
变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。

## 作用域链
在javascript中，每个函数都有自己的执行上下文环境，当代码在这个环境中执行时，会创建变量对象的作用域链，作用域链是一个对象列表或对象链，它保证了变量对象的有序访问。 <br>
作用域链的前端是当前代码执行环境的变量对象，常被称之为“活跃对象”，变量的查找会从第一个链的对象开始，如果对象中包含变量属性，那么就停止查找，如果没有就会继续向上级作用域链查找，直到找到全局对象中。 <br>
作用域链的逐级查找，也会影响到程序的性能，变量作用域链越长对性能影响越大，这也是我们尽量避免使用全局变量的一个主要原因。
```
var outVariable = "我是最外层变量"; //最外层变量
function outFun() { //最外层函数
    var inVariable = "内层变量";
    function innerFun() { //内层函数
        console.log(inVariable);
        var tempVariable = inVariable;
    }
    innerFun();
}
```
其作用域链为：
```
window
├──outVariable
└──outFun()
   ├──inVariable
   └──innerFun()
      └──tempVariable
```
对于 innerFun()，其作用域链包含 3 个对象：innerFun() 自己的变量对象、outFun()的变量对象、全局变量对象。

## 闭包
闭包是指在当前作用域内总是能访问外部作用域中的变量。
