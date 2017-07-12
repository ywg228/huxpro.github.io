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
```
function Foo() {
}
Foo.prototype.x = 'hello';
var f1 = new Foo();
console.log(f1.x); //hello
console.log(Foo.x) //undefined
```

## __proto__
每一个JS对象(除了 null )都具有的一个 __proto__ 属性，这个属性指向对象所属类的原型。即f1.__proto__ === Foo.prototype ->  true
```
function Foo() {
}
var f1 = new Foo();
console.log(f1.__proto__ === Person.prototype); // true
```

## constructor
每个原型都有一个 constructor 属性指向关联的构造函数。 Foo.prototype.constructor === Foo ->  true 
```
function Foo() {
}
console.log(Foo.prototype.constructor === Foo); // true
```
浏览器默认给 prototype 开辟的堆内存中自带一个属性：constructor，指向当前类本身
![constructor](http://images.cnitblog.com/blog/138012/201409/172130097842386.png)

## 完整的原型链图
![原型链](http://www.mollypages.org/tutorials/jsobj_full.jpg)
图片来自  [mollypages.org](http://www.mollypages.org/tutorials/js.mp) 
