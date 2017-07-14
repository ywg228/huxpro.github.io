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
 * 使用方式：
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
    arr.push(('v=' + random()).replace('.'));
    return arr.join('&');
}

// 获取随机数
function random() {
    return Math.floor(Math.random() * 10000 + 500);
}
``` 

## JSONP 
### 同源策略
AJAX之所以需要’跨域‘，就是因为浏览器的同源策略，即一个页面的AJAX只能获取这个页面相同源或者相同域的数据。
何为“同源”或者“同域”？——协议、域名、端口号都必须相同，如下：
``` 
http://example.com 和 https://example.com 不同，因为协议不同；   

http://example.com 和 http://example1.com 不同，因为域名不同；   

http://localhost:8080 和 http://localhost:1000 不同，因为端口不同；   

http://localhost:8080 和 https://example.com 不同，协议、域名、端口号都不同，
``` 
