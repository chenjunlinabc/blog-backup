---
title: "CSS预处理器-Sass学习笔记"
categories: [ "默认" ]
tags: [ "Sass" ]
draft: false
slug: "51"
date: "2021-07-02 08:44:00"
---

sass是一门css扩展语言，css预处理器


sass是基于ruby语言开发，在前端中可以使用npm安装

npm install node-sass或者npm install node-sass --save-dev

当然也可以安装ruby，通过gem托管服务进行安装sass

gem install sass


文件

hallo.scss


解析为css文件

sass hallo.scss hallo.css


sass提供了4种编译风格


nested：嵌套缩进的css代码，默认值

expanded：没有缩进，扩展的css代码

compact：简洁格式的css代码

compressed：压缩后的css代码，生产环境一般选择这个

编译风格使用语法

sass --style nested hallo.scss hallo.css

也可以让sass自动编译

监听文件

sass --watch hallo.scss:hallo.css


监听目录

sass --watch sass/style.scss:css/style.css


注意：目录不要有中文


---

注释

/\* 注释 \*/

// 注释

/\*! 注释 \*/

第一种注释，会保留到编译后的css文件中

第二种注释，不会编译到编译后的css文件中

第三种注释，是表示重要注释，就算就是压缩编译，也会保留这个注释，一般用来声明版权

变量

    $color: #ccc;
    #app{
        color: $color;
    }

变量也可以嵌套到属性中

    $right: right;
    #app{
        margin-#{$right}: 50px;
    }

运算

允许使用加减乘除运算

    #app{
        width: 100px + 20px;
        height: (300px / 3) * 2;
    }


嵌套，嵌套一般用在有同一个父元素（祖先元素）下，例如：

    .nav{
        padding: 100px;
        .nav-ul{
            right: left;
            .nav-li{
                margin: 10px;
            }
        }&:hover{
            color: #ccc;
        }
    }

伪类或者伪元素需要在其前面加&


继承

sass可以将一个样式集继承到另一个继承集中

    .container{
        width: 100px;
    }
    #app{
        @extend .container;
        height: 100px;
    }


混合

    @mixin width{
        width: 100px;
    }
    #app{
        @include: width;
    }


混合和继承的区别就是，混合可以指定参数

    @mixin width($nums: 60px){
        width: $nums;
    }
    #app{
        @include: width(100px);
    }


函数

提供了一下函数来处理颜色之类的


grayscale(#333)



导入外部文件

导入scss文件
@import "scss/hallo.scss";

导入css文件
@import "css/hallo.css"



条件判断

    #app{
        @if 100 < 60 {  
            width: 100px;
        } @else {
                width: 60px;
        }
    }

循环

for

    @for $i from 1 to 6 {
        .col-#{$i} {
            border: #{$i}px solid #ccc;
        }
    }




while

    $i : 6;
    @while $i > 0 {
        .col-#{$i} {
             width: 100px * $i;
        }
        $i : $i  - 1;
    }


each

    @each $nums in 1,2,3 {
        .col-#{$nums} {
            background-image: url("/image/#{$nums}.jpg");
        }
    }



自定义函数


    @function hallo($i) {
        @return ($i * 2) + 100;
    }
    #app {
        width: hallo(10px);
    }










