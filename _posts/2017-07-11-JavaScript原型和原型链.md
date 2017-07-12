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
## 前言
```
function Person(){
    this.name = 'zhangsan';
}
Person.prototype.getName = function(){
    console.log(this.name);
}
var p1 = new Person();
p1.getName();      //'zhangsan' 
```
上面这个例子中的p1获取到了getName方法和name属性，这是怎么做到的呢？

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

函数定义的时候会自带一个prototype属性，浏览器默认为它开辟一个堆内存。<br>
浏览器默认给 prototype 开辟的堆内存中自带一个属性：constructor，指向当前构造函数本身。<br>
![constructor](http://upload-images.jianshu.io/upload_images/1430985-b650bc438f236877.jpg)

## 原型链模式
访问一个对象的属性：<br>
首先在对象的私有属性上进行查找，如果私有属性中存在，则获取私有属性值， 反之，则通过__proto__找到所属构造函数的原型（构造函数的原型上定义的方法和属性都是当前实例公用的），原型上存在的话则获取公有的属性值， 如果原型上也没有，则继续通过原型上的__proto__继续向上查找，一直找到Object.prototype为止，这就是原型链。
```
console.log(p1.__proto__.__proto__ === Object.prototype); //true
console.log(p1.__proto__.__proto__.constructor === Object); //true
//查找属性的时候查到 Object.prototype 就停止查找了。
console.log(Object.prototype.__proto__ === null) // true
```
如图：<br>
![原型链](https://github.com/mqyqingfeng/Blog/raw/master/Images/prototype5.png)
#### 1. 判断某对象是否含有特定的自身属性 hasOwnProperty
需要注意的是，此方法无法检查某对象的原型链中是否具有该属性，该属性必须是对象本身的一个成员。
```
//获取对象所有的自身属性
function Person() {
   this.name = 'zhangsan';
   this.age = 20;
}  
Person.prototype.getName = function(){
    console.log(this.name);
}
Person.prototype.getAge = function(){
    console.log(this.age);
}
var p1 = new Person();
for(var item in p1) {
    if(p1.hasOwnProperty(item)) {
        console.log(item); //name age
    }
}
```
对象的原型链是沿着__proto__这条线走的，因此在查找p1.hasOwnProperty属性时，就会顺着原型链一直查找到Object.prototype。<br>

而**isPrototypeOf** 则是用来判断一个对象是否存在于另一个对象的原型链上。
```
console.log(Person.prototype.isPrototypeOf(p1)); //true
```
#### 2. instanceof 运算符用来检测运行时一个对象是否是构造函数的一个实例,
```
if(p1 instanceof Person) {
}
```
#### 3. 原型继承
由于所有对象的原型链都会找到Object.prototype，因此所有的对象都会有Object.prototype中的方法，这就是“原型继承”。
#### 4. 完整的原型链图
![原型链](http://www.mollypages.org/tutorials/jsobj_full.jpg)
图片来自  [mollypages.org](http://www.mollypages.org/tutorials/js.mp) 

## 注意点
#### 1. 只有函数才有prototype属性
#### 2. p1.constructor === Person
当获取 p1.constructor 时，其实 p1 中并没有 constructor 属性,当不能读取到constructor 属性时，会从 p1 的原型也就是 Person.prototype 中读取，
正好原型中有该属性，所以：p1.constructor === Person.prototype.constructor === Person
#### 3. 所有prototype的__proto__都指向Object.prototype
#### 4. 构造函数的原型上定义的方法和属性都是当前实例公用的
```
function Person() {
}
Person.prototype.name = 'shangsan';
Person.prototype.age = '20';
var p1 = new Person();
p1.name = 'lisi';
console.log(p1.name); // lisi
console.log(p1.age); // 20
```
#### 5. 所有函数对象(包括Object)的__proto__都指向Function.prototype，它是一个空函数
Object.__proto__ === Function.prototype // true <br>
Object 是函数对象，是通过new Function()创建的，所以Object.__proto__指向Function.prototype。
```
Number.__proto__ === Function.prototype  // true
Number.constructor == Function //true

Boolean.__proto__ === Function.prototype // true
Boolean.constructor == Function //true

String.__proto__ === Function.prototype  // true
String.constructor == Function //true

// 所有的构造器都来自于Function.prototype，甚至包括根构造器Object及Function自身
Object.__proto__ === Function.prototype  // true
Object.constructor == Function // true

// 所有的构造器都来自于Function.prototype，甚至包括根构造器Object及Function自身
Function.__proto__ === Function.prototype // true
Function.constructor == Function //true

Array.__proto__ === Function.prototype   // true
Array.constructor == Function //true

RegExp.__proto__ === Function.prototype  // true
RegExp.constructor == Function //true

Error.__proto__ === Function.prototype   // true
Error.constructor == Function //true

Date.__proto__ === Function.prototype    // true
Date.constructor == Function //true
```

## 原型链模式中的this
常见两种情况：<br>
#### 1. 在构造函数中的this
this.XXX = XXX; this -> 当前构造函数的实例
```
function Person() {
  this.name = 'zhangsan';
}
var p1 = new Person();
console.log(p1.name); // zhangsan
```
####  2. 某一个方法中的this 
看方法执行的时候：'.'前面是谁，this就是谁<br>
1)先确定this的指向，看方法执行时'.'前面的内容<br>
2)把this替换成对应的代码<br>
3)最后按照原型链的查找机制，一步步的查找对应值的即可(看原型链模式这一块) <br>
```    
function Person() {
  this.name = 'zhangsan';
  this.age = 20;
  this.getAge = function () {
    console.log(this.age);
  }
}
//重构原型对象:开辟一个新的堆内存，存储公用的属性和方法，把浏览器默认给Person.prototype开辟的哪个堆内存给替换掉
Person.prototype = {
  /**
  *  只有浏览器默认给Person.prototype的开辟的堆内存才有constructor这个属性，自己手动开辟的堆内存没有这个属性
  *  这样constructor的指向就不再是Person而是Object
  *  为了与原来保持一致，我们需要手动增加constructor的指向
  */
  constructor: Person,
  age: 30,
  getName: function () {
    console.log(this.name);
  },
  getAge: function () {
     onsole.log(this.age);
  }
}
var p1 = new Person();
p1.getName(); //p1.name - > 'zhangsan'
p1.__proto__.getName(); //p1.__proto__.name -> undefined
Person.prototype.getName(); //undefined
p1.getAge(); //p1.age -> 20
p1.__proto__.getAge(); //p1.__proto__.age -> 30
```     

## 原型继承
