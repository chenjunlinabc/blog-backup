---
title: "Ajax学习笔记"
categories: [ "学习" ]
tags: [ "JavaScript" ]
draft: false
slug: "54"
date: "2021-07-03 15:30:00"
---

ajax是浏览器提供的一套方法，可以实现页面无刷新更新数据

关于XMLHttpRequest对象（构造函数）的用法：https://xiaochenabc123.test.com/archives/14.html


ajax需要网站环境下生效，需要web服务器，nodemon

app.js

    // 导入express
    const express = require("express");
    // 导入path
    const patg = require("path");
    // 创建web服务器
    const app = express();
    // 静态资源访问服务
    app.use(express.static(path.join(__dirname,"public")));
    app.get("/hallo",(req, res) = >{
        res.send("hallo");
    });
    // 监听端口
    app.listen(3000);
    console.log("服务器启动成功")


nodemon app.js


ajax运行原理

ajax代理浏览器发送请求和接收响应，达到局部更新页面数据的效果

创建ajax对象

var hallo = new XMLHttpRequest();

请求方式和请求地址

hallo.open("GET","https://httpbin.org/get")

发送请求

hallo.send();

获取服务端给予客户端的响应数据，因为请求和获取数据的速度取决于网络速度，应该设置onload事件，当加载完毕了再获取数据

    hallo.onload = function(){
        console.log(hallo.responseText)
    }

服务端大部分情况下用json对象作为响应数据的格式，通过拼接json数据和html，将拼接的结果显示在页面中

在http请求与响应的过程中，请求参数或者响应内容，如果是对象类型，最后都会转换为对象字符串进行传输，例如：

    app.get("/hallo",(req, res) = >{
        res.send({"name" : "root"});
    });


json字符串转换为json对象

    var responseText = JSON.parse(hallo.responseText);
    console.log(responseText)
    var str = "<div>"+ responseText.name +"</div>";
    document.body.innerHTML = str;


请求参数传递

get请求

hallo.open("get","https://httpbin.org/get?name=hallo&pass=123")

var hallo = "name="+ names + "&pass=" + pass


表单的数据获取

hallo.value


post请求参数

post请求参数是保存到请求头中的报文，不像get是通过链接参数传输的

hallo.setRequestHeader("Content-Type","application/x-www-forn-urlencoded")

hallo.send("name=root&pass=123")

post请求必须设置请求参数格式的类型

post请求需要body-parser模块

const bodyParser = require("body-parser")



json

Content-Type属性：application/json

{name: "root","pass": "123"}

将json对象转换为json字符串

JSON.stringify()  












ajax错误处理

获取http状态码

hallo.status()






