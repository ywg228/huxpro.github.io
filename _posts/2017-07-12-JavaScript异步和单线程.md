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


同步和异步的区别 分别举一个同步和异步的例子
一个关于setTimeout的笔试题
前端使用异步的场景有哪些

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
