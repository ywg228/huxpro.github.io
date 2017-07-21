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
localStorage 是 HTML5 标准中新加入的技术。

## sessionStorage
sessionStorage 与 localStorage 的接口类似，但保存数据的有效期与 localStorage 不同。sessionStorage 是一个前端的概念，它只是可以将一部分数据在当前会话中保存下来，刷新页面数据依旧存在。但当页面关闭后，sessionStorage 中的数据就会被清空。

## 相同点
- 都会在浏览器端存储数据，有大小限制，同源限制

## 不同点

特性 | cookie | localStorage | sessionStorage
------------ | ------------- | ------------- | -------------
存储数据大小 | 每个cookie不能超过4k，浏览器不能保存超过300个cookie，单个服务器不能超过20个 | 5M | 5M
数据有效期 | 只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭 | 除非手动清除，否则永久保存 | 仅在当前会话下有效，关闭页面或浏览器后被清除
作用域 | 在同源且符合path规则的文档之间共享 | 在同源文档之间共享 | 不在不同的浏览器窗口中共享，即使是同一个页面
与服务器端通信 | 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信，后同

## 应用场景

```
```
