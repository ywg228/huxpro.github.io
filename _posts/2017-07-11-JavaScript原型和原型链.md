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
function Person() {
}
Person.prototype.name = 'zhangsan';
Person.prototype.getAge = function() {
  return 20;
};
var person = new Person();
console.log(person.name);
console.log(person.getAge());
```

## __proto__
每一个JS对象(除了 null )都具有的一个__proto__ 属性，叫，这个属性指向该对象的原型。
```
function Person() {
}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true
```

## constructor



完整的原型链图：
![原型链](http://www.mollypages.org/tutorials/jsobj_full.jpg)
