---
layout:     post
title:      JS AJAX和JSONP
subtitle:   JS基础
date:       2017-07-14
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: JavaScript JS基础
---

## AJAX 
Asynchronous JavaScript and XML，异步请求数据，不需要刷新整个页面，局部刷新，核心是 XMLHttpRequest 对象 <br>
#### 请求步骤
1. 创建XMLHttpRequest对象
2. 连接服务器
3. 发送请求
4. 接收响应数据

#### 简单封装
``` 
/**
 * ajax异步请求简单封装
 * 使用方式
 *  ajax({
        url : '',
        type: 'GET',
        data: {},
        success: function (res) {
            console.log(res);
        },
        fail: function (status) {
            console.log(status);
        }
    })
 */
function ajax(params) {
    params = params || {};
    if (!params.url)return;
    params.data = params.data || {};
    params.async = params.async || true; //是否异步请求 默认true
    params.type = (params.type || 'GET').toUpperCase();  //请求方式 默认GET

    // 避免有特殊字符，必须格式化传输数据
    params.data = formatParams(params.data);

    //1.创建XMLHttpRequest对象
    var xhr = null;
    if (window.XMLHttpRequest) {  //非IE6
        xhr = new XMLHttpRequest();
    } else { //IE6及其以下版本浏览器
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }

    //2.连接服务器并发送请求
    if (params.type == 'GET') {
        //连接服务器 三个参数：请求方式、请求地址(get方式时，传输数据是加在地址后的)、是否异步请求(同步请求的情况极少)
        xhr.open('GET', params.url + '?' + params.data, params.async);
        //发送请求
        xhr.send(null);
    } else if (params.type == 'POST') {
        xhr.open('POST', params.url, params.async);
        //设置表单提交时的内容类型
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded;charset=UTF-8');
        //发送请求并传输数据
        xhr.send(params.data);
    }

    //3.接受响应数据
    xhr.onreadystatechange = function () {
        //readyState为4表示已接受到全部的响应数据
        if (xhr.readyState == 4) {
            //响应的HTTP状态码，以2开头的都是成功
            //304表示从缓存中获取 上面代码已经在每次请求时都加了随机数，所以无需从缓存中取值
            var status = xhr.status;
            if (status >= 200 && status < 300) {

                var response = xhr.responseText; //默认字符串数据
                // 判断接受数据的内容类型
                var type = xhr.getResponseHeader('Content-type');
                if (type.indexOf('xml') !== -1 && xhr.responseXML) {

                    response = xhr.responseXML; //Document对象响应

                } else if (type === 'application/json') { //JSON数据
                    try {
                        response = JSON.parse(response);
                    }catch (e) {
                        response = eval('(' + response + ')');
                    }
                }
                //成功回调
                params.success && params.success(response);
            } else {
                params.fail && params.fail(status);
            }
        }
    }
}
//格式化参数
function formatParams(data) {
    var arr = [];
    for (var name in data) {
        //encodeURIComponent()用于对 URI 中的某一部分进行编码
        arr.push(encodeURIComponent(name) + '=' + encodeURIComponent(data[name]));
    }
    //添加一个随机数参数，防止缓存
    arr.push(('v=' + Math.random()).replace('.', ''));
    return arr.join('&');
}
``` 
![](http://images2015.cnblogs.com/blog/63651/201612/63651-20161218091406308-2001323373.png)
#### AJAX不能跨域请求！
#### 三个标签允许跨域加载资源
``` 
<img src="xxx"> 可用于打点统计，统计网站可能是其他域
<link href="xxx"> link，script 可用于CDN，CDN也是其他域
<script src="xxx"></script> 用于JSONP 下面有讲到
``` 
### 优缺点
#### 优点
1. 无刷新更新数据，局部刷新，
2. 异步与服务器通信，非阻塞方式提升了用户体验
3. 不需要任何浏览器插件，但需要用户允许 JavaScript在浏览器上执行 。
#### 缺点
1. 破坏浏览器的后退与加入收藏书签功能。在用AJAX动态更新页面的情况下，用户无法回到前一个页面状态。
2. 对搜索引擎的支持不足
### 适用场景

## JSONP 
一种跨域请求数据的方式。
### 同源策略
AJAX之所以不能’跨域‘，就是因为浏览器的同源策略，即一个页面的AJAX只能获取这个页面相同源或者相同域的数据。 <br>
何为“同源”或者“同域”？——协议、域名、端口号都必须相同，如下：
``` 
http://example.com 和 https://example.com 不同，因为协议不同；   

http://example.com 和 http://example1.com 不同，因为域名不同；   

http://localhost:8080 和 http://localhost:1000 不同，因为端口不同；   

http://localhost:8080 和 https://example.com 不同，协议、域名、端口号都不同，
``` 
如果我们跨域请求，会出现如下错误：
``` 
XMLHttpRequest cannot load XXX. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.
``` 
那如何跨域请求？这就需要JSONP了。
### 核心原理
利用script标签可以跨域请求的特性，可以通过向页面中动态添加script标签来完成对跨域资源的访问<br>
通过页面上的script标签，向服务器请求JSON数据；当服务器接受到请求后，会将数据放在一个由用户指定的回调函数里返回给页面，用户则可以通过这个回掉函数来获取请求结果中的数据。
### 请求步骤
1. 请求阶段：浏览器创建一个 script 标签，并给其src 赋值(类似 http://suggestion.baidu.com/su?wd=jsonp&cb=jsonpCallback）。
2. 发送请求：当给script的src赋值时，浏览器就会发起一个请求。
3. 数据响应：服务端将要返回的数据作为参数和函数名称拼接在一起(格式类似”jsonpCallback({name: 'abc'})”)返回。当浏览器接收到了响应数据，由于发起请求的是 script，所以相当于直接调用 jsonpCallback 方法，并且传入了一个参数。

### 简单封装
```
/**
 * jsonp跨域请求简单封装
 * 使用方式：
 * jsonp({
            //百度搜索数据地址
            url: 'http://suggestion.baidu.com/su',
            //数据接口
            data: {
                'wd': 'jsonp'
            },
            callback: 'cb', 
            //响应成功
            success: function (res) {
               console.log(res.s);
            },
            //响应超时
            fail: function (msg) {
                console.log(msg.message);
            }
        });
   后台
   <?php 
        $go=$_GET['callback'];  // 获取callback的值
        echo $go.'({a:"1"})';  // 输出回调函数
   ?>

 */
function jsonp(params) {
    params = params || {};
    if (!params.url)return;

    params.data = params.data || {};
    //设置响应时间，默认为15秒（后台为3秒）
    params.timeout = params.timeout || 15000;

    //回调函数 可以设置为合法的字符串
    var callbackName = 'cb' + (~~(Math.random() * 0xffffff)).toString(16);

    var head = document.getElementsByTagName('head')[0];

    //设置传递给后台的回调参数名
    params.data[params.callback] = callbackName;

    params.data = formatParams(params.data);

    //创建script标签并加入到页面中
    var script = document.createElement('script');

    //发送请求
    script.src = params.url + '?' + params.data;

    head.appendChild(script);

    //数据响应回调函数
    window[callbackName] = function (res) {

        //删除之前插入的script（因为script只加载一次，并且此时已收到数据）
        head.removeChild(script);

        clearTimeout(script.timer);

        window[callbackName] = null;

        //成功回调
        params.success && params.success(res);

    };

    //为了得知此次请求是否成功，设置超时处理
    if (params.timeout) {

        script.timer = setTimeout(function () {

            //当响应时间超时后，使后续的函数不执行
            window[callbackName] = null;

            head.removeChild(script);

            //返回错误信息
            params.fail && params.fail({message: '超时'});

        }, params.timeout);

    }
}

//格式化参数
function formatParams(data) {
    var arr = [];
    for (var name in data) {
        //encodeURIComponent()用于对 URI 中的某一部分进行编码
        arr.push(encodeURIComponent(name) + '=' + encodeURIComponent(data[name]));
    }
    //添加一个随机数参数，防止缓存
    arr.push(('v=' + Math.random()).replace('.', ''));
    return arr.join('&');
}
```

### 优缺点
#### 优点
兼容性非常好，其原理决定了即便在非常古老的浏览器上也能够很好的被实现。
#### 缺点
1. 只能 GET 不能 POST，因为是通过script引用的资源，参数全都显式的放在URL里，和AJAX没有关系。
2. 存在安全隐患，动态插入script标签其实就是一种脚本注入，XSS听过没？

### 应用场景
