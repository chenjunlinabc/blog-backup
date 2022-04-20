---
title: "JavaScript-XMLHttpRequest对象笔记"
categories: [ "默认" ]
tags: [ "JavaScript" ]
draft: false
slug: "14"
date: "2021-06-16 09:25:49"
---

ajax请求是异步的，因此可以通过回调函数来处理响应

实现ajax请求大多是使用XMLHttpRequest对象，该对象用于与服务器交互，可以在不刷新页面的情况下请求url，获取数据，从而达到更新页面内容的目的

初始化XMLHttpRequest()构造函数，可以获得一个XMLHttpReques实例，例如：

    var xmlhttp = new XMLHttpRequest()
    xmlhttp.onreadystatechange = function(){
        if(xmlhttp.readyState == 4 && xmlhttp.status == 200){
            document.getElementById("app").innerHTML = xmlhttp.responseText
        }
    }
    console.log(xmlhttp.readyState) // 0
    xmlhttp.open("GET","https://httpbin.org/get",true)
    console.log(xmlhttp.readyState) // 1
    xmlhttp.onprogress =function(){
        console.log(xmlhttp.readyState) // 3
    }
    xmlhttp.onload = function(){
        console.log(xmlhttp.readyState) // 4
    }
    xmlhttp.send();



XMLHttpReques实例的属性

XMLHttpRequest.readyState：该属性会返回一个XMLHttpRequest的状态，状态有5种，例如：

状态0：已被实例化，但是未调用open()方法
状态1：open()方法已被调用（连接）
状态2：send()方法已被调用（请求）
状态3：请求处理中
状态4：请求完毕，且响应已就绪

XMLHttpReques.onreadystatechange：该属性对应了一个回调函数，当XMLHttpRequest.readyState属性发生改变时，该回调函数就会被调用，例子如上面所示

XMLHttpRequest.response：该属性会返回一个类型，该类型取决于XMLHttpRequest.responseType的值，类型例如：

DOMString：当XMLHttpRequest.responseType的值为空字符串，那么就是DOMString类型（是一个utf-16字符串，默认）

arraybuffer：XMLHttpRequest.responseType的值为存储二进制数据的ArrayBuffer对象（该对象是用于存储二进制数据，不能直接进行操作，只能通过视图来进行操作）

Blob：XMLHttpRequest.responseType的值为包含二进制数据的Blob对象（该对象是用于表示一个类似文件的对象，可以通过二进制的方式进行读取）

Document：值是一个Document

json：值是一个JavaScript对象

text：值是一个DOMString对象表示的文本（utf-16字符串）

XMLHttpRequest.responseText：该属性的值是请求被发送到服务端后，从服务端返回的文本，如果值为null，那么就是请求失败，如果为空字符串，那么就是没有send()

XMLHttpRequest.responseType：该属性会返回一个值，该值和response属性的值一样

XMLHttpRequest.responseURL：该属性会返回一个序列化url，如果url为空那么就返回空字符串

XMLHttpRequest.responseXML：该属性返回Document(html/xml)，如果请求没有成功或者获取的数据，无法解析为html或者xml，那么为null

XMLHttpRequest.status：该属性会返回响应中的http状态码，如果请求没有完成，那么值为0，如果出错也是为0

XMLHttpRequest.timeout：该属性会返回一个值，该值为请求被自动终止前所消耗的毫秒数（默认为0，则表示没有超时）

XMLHttpRequest.upload：该属性是用于表示上传的进度，可搭配事件监听器来追踪进度，例如：

onloadstart：开始获取数据
onprogress：数据正在传输中
onabort：数据获取终止
onerror：数据获取失败
onload：数据获取成功
ontimeout：数据获取操作在规定的时间内未完成
onloadend：数据获取完成（不管是否成功）


---

open()方法

该方法有3个常用参数，请求方式，请求url，是否异步执行（如果为false，那么JavaScript会等到服务器响应完毕时才会继续执行）

xmlhttp.open("GET","https://httpbin.org/get",true)