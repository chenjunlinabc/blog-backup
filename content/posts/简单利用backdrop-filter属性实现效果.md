---
title: "简单利用backdrop-filter属性实现效果"
categories: [ "技术" ]
tags: [ "css" ]
draft: false
slug: "65"
date: "2021-08-06 10:18:00"
---

backdrop-filter是css原生的属性

backdrop-filter的一些方法（用法和filter一样）

blur：模糊
brightness：亮度
contrast：对比度
invert：反相
opacity：透明度
saturate：饱和度
drop-shadow：投影
grayscale：灰度
hue-rotate：色调变化
sepia：褐色


---


简单实现一个毛玻璃背景效果，例如：



    <style>
        *{
            margin: 0;
            padding: 0;
        }
        #app{
            width: 100%;
            height: 50rem;
            background-image: url("1.jpg");
        }
        #test{
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            backdrop-filter: blur(10px);
        }
        .text{
            padding-top: 100px;
            text-align: center;
        }
    </style>
    <div id="app">
        <div id="test">
            <p class="text">
                hallo word
            </p>
        </div>
    </div>



---



backdrop-filter和filter区别：

filter是作用于当前元素（效果体现在本身，而不是背景），而且后代也会继承该属性

backdrop-filter是作用于当前元素背后的所有元素，不会影响自己

backdrop-filter兼容性没有filter优秀（目前低版本浏览器和IE，火狐都不支持该属性）
