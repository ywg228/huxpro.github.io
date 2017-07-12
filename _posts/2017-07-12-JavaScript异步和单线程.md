---
layout:     post
title:      JavaScript异步和单线程
subtitle:   学习笔记 
date:       2017-07-12
author:     Ywg
header-img: img/home-bg.jpg
catalog:    true
tags: JavaScript
---
## 同步
同步模式，又称阻塞模式，会阻止浏览器的后续处理，停止后续的解析，只有当当前加载完成后，才能进行下一步操作。所以默认同步执行才是安全的。<br>
但如果js中有输出document内容、修改dom、重定向等行为，就会造成页面堵塞。所以我们一般建议把<script>标签放在<body>结尾处，这样尽可能减少页面阻塞。
```
console.log(100);
alert(200); 
console.log(300);
```
## 异步
异步加载又叫非阻塞加载，浏览器在下载执行js的同时，还会继续进行后续页面的处理。<br>
**异步代码需要等待js引擎处理完同步代码之后才会执行。**

### 异步使用的场景
#### 1. 定时任务：setTimeout、setInerval
```
console.log(100);
setTimeout(funtion() {
  console.log(200);
}, 1000);
console.log(300);
```
#### 2. 网络请求：ajax请求、动态img加载
ajax请求：
```
console.log('start');
$.get('./data.json', function(data) {
  console.log(data);
});
console.log('end');
```
动态img加载：
```
console.log('start');
var img = document.createElement('img');
img.onload = function() {
  console.log('loaded');
};
img.src = 'xxx.png';
console.log('end');
```
#### 3. 事件绑定：
```
console.log('start');
document.getElementById('btn1).addEventListener('click', function() {
  console.log('clicked');
});
console.log('end');
```

## 单线程
触发和执行并不是同一概念，计时器的回调函数一定会在指定delay的时间后被触发，但并不一定立即执行，可能需要等待。<br>

所有JavaScript代码是在一个线程里执行的，**像定时任务和事件绑定只有在JS单线程空闲时才执行**。<br>

**JavaScript引擎是单线程运行的,浏览器无论在什么时候都只且只有一个线程在运行JavaScript程序**<br>

但是浏览器内部是多线程的,这些线程在内核控制下相互配合以保持同步。在处理js的异步上浏览器内核的实现可能有多个进程:Javascript引擎线程、界面渲染线程、浏览器事件触发线程、HTTP请求线程……这些线程的名字为渲染引擎、网络、js解析器等。<br>

Javascript除了一个主线程外,还配有一个代码队列,这个队列用以存放定时器、HTTP请求、事件响应的回调。<br>

![单线程](http://images.cnblogs.com/cnblogs_com/wancy86/399808/o_js1.jpg)

```
setTimeout(function(){
  alert('ok'); //这个alert永远不会被执行
},0);
while(true){}
```

## 面试题
```
console.log(1);
setTimeout(function () {
  console.log(2)
}, 0);
console.log(3);
setTimeout(function () {
 console.log(4);
}, 1000);
console.log(5);
//1 3 5 2 4
```
```
/**
 * js的工作机制是：当线程中没有执行任何同步代码的前提下才会执行异步代码，
 * setTimeout是异步代码，所以setTimeout只能等js空闲才会执行，但死循环是永远不会空闲的，所以setTimeout也永远不会执行。
 */
setTimeout(function (){
    console.log('ok');
},1000);
while (true){}
alert('end');
//死循环导致alert不执行,setTimeout也不执行。
```


