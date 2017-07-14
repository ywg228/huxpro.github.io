---
layout:     post
title:      JavaScript  AJAX和JSONP升
subtitle:   学习笔记 
date:       2017-07-14
author:     Ywg
header-img: img/home-bg.jpg
catalog:    true
tags: JavaScript 
---

## Ajax 
一种请求数据的方式，不需要刷新整个页面；核心是 XMLHttpRequest 对象 <br>
#### AJAX请求步骤
1. 创建XMLHttpRequest对象
2. 连接服务器
3. 发送请求
4. 接收响应数据

#### AJAX的简单封装
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
    //请求方式 默认GET
    params.type = (params.type || 'GET').toUpperCase();
    // 避免有特殊字符，必须格式化传输数据
    params.data = formatParams(params.data);

    //1.创建XMLHttpRequest对象
    var xhr = null;
    if(window.XMLHttpRequest) {  //非IE6
        xhr = new XMLHttpRequest();
    }else { //IE6及其以下版本浏览器
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }

    //2.连接服务器并发送请求
    if(params.type == 'GET') {
        //连接服务器 三个参数：请求方式、请求地址(get方式时，传输数据是加在地址后的)、是否异步请求(同步请求的情况极少)
        xhr.open('GET', params.url + '?' + params.data, true);
        //发送请求
        xhr.send(null);
    }else if(params.type == 'POST') {
        xhr.open('POST', params.url, true);
        //设置表单提交时的内容类型
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded;charset=UTF-8');
        //发送请求并传输数据
        xhr.send(params.data);
    }

    //3.接受响应数据
    xhr.onreadystatechange = function () {
        //readyState为4表示已接受到全部的响应数据
        if(xhr.readyState == 4) {
            //响应的HTTP状态码，以2开头的都是成功
            var status = xhr.status;
            if (status >= 200 && status < 300 || status == 304) {
                
                var response = '';
                // 判断接受数据的内容类型
                var type = xhr.getResponseHeader('Content-type');
                if(type.indexOf('xml') !== -1 && xhr.responseXML) {
                    
                    response = xhr.responseXML; //Document对象响应
                    
                } else if(type === 'application/json') {
                    
                    response = JSON.parse(xhr.responseText); //JSON数据
                    
                } else {
                    
                    response = xhr.responseText; //字符串数据
                    
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
    arr.push(('v=' + random()).replace('.', ''));
    return arr.join('&');
}

// 获取随机数
function random() {
    return Math.floor(Math.random() * 10000 + 500);
}
``` 
#### ajax请求是不能跨域的！

## JSONP 
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
那如何跨域请求？这就需要JSONP
#### 定义
一种跨域请求方式。<br>
主要原理是利用script标签可以跨域请求的特性，由其src属性发送请求到服务器，服务器返回JS代码，浏览器接受响应，然后就直接执行了，这和通过script标签引用外部文件的原理是一样的。
#### 组成
由两部分组成：回调函数和数据，回调函数一般是在浏览器控制，作为参数发往服务器端（当然，你也可以固定回调函数的名字，但客户端和服务器端的名称一定要一致）。当服务器响应时，服务器端就会把该函数和数据拼成字符串返回。 著作权归作者所有。
#### 请求步骤
1. 请求阶段：浏览器创建一个 script 标签，并给其src 赋值(类似 http://suggestion.baidu.com/su?wd=jsonp&cb=jsonpCallback）。
2. 发送请求：当给script的src赋值时，浏览器就会发起一个请求。
3. 数据响应：服务端将要返回的数据作为参数和函数名称拼接在一起(格式类似”jsonpCallback({name: 'abc'})”)返回。当浏览器接收到了响应数据，由于发起请求的是 script，所以相当于直接调用 jsonpCallback 方法，并且传入了一个参数。

#### JSONP的简单封装
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
            callback: 'cb', //
            //响应成功
            success: function (res) {
               console.log(res.s);
            },
            //响应超时
            fail: function (msg) {
                console.log(msg.message);
            }
        });
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
    arr.push(('v=' + random()).replace('.', ''));
    return arr.join('&');
}

// 获取随机数
function random() {
    return Math.floor(Math.random() * 10000 + 500);
}
```


