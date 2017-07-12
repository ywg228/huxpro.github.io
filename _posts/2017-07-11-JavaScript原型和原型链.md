---
layout:     post
title:      JavaScript原型和原型链
subtitle:   学习笔记 
date:       2017-07-11
author:     Ywg
header-img: img/home-bg.jpg
catalog:    true
tags: JavaScript
---

## prototype
每个**函数**都有一个 prototype 属性，它存储的值是一个对象数据类型的值，浏览器默认为它开辟一个堆内存。
这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。
```
function Person() {
}
Person.prototype.name = 'zhangsan';
var p1 = new Person();
console.log(p1.name); //hello
console.log(Person.name) //undefined
```
如图：<br>
![prototype](https://raw.githubusercontent.com/mqyqingfeng/Blog/master/Images/prototype1.png)

##  __proto__
每一个**JS对象**(除了 null )都具有的一个__proto__属性，这个属性指向当前对象所属构造函数的原型。<br>
这也保证了实例能够访问在构造函数原型中定义的属性和方法。
```
function Person() {
}
var p1 = new Person();
console.log(p1.__proto__ === Person.prototype); // true
```
如图：<br>
![__proto__](https://github.com/mqyqingfeng/Blog/raw/master/Images/prototype2.png)

## constructor
每个**原型**都有一个 constructor 属性指向关联的构造函数。 
```
function Person() {
}
console.log(Person.prototype.constructor === Person); // true
console.log(p1.__proto__.constructor === Person); //true
```
如图：<br>
![constructor](https://raw.githubusercontent.com/mqyqingfeng/Blog/master/Images/prototype3.png)

浏览器默认给 prototype 开辟的堆内存中自带一个属性：constructor，指向当前类本身
![constructor](http://images.cnitblog.com/blog/138012/201409/172130097842386.png)

## 原型链
通过 对象名.属性名 的方式获取属性值：<br>
首先在对象的私有属性上进行查找，如果私有属性中存在，则获取私有属性值， 反之，则通过__proto__找到所属构造函数的原型（构造函数的原型上定义的方法和属性都是当前实例公用的），原型上存在的话则获取公有的属性值， 如果原型上也没有，则继续通过原型上的__proto__继续向上查找，一直找到Object.prototype为止，这就是原型链。
```
console.log(p1.__proto__.__proto__ === Object.prototype); //true
console.log(p1.__proto__.__proto__.constructor === Object); //true
//查找属性的时候查到 Object.prototype 就停止查找了。
console.log(Object.prototype.__proto__ === null) // true
```
如图：<br>
![原型链](https://github.com/mqyqingfeng/Blog/raw/master/Images/prototype5.png)
## 完整的原型链图
![原型链](http://www.mollypages.org/tutorials/jsobj_full.jpg)
图片来自  [mollypages.org](http://www.mollypages.org/tutorials/js.mp) 

## 注意点
### 只有函数才有prototype属性
### p1.constructor === Person
当获取 p1.constructor 时，其实 p1 中并没有 constructor 属性,当不能读取到constructor 属性时，会从 p1 的原型也就是 Person.prototype 中读取，
正好原型中有该属性，所以：p1.constructor === Person.prototype.constructor === Person
### 所有prototype的__proto__都指向Object.prototype


