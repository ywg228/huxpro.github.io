---
layout:     post
title:      JS三种存储方式cookie、sessionStorage和localStorage的区别
subtitle:   JS基础
date:       2017-07-20
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: JavaScript JS基础
---
## cookie
Cookie 是小甜饼的意思。顾名思义，cookie 确实非常小，它的大小限制为4KB左右，它的主要用途有保存登录信息，比如你登录某个网站市场可以看到“记住密码”，这通常就是通过在 Cookie 中存入一段辨别用户身份的数据来实现的。

## localStorage
- localStorage 是 HTML5 标准中新加入的技术。

#### 添加数据
- localStorage.setItem("key", "value");
#### 读取数据
- localStorage.getItem("key");
#### 删除某个键名的数据
- localStorage.removeItem("key");
#### 删除所保存的数据
- localStorage.clear();

#### sessionStorage
- sessionStorage 与 localStorage 的接口类似，但保存数据的有效期与 localStorage 不同。sessionStorage 是一个前端的概念，它只是可以将一部分数据在当前会话中保存下来，刷新页面数据依旧存在。但当页面关闭后，sessionStorage 中的数据就会被清空。

## 相同点
- 都会在浏览器端存储数据，有大小限制，同源限制

## 不同点

特性 | cookie | localStorage | sessionStorage
------------ | ------------- | ------------- | -------------
存储数据大小 | 每个cookie不能超过4k，浏览器不能保存超过300个cookie，单个服务器不能超过20个 | 5M | 5M
数据有效期 | 一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效 | 除非手动清除，否则永久保存 | 仅在当前会话下有效，关闭页面或浏览器后被清除
作用域 | 在同源且符合path规则的文档之间共享 | 在同源文档之间共享 | 不在不同的浏览器窗口中共享，即使是同一个页面
与服务器端通信 | 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信
安全性 | cookie中最好不要放置任何明文的东西 | 两个storage的数据提交后在服务端一定要校验 
## 应用场景
- 判断用户是否登录：Cookie，因为每个 HTTP 请求都会带着 Cookie 的信息。针对登录过的用户，服务器端会在他登录时往 Cookie 中插入一段加密过的唯一辨识单一用户的辨识码，下次只要读取这个值就可以判断当前用户是否登录啦。
- 电商网站的购物车：localStorage
- HTML5游戏本地数据：localStorage
- 多个子页面的表单页面，按步骤引导用户填写：sessionStorage 

## 注意
不是什么数据都适合放在 Cookie、localStorage 和 sessionStorage 中的。使用它们的时候，需要时刻注意是否有代码存在 XSS 注入的风险。因为只要打开控制台，你就随意修改它们的值，也就是说如果你的网站中有 XSS 的风险，它们就能对你的 localStorage 肆意妄为。所以千万不要用它们存储你系统中的敏感数据。
```
```
