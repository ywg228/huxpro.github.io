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
同步模式，又称阻塞模式，会阻止浏览器的后续处理，停止后续的解析，只有当当前加载完成后，才能进行下一步操作。所以默认同步执行才是安全的。但这样如果js中有输出document内容、修改dom、重定向等行为，就会造成页面堵塞。所以我们一般建议把<script>标签放在<body>结尾处，这样尽可能减少页面阻塞。
```
console.log(100);
alert(200); 
console.log(300);
```
## 异步
异步加载又叫非阻塞加载，浏览器在下载执行js的同时，还会继续进行后续页面的处理。
### 异步使用的场景
#### 1. 定时任务：setTimeout、setInerval
```
console.log(100);
setTimeout(funtion() {
  console.log(200);
}, 1000);
console.log(300);
```
#### 2. 网络请求：ajax请求、动态<img>加载
ajax请求：
```
console.log('start');
$.get('./data.json', function(data) {
  console.log(data);
});
console.log('end');
```
动态<img>加载：
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
