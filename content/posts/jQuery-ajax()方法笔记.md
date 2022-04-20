---
title: "jQuery-ajax()方法笔记"
categories: [ "默认" ]
tags: [ "JavaScript","jQuery" ]
draft: false
slug: "13"
date: "2021-06-16 09:24:37"
---

ajax()是jQuery中定义的一个方法，该方法用于执行ajax请求，例如：

    $(document).ready(function(){
        $("button").click(function(){
            $.ajax({
                type: "GET",
                url: "https://httpbin.org/get",
                success: function(getdata){
                    console.log(getdata)
                }
            })
        })
    });


参数

url：指定发送请求的URL，默认是当前页面

type：指定请求方式（GET或者POST）

success：当请求成功时执行的函数

data：指定要发送到服务端的数据

dataType：预期服务端响应过来的数据类型

async：指定请求是否异步（布尔值）

beforeSend：在发送请求之前执行的函数

cache：指定客户端是否缓存被请求页面，默认是true（布尔值）

complete：在请求完成时执行的函数（不管是否发送成功）

contentType：指定要发送到服务端时使用的内容类型

context：指定所有ajax相关的回调函数规定this值

dataFilter：指定用于处理ajax返回的原始响应数据的函数

error：指定请求失败时执行的函数

global：指定请求是否触发全局ajax事件，默认为true

ifModified：指定是否在最后一次请求

jsonp：指定一个jsonp请求中重写回调函数的字符串

jsonpCallback：指定一个jsonp回调函数的名称

processData：指定是否将请求发送的数据转换为查询字符串，默认为true

scriptCharset：指定请求的字符集

timeout：指定请求超时时间（单位：毫秒）

traditional：指定是否使用传统的方式来序列化数据

username：指定响应http访问认证请求的用户名

password：指定响应http访问认证请求的密码

xhr：用于重写或者增强XMLHttpRequest对象的函数