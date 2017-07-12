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
JavaScript中有6种基本数据类型：number、boolean、string、null、undefined、symbol（es6新增），1种引用数据类型：object(array,function) <br>
判断js中的数据类型有四种方法：typeof、instanceof、 constructor、 prototype。

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
局限性：对于Array,Null，RegExp等对象数据类型使用typeof一律返回object，不能检测。
在实际的项目应用中，typeof只有两个用途，就是检测一个元素是否为undefined，或者是否为function。
```
if(typeof a!='undefined'){ //判断一个变量是否存在
}
```
而不要去使用if(a)，因为如果a不存在（未声明）则会出错。
