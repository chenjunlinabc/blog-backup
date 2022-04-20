---
title: "JavaScript-IntersectionObserver构造函数笔记"
categories: [ "默认" ]
tags: [ "JavaScript" ]
draft: false
slug: "16"
date: "2021-06-16 09:29:56"
---



IntersectionObserver是浏览器本身提供的构造函数，因此可能有一些老版本浏览器没有效果

该构造函数提供了一种异步的监测目标对象和祖先对象或者视口相交的方法

var observe = new IntersectionObserver(callback, options)

例如上面，该函数可以传入两个参数，callback是当可视性发生改变时执行回调函数，options是配置对象

使用该构造函数生成的实例中有3个观察器实例，分别是observe（开始监测），unobserve（停止监测），disconnect（关闭监测），其中observe的参数是dom对象


当监测目标对象的可视性发生改变时调用callback参数中的回调函数

options参数：主要是设置观测的对象和观测值，该参数中有三个键值对

root指的是观测对象的根元素，默认是浏览器视口，值要么是根元素，要么就是观测对象的父元素

rootMargin指的是用于扩大或者缩小视口的大小

threshold指的是交叉的比例，主要决定什么时候触发回调函数，是数组，默认值为0


callback参数中的回调函数一般会被调用两次，一次是当监测对象可视性满足了threshold指定的值，还有一次就是监测对象不满足threshold指定的值



IntersectionObserverEntry对象

该对象提供了监测对象的信息，有七个属性



boundingClientRect：返回目标的矩形信息

intersectionRatio：返回相交时和目标的比例值，不可视时小于等于0

intersectionRect：返回目标和视口相交的矩形信息

isIntersecting：返回目标当前是否可视，可视为true（返回值为布尔值）

rootBounds：返回根元素的矩形信息，没有指定根元素则返回当前视口的矩形信息

target：返回观测的目标对象，是dom对象

time：返回一个记录了从观测开始到交叉被触发时间的的时间戳，单位为毫秒


如果是搞懒加载，那么intersectionRatio和isIntersecting是关键点

例如：

    const lazyload = new IntersectionObserver((target)=>{
        // 实例化
        target.forEach((item) =>{
            if (item.intersectionRatio){
                // 当目标可视
                item.target.src = item.target.alt; // 进行属性值覆盖
                lazyload.unobserve(item.target) // 停止观测
            }
        })
    },{
        rootMargin: "100px" // 提前100px
    }); 
    const imgs = document.querySelectorAll("img[alt]"); // 选择带有alt属性的img元素
    imgs.forEach((item) => {
        lazyload.observe(item)
        // 开始观测
    });