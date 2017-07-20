---
layout:     post
title:      Fiddler web调试工具使用
subtitle:   调试工具
date:       2017-07-20
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: 工具
--- 

## 前言
  Fiddler是一个http协议调试代理工具，它能够记录客户端和服务器之间的所有 HTTP请求，可以针对特定的HTTP请求，分析请求数据、设置断点、调试web应用、修改请求的数据，甚至可以修改服务器返回的数据，功能非常强大，是web调试的利器。

## 工作原理

## 使用场景
#### 开发环境host配置：
- 通常情况下，配置host需要改变系统文件很不方便，在多个开发环境下切换很低效
- fiddler提供了相对高效的host配置方法

#### 前后端接口调试：
- 通常情况下，调试前后端的接口需要真是的环境，一大推假数据，写javascript代码
- fiddler只需要一个ui界面进行配置即可

#### 线上bugfix
- fiddler可以将发布文件代理到本地，快速定位线上bug

#### 性能分析和优化
- fiddler会提供请求的实际图，清晰明了，网站需要优化的部分
