---
layout:     post
title:      JavaScript call、apply和bind改变this指向
subtitle:   学习笔记
date:       2017-07-11
author:     Ywg
header-img: img/home-bg.jpg
catalog:    true
tags: JavaScript 
---

# 前言
call、apply和bind是Function对象自带的三个方法，都用来改变函数中的this指向。

# call
fun.call(thisArg[, arg1[, arg2[, ...]]]) <br>
thisArg:在fun函数运行时指定的this值

thisArg取值四种情况:<br>
  1)不传，或者传null,undefined， 函数中的this指向window对象<br>
  2)传递另一个函数的函数名，函数中的this指向这个函数的引用<br>
  3)传递字符串、数值或布尔类型等基础类型，函数中的this指向其对应的包装对象，如 String、Number、Boolean<br>
  4)传递一个对象，函数中的this指向这个对象<br>
```
//这个例子说明第一个参数写谁，function中的this就指向谁
function add(a, b) {
  console.dir(this);
}

function sub(a, b) {
  console.dir(this);
}

add(1, 2); //this -> window
sub(1, 2); //this -> window

add.call(sub, 1, 2); //this -> function sub()
sub.call(add, 1, 2); //this -> function add()
```
    
# apply
fun.apply(thisArg, [argsArray])<br>
```
//这个例子说明call和apply传参的区别
var foo = {
  x: 100
};

function bar(a, b) {
  console.log(this.x);
}

bar.call(foo, 1, 2); //this -> foo
bar.apply(foo, [1, 2]); //this -> foo
```
# bind 
fun.bind(thisArg[, arg1[, arg2[, ...]]]) <br>
bind是在EcmaScript5中扩展的方法（IE6,7,8不支持）
```
var foo = {
  x: 100
};

function bar(a, b) {
  console.log(this.x);
}

bar.call(foo, 1, 2); //this -> foo
bar.apply(foo, [1, 2]); //this -> foo
var func = bar.bind(foo, 1, 2); //this -> foo
func();
```
# call、apply和bind的区别
相同点：都是用来改变this指向 <br>
不同点： <br>
1)call、bind方法接受的是若干个参数的列表，而apply()方法接受的是一个包含多个参数的数组。 <br>
2)call、apply都是立即执行函数，bind()则是返回一个函数，便于稍后调用。

# 通俗理解
call()、apply()、bind()都是Function对象的一个方法，他的第一个参数是this，第二个是函数的参数。<br>
比如你的方法里写了this,普通调用这个方法这个this可能是window。<br>
而如果你用了call/apply/bind，第一个参数写谁，函数里的this就指向谁。 <br>

# 适用条件:
当你的参数是明确知道数量时用 call 。 <br>
而不确定的时候用 apply，然后把参数 push 进数组传递进去。 <br>
当参数数量不确定时，函数内部也可以通过 arguments 这个类数组对象来遍历所有的参数。 <br>

# 应用
## 求最大值和最小值:apply
```
var arr = [12, 13231, 1213, 223, 524];
var min = Math.min.apply({}, arr), // Math.min.apply(Math, arr) Math.min.apply(null, arr),
    max = Math.max.apply({}, arr);
console.log('最小值=' + min + ' 最大值=' + max); //最小值=12 最大值=13231
```
## 求平均数:call
```
function avgFn() {
  var newArr = Array.prototype.slice.call(arguments);
  newArr.sort(function (a, b) {
    return a - b;
  });
  newArr.shift();
  newArr.pop();
  return (eval(newArr.join('+')) / newArr.length).toFixed(2);
}
console.log(avgFn(2, 3, 123, 13, 12, 12)); //10.00
```
## 判断是否是数组:call
```
function isArray(obj){ 
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```
