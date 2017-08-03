---
layout:     post
title:      TypeScript入门
subtitle:   TypeScript
date:       2017-08-03
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: JavaScript 
---

## 前言
- TypeScript是一种由微软开发的自由和开源的编程语言。它是JavaScript的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。遵循ES6

## 优势
- 支持ES6规范
- 强大的IDE支持
- Angular2的开发语言

## compiler工具
- 使用在线compiler：http://www.typescriptlang.org/play/index.html
- 使用本地compiler：WebStorm，新建TypeScript File，设置将TypeScript自动转成JavaScript就行

## TypeScript安装
``` 
npm install -g typescript
``` 

## 字符串
使用双引号或单引号表示字符串。
name为变量名，string为数据类型
``` 
var myName: string = 'ywg';
myName = 'tom';
``` 
字符串中可嵌入表达式，这时用反引号` 
``` 
var myName: string = 'ywg';
var say: string = `My name is ${myName}`;
``` 

## 类
``` 
class Person {
    name: 'tom'; //参数
    constructor(name: string) { //构造函数
        this.name = name;
    }
    sayHello() { //方法
        return 'hello, ' + this.name;
    }
} 
var s = new Person('ywg');
s.sayHello();
``` 

#### 继承
- 子类的构造函数必须调用super()，会执行基类的构造方法。
``` 
class Person {
    name:string;
    constructor(name: string) {
        this.name = name;
    }
    run(distance: number = 0) {
        console.log(this.name + ' Run ' + distance);
    }
}
class Teacher extends Person {
    name:string;
    constructor(name: string) {
        super(name);
    }
    run(distance: number = 10) {
         super.run(distance);
    }
}
class Student extends Person {
    name:string;
    constructor(name: string) {
        super(name);
    }
    run(distance: number = 20) {
         super.run(distance);
    }
}
var t = new Teacher('Tom');
var s:Person = new Student('Jerry');
t.run(); //This is Tom  Tom Run 10
s.run(30); //This is Jerry Jerry Run 30
``` 

#### 默认public修饰符
自由地访问类中定义的成员。
``` 
class Person {
    public name: 'tom'; //参数
    public constructor(name: string) { //构造函数
        this.name = name;
    }
    public sayHello() { //方法
        return 'hello, ' + this.name;
    }
} 
``` 
#### private修饰符
private声明的成员，在它类的外部不能被访问.
``` 
class Person {
    private name: 'tom'; //参数
    constructor(name: string) { //构造函数
        this.name = name;
    }
}
var p = new Person('Tom');
console.log(p.name); // Error: 'name' is private;
``` 
#### protected修饰符
与private修饰符的行为类似，有一点不同：protected成员在子类中仍然可以访问。
``` 
class Person {
    protected name: string;
    constructor(name: string) { this.name = name; }
}

class Teacher extends Person {
    private age: number;
    constructor(name: string, age: number) {
        super(name);
        this.age = age;
    }
    public sayHello() {
        return `Hello, my name is ${this.name} and I'm ${this.age} years old`;
    }
}
var t = new Teacher('tom', 25);
console.log(t.sayHello()); //Hello, my name is tom and I'm 25 years old
console.log(t.name); //Error: Property 'name' is protected
``` 

protected修饰的构造函数不能在类外被实例化，但能被子类继承。
``` 
class Person {
    protected name: string;
    protected constructor(name: string) { this.name = name; }
}

class Teacher extends Person {
    private age: number;
    constructor(name: string, age: number) {
        super(name);
        this.age = age;
    }
    public sayHello() {
        return `Hello, my name is ${this.name} and I'm ${this.age} years old`;
    }
}
var p = new Person("tom"); // Error: The 'Person' constructor is protected
var t = new Teacher('tom', 25);
``` 

## 函数
前两个number是参数类型，第三个number是返回类型，可省略
``` 
function add(x: number, y: number): number {
    return x + y;
}
add(1, 2);
``` 

#### 可选参数
在参数名后加?可实现可选参数功能。
``` 
function printStr(a: string, b?:string) {
  if(b) {
    return a + ' ' + b;
  }else {
    return a;
  }
}
printStr('hello'); //hello
printStr('hello', 'tom'); //hello tom
``` 
可选参数必须放在必选参数后面！！

#### 默认参数
- 带默认值的参数是可选的
- 带默认值的参数不需要放在必须参数的后面
- 如果带默认值的参数在必选参数的前面，必须传递undefined值来获得默认值
- 带默认值的参数的数据类型可省略，即默认值的数据类型
- 当默认参数不传值或者传的值是undefined时则使用默认值
``` 
function printStr(a: string, b='tom') {
  if(b) {
    return a + ' ' + b;
  }else {
    return a;
  }
}
function printStr2(a='hello', b=string) {
  if(b) {
    return a + ' ' + b;
  }else {
    return a;
  }
}
printStr('hello'); //hello tom
printStr('hello', 'ywg'); //hello ywg
printStr('hello', undefined); //hello tom
printStr2(undefined, 'tom'); //hello tom
``` 

#### 剩余参数
- 在参数前加...就是剩余参数
- 剩余参数是个数组，代表之后的所有参数，类似arguments
- 剩余参数会被当做个数不限的可选参数。 可以一个也没有，也可以是多个
- 剩余参数一定要在其它参数的后面
``` 
function printStr(firstArg: string, ...restOfArgs: string[]) {
  return firstArg + ' '+ restOfArgs.join(' ');
}
printStr('hello', 'tom', 'my', 'name', 'is', 'ywg');
``` 

