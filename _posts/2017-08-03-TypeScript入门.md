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

## 变量声明
#### let 声明
let 声明的变量当前作用域为块作用域，变量在包含它们的块之外是不能被访问的。
```
if (true) {
    let num = 10;
}
console.log(num); //Error: 'num' doesn't exist here
```

#### const 声明
与let声明相似，被赋值后不能再改变，不能对它们重新赋值。
```
const num = 10;
num = 20;
```
#### let vs const
变量如果你不打算去修改，那就使用const，反之则使用let;

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

## 解构表达式
#### 解构数组
``` 
let arr = ['hello', 'world'];;
let [first, second] = arr;
console.log(first, second);
```
使用剩余变量
```
let [first, ...rest] = [1, 2, 3, 4];
console.log(first, rest); //1 Array(3)
```

#### 对象解构
```
let o = {
    a: '123',
    b: true,
    c: '456'
};
let {a, b} = o;
console.log(a, b); //123 true
```
使用剩余变量
```
let {a, ...reset} = o;
console.log(a, reset.b, reset.c); //123 true 456
```

## 箭头表达式
用来声明匿名函数，消除传统匿名函数的this指针问题
``` 
var add = (num1, num2) => {
    return num1 + num2;
}
console.log(add(1, 2));
``` 
``` 
function timer(name: string) {
    this.name = name;
    setInterval(() => {
        console.log(this.name);
    }, 1000);
}
timer('tom');
``` 

## 循环
#### for..of
迭代对象的键对应的值
``` 
let arr = [12, 34, 56, 78];
for (let item of arr) {
    console.log(item);
}
``` 
#### for..of vs for..in 
for..of和for..in均和循环一个集合；但是循环的值却不同，for..in循环的是对象的 键 的列表，而for..of则循环对象的键对应的值。
``` 
let list = [4, 5, 6];

for (let i in list) {
    console.log(i); // "0", "1", "2",
}

for (let i of list) {
    console.log(i); // "4", "5", "6"
}
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
- 类的内外部都可以被访问到
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
- private声明的成员，只能在类的内部被访问到
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
- protected修饰符声明的成员，能在类的内部和子类中被访问到
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

- protected修饰的构造函数不能在类外被实例化，但能被子类继承。
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

## 泛型
- 参数化的类型，用来限制集合的内容

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

var teachers: Array<Person> = [];
teachers[0] = new Teacher('tom', 25);
teachers[1] = 123; //Error: type '123' is not assignable to type 'Person
``` 

## 接口
- 用来建立某种代码约定，使得其他开发者在调用某个方法或创建新的类时必须遵守接口定义的代码约定。
#### 接口声明属性
``` 
interface IPerson {
    name: string;
    age: number;
}
class Person {
    constructor(public config: IPerson) {

    }
}
var p1 = new Person({
    name: 'Tom',
    age: 25
})
``` 

#### 接口声明方法
``` 
interface Animal {
    eat() 
}
class Sheep implements Animal{
    eat() {
        console.log('eat grass');
    }
}
class Tiger implements Animal{
    eat() {
        console.log('eat meat');
    }
}
``` 

## 模块
- 模块在其自身的作用域里执行，而不是在全局作用域里；这意味着定义在一个模块里的变量，函数，类等等在模块外部是不可见的，除非你明确地使用export形式之一导出它们。 相反，如果想使用其它模块导出的变量，函数，类，接口等的时候，你必须要导入它们，可以使用import形式之一。
- 模块是自声明的；两个模块之间的关系是通过在文件级别上使用imports和exports建立的。
- 模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。 

## 注解

## 类型定义文件（*.d.ts）
用来帮助开发者TypeSript中，使用已有的JavaScript工具包，如jQuery
``` 
npm install @types/jquery
``` 

