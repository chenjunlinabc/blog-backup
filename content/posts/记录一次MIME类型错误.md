---
title: "记录一次MIME类型错误"
categories: [ "默认" ]
tags: [ "nginx" ]
draft: false
slug: "39"
date: "2021-06-17 08:25:00"
---

打开网页，发现css没有加载出来，f12一看，content-type: text/plain;，把这个css当文本输出了，浏览器请求到这种类型的文件都不会对其进行处理，而且应该是text/css才对


先了解一下浏览器是如何处理这些数据的，是怎么区分的

数据通过http传输协议获取到，然后由web服务器的content-type向浏览器进行指示数据的类型，而mime.types就是用来定义数据文件的类型，用什么格式来进行网页编码（charset=utf-8）

当web服务器接收到请求时，会依据请求文件的后缀名在服务器的MIME配置文件中找到对应的mime.types，然后根据mime.types来确定content-type，浏览器根据content-type来处理数据



解决方法：当然是指定mime.types文件，而宝塔的nginx一般是在/www/server/nginx目录下，而mime.types文件一般在nginx目录下的conf目录下，会看到一个叫mime.types的文件和一个叫mime.types.default的文件



往nginx配置文件上输入    
include /www/server/nginx/conf/mime.types;
default_type application/octet-stream;


第一个行指定mime.types，第二行就是默认类型


然后重启一下nginx服务器，刷新一下网页，看到恢复成content-type: text/css了