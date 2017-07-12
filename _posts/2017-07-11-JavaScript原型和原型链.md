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
每个**函数**都有一个 prototype 属性
```
function Foo() {
}
Foo.prototype.name = 'zhangsan';
Foo.prototype.getAge = function() {
  return 20;
};
var f = new Foo();
console.log(f.name);
console.log(f.getAge());
```

## __proto__
每一个JS对象(除了 null )都具有的一个 __proto__ 属性，叫，这个属性指向该对象的原型。
```
function Foo() {
}
var f = new Foo();
console.log(f.__proto__ === Person.prototype); // true
```

## constructor
每个原型都有一个 constructor 属性指向关联的构造函数。
``
function Foo() {
}
console.log(Foo.prototype.constructor === Foo); // true
``

完整的原型链图：
![原型链](http://www.mollypages.org/tutorials/jsobj_full.jpg)
