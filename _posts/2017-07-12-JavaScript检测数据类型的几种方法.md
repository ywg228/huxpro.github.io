---
layout:     post
title:      JavaScript检测数据类型的几种方法
subtitle:   学习笔记 
date:       2017-07-12
author:     Ywg
header-img: img/home-bg.jpg
catalog:    true
tags: JavaScript
---

## 前言
JavaScript中有6种基本数据类型：number、boolean、string、null、undefined、symbol（es6新增），1种引用数据类型：object(array,function)。 <br>
判断js中的数据类型有四种方法：typeof、instanceof、 constructor、 toString。

## typeof
检测数据类型的运算符，返回一个字符串，包含对应的数据类型：number string boolean object function undefined。
```
console.log(typeof 12); //'number'
console.log(typeof '12'); //'string'
console.log(typeof true); //'boolean'
console.log(typeof num); //'undefined'
console.log(typeof undefined); //'undefined'
console.log(typeof function () {}); //'function'
console.log(typeof null); //'object' 无效
console.log(typeof []); //'object' 无效
console.log(typeof  new Date()); //object 无效
console.log(typeof  new RegExp());  //object 无效
```
#### 局限性
**对于Array，RegExp，Date等对象数据类型使用typeof一律返回object**，不能检测。<br>
#### 适用条件
typeof只能用于基本数据类型检测，对于**null还有bug**。<br>
#### 应用
在实际的项目应用中，typeof只有两个用途，就是检测一个元素是否为undefined，或者是否为function。<br>
**应用一**：检测一个元素是否为undefined，并添加默认值
```
function fn(num1, num2) {
   // 方式一
   if (typeof num2 === 'undefined') { //判断一个变量是否存在
      num2 = 0;
   }
   // 方式二：不适用fn(10,false)这种情况
   num2 = num2 || 0;
}
fn(10);
```
而不要去使用if(num)，因为如果num不存在（未声明）则会出错。<br>
**应用二**：回调函数调用
```
function fn(callback) {
    //typeof callback === 'function' ? callback() : null;
    callback && callback();
}
fn(function () {
});
```

## instanceof
检测某个实例是否属于某个类
```
console.log(1 instanceof Number);               //false
console.log(new Number(1) instanceof Number);   //true
console.log('' instanceof String);              //false
console.log(new String('1') instanceof String); //true
console.log([] instanceof Array);               //true
console.log([] instanceof Object);              //true
function fn() {}
console.log(fn instanceof Function);            //true
console.log(fn instanceof Object);              //true
```
#### 局限性
对于基本数据类型来说，字面量创建和实例方式创建出来的结果有区别，从严格意义上来讲，只有实例创建出来的结果才是标准对象数据类型的值，所以**不能用来检测和处理字面量方式创建的基本数据类型的值**。<br>
instanceof不可以检测null和undefined
#### 适用条件
instanceof适用于检测对象，它是基于原型链运作的。

## constructor
构造函数，作用和instanceof非常相似，可以处理基本数据类型的检测
```
var obj = [];
console.log(obj.constructor === Array);  //true
console.log(obj.constructor === RegExp); //false
var num = 1;
console.log(num.constructor === Number); //true
var reg = /^$/;
console.log(reg.constructor === RegExp); //true
console.log(reg.constructor === Object); //false
```
#### 局限性
把类的原型进行重写，在重写的过程中很有可能把之前的constructor给覆盖了,这样检测的结果是不准确的。 <br>
constructor不可以检测null和undefined
#### 适用条件
constructor指向的是最初创建者，而且容易伪造，不适合做类型判断；

## Object.prototype.toString.call()  最准确最常用的方式
首先获取Object原型的toString方法，让方法执行，并且改变this指向，让toString方法中的this指向第一个参数的值。<br>
Object.prototype.toString作用是返回当前方法的执行主体（方法中this）所属类的详细信息。<br>
'[object Array]':第一个object代表当前实例是对象数据类型的，**注意是小写**，第二个Array代表的是this所属的类是Array。 <br>
Object.prototype.toString.call()可以检测null和undefined，第二、三种方法不可以检测null和undefined
```
console.log(Object.prototype.toString.call(1)); //'[object Number]'
console.log(Object.prototype.toString.call('1')); //'[object String]'
console.log(Object.prototype.toString.call(true)); //'[object Boolean]'
console.log(Object.prototype.toString.call([])); //'[object Array]'
console.log(Object.prototype.toString.call(/^$/)); //'[object RegExp]'
console.log(Object.prototype.toString.call(new Date())); //'[object Date]'
console.log(Object.prototype.toString.call(null)); //'[object Null]'
console.log(Object.prototype.toString.call(undefined)); //'[object Undefined]'
console.log(Object.prototype.toString.call(function(){})); //'[object Function]'
```
#### 适用条件
toString适用于ECMA内置JavaScript类型（包括基本数据类型和内置对象）的类型判断。
#### 应用
```
funtion isArray(obj) { //判断一个变量是否是数组
   return Object.prototype.toString.call(obj) === '[object Array]';
}
```

## jQuery中的方法
```
var type = function (obj) {
        var class2type = {};
        //把所有的数据类型遍历进class2type里面 jQuery中用的是自己封装的each  forEach() 是es5新增的方法
        'Boolean Number String Function Array Date RegExp Object Error Symbol'.split(' ').forEach(function (name, i) {
            class2type['[object ' + name + ']'] = name.toLowerCase();
        });

        if (obj == null) { // obj === null ||  obj === undefined
            return obj + '';
        }

        return typeof obj === 'object' || typeof obj === 'function' ?
            class2type[Object.prototype.toString.call(obj)] || 'object' :
            typeof obj;
    }

    console.log(type(1)); //number
    console.log(type('1')); //string
    console.log(type(true)); //boolean
    console.log(type([])); //array
    console.log(type({})); //object
    console.log(type(new RegExp())); //regexp
    console.log(type(new Date())); //date
    console.log(type(function () {})); //function
```
