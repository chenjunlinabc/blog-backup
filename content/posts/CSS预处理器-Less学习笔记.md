---
title: "CSS预处理器-Less学习笔记"
categories: [ "默认" ]
tags: [ "Less" ]
draft: false
slug: "50"
date: "2021-07-01 15:31:00"
---

less是一门css扩展语言，是css预处理器，扩展了css的动态特性，在css语法基础上引入了变量，混合，运算和函数等功能

常见的css预处理器有sass，less，stylus

less开发手册：https://less.bootcss.com/



安装less

npm install -g less


检查是否安装成功

lessc -v

或者使用CDN和src导入的方法也都是可以的

例如：

https://cdn.jsdelivr.net/npm/less@4.1.1/dist/less.min.js


less文件

hallo.less



注释

/\*块注释\*/

// 行注释

变量



变量名命名规范

必须是以@开头

不能包含特殊字符

不能以数字开头

大小写敏感

例如：

    @color: #ccc;

    #app{
        background-color: @color;
    }




less编译

less提供了一个解析器，通过解析器，编译成css文件

例如：

npm

lessc style.less style.css

将style.less编译成style.css文件

或者

vsc插件：Easy LESS（修改保存就自动编译成css文件）


less嵌套


子元素嵌套到父元素上（后代选择器）

    #app{
        width: 100px;
        div{
            width: 60px;
        }
    }


伪类或者伪元素选择之类的，需要加&连接起来，不加则认为是后代

    #app{
        width: 100px;
        a{
            color: #000;
            &:hover{
                color: @color;
            }
            &::before{
                 width: 30px;
            }
        }   
    }


less运算

数值，颜色，变量都可以参与运算，加减乘除


     @width: 50px;
     #app{
        width: 100px - 10;
        height: 100px * 3;
        div{
            width: (@width / 30) * 6;
        }
    }




    #app{
        color: #666 - #333;
    }

上面得到的是#333

注意：

如果有两个不同单位的值运算，运算结果单位取第一个值的单位

如果有两个值，它们只要一个单位，取该单位作为运算结果单位

运算符中间左右两边要有空格分开


混合

混合（Mixin）可以将一组属性从包含到另一个集合中，例如：

    .container{
        max-width: 980px;
        margin-left: auto;
        margin-right: auto;
    }
    #app{
        .container
    }


转义

转义允许使用任意字符串作为属性或者变量值，例如：



    @max768: ~"(max-width: 768px)"~
    .nav-svg{
        @media @max768{
            display: none;
        }
    }



函数

less内置了多个函数，用于算术运算，颜色处理等等


    @width: 0.6;
    #app{
        width: percentage(@width); // 60%
    }



映射

映射支持less3.5以上版本，例如：

    #data(){
        width: 100px;
    }
    #app{
        width: #data[width];
    }






