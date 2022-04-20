---
title: "css常见布局"
categories: [ "默认" ]
tags: [ "css" ]
draft: false
slug: "24"
date: "2021-06-16 13:23:00"
---

居中布局

分水平居中和垂直居中，水平+垂直居中


水平居中：当前元素在父级元素容器中，水平方向是居中显示的


inline-block+text-algin

    #app{
        text-align: center;
        // 父元素，文本内容居中对齐
    }
    .child{
        display: inline-block;
        // 子元素，行内块级元素
    }

优点：浏览器兼容性好（css2）

缺点：text-align具有继承性，会导致子元素的文本也是居中的


table+margin

    .child{
        display: table; // 也可以为block
        margin: 0 auto;
         // 子元素，margin外边距，上下为0，左右为auto（浏览器自动分配）
    }

优点：只需要对子元素设置，就可以实现效果

缺点：如果子元素脱离正常流，将会导致margin属性的值无效化



absolute+transform


    #app{
        position: relative; // 父元素相对定位
    }
    .child{
        position: absolute; // 子元素绝对定位，如果父元素没有定位，那么该元素是相对于页面定位，父元素定位了，那么该元素是相对于父元素的
        left: 50%; // 相对于父元素左边50% 
        transform: translateX(-50%); // 子元素水平平移-50%（左负数，右正数）
    }


优点：父元素是否脱离正常流，也是不影响子元素的水平居中效果

缺点：transform属性是css3的新属性，浏览器兼容性比较差




垂直居中：当前元素在父级元素容器中，垂直方向是居中显示的


table-cell+vertical-algin

    #app{
        // 父元素
        display: table-cell;
        vertical-align: middle; // 设置文本的垂直方向对齐方式
    }



优点：浏览器兼容性好

缺点：vertical-align属性具有继承性


absolute+transform

    #app{
        position: relative;
    }
    .child{
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
    }



优点：父元素是否脱离正常流，也是不影响子元素的垂直居中效果

缺点：transform属性是css3的新属性，浏览器兼容性比较差




水平垂直居中：水平方向居中，也要垂直方向居中

table+margin（水平），table-cell+vertical-algin（垂直）


absolute+transform

    #app{
        position: relative;
    }
    .child{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }



多列布局（两列布局+三列布局+CSS3 多列布局）


两列布局：其中有一列是确定宽度，而另外一列则是自动填满（指定宽于自适应）


float+margin


    .left{
        width: 100px;
        float: left;
    }
    .right{
        margin-left: 100px;
    }

优点：简单

缺点：自适应元素的margin属性值要和定宽元素的width属性值保持一致

或者给定宽元素position: relative，自适应元素给float: right和width: 100%,margin-left: -100px



float+overflow

    .left{
        width: 100px;
        float: left;
    }
    .right{
        overflow: hidden;
    }

优点：简单

缺点：overflow属性会导致内容溢出（BFC）



display：table


    #app{
        display: table;
        table-layout: fixed;
    }
    .left{
        width: 100px;
    }
    .left,.right{
        display: table-cell;
    }


优点：浏览器兼容性好


缺点：父元素的display属性设置为table，会受到制约


三列布局：三列，有两列是确定宽度，另外一列自动填满（指定宽于自适应），一般中间那一列是内容区

效果实现和二列一样


圣杯布局

头部和底部都占页面100%宽度，高度固定，中间区域是一个三栏布局，三栏布局两侧宽度确定，中间部分自动填充（一般中间那是内容区）

中间部分一般是三栏中最高的（因为是内容区）


float

头部和底部填满宽度，三列设置浮动和相对定位，中间部分的放在前面

三列设置overflow: hidden和padding-left（值为left的宽度），padding-right（值为right的宽度）

left栏设置margin-left: -100%,就返回到左侧了，然后left，值为负的left高度

right栏right，值为负的right高度，margin-left，值为负的right高度


例如：

    <style>
        header{
            max-width: 100%;
            height: 100px;
            background-color: aqua;
            text-align: center;
        }
        footer{
            max-width: 100%;
            height: 100px;
            background-color: aquamarine;
            text-align: center;
        }
        main{
            overflow: hidden; 
            padding-left: 20%;
            padding-right: 20%;
        }
        main div{
            float: left;
            position: relative;
            text-align: center;
        }
        .center{
            width: 100%;
            height: 600px;
            background-color: blue;
        }
        .left{
            height: 300px;
            width: 100px;
            background-color: azure;
            margin-left: -100%;
            left: -100px;
        }
        .right{
            height: 300px;
            width: 100px;
            background-color: bisque;
            right: -100px;
            margin-left: -100px;
        }
    </style>
    <header>
        头部
    </header>
    <main>
        <div class="center">
            中间
        </div>
        <div class="left">
            左
        </div>
        <div class="right">
            右
        </div>

    </main>
    <footer>
        底部
    </footer>
    


flex

用flex弹性盒子比较简单一点，只需要给三列设置flex，按照左中右排列，中间部分设置flex: 1;

优点：简单，不需要设置dom

缺点：（flex）浏览器兼容性比较差



双飞翼布局

双飞翼布局和圣杯布局效果和要求类似，侧边宽度确定，中间自适应，不同的是，圣杯布局是在父元素设置内边距，两侧通过定位和浮动来插入到中间，而双飞翼布局是直接设置一个dom来放置三列


三列设置左浮动，中间设置宽度为100%，left设置外边距为负100%，right设置外边距为其本身宽度，中间设置外边距为两侧腾出位置，值为left和right的宽度

例如：

    <style>
        header{
            max-width: 100%;
            height: 100px;
            background-color: aqua;
            text-align: center;
        }
        footer{
            max-width: 100%;
            height: 100px;
            background-color: aquamarine;
            text-align: center;
        }
        main{
            text-align: center;
            overflow: hidden;
        }
        
        .center{
            width: 100%;
            height: 600px;
            background-color: blue;
            float: left;
        }
        .center div{
            margin: 0 100px 0 100px;
        }
        .left{
            height: 300px;
            width: 100px;
            background-color: azure;
            float: left;
            margin-left: -100%;
        }
        .right{
            height: 300px;
            width: 100px;
            background-color: bisque;
            float: left;
            margin-left: -100px;
        }
    </style>
    <header>
        头部
    </header>
    <main>
        <div class="center">
            <div>
                中间
            </div>
        </div>
        <div class="left">
            左
        </div>
        <div class="right">
            右
        </div>

    </main>
    <footer>
        底部
    </footer>
    

其他布局


等分布局：子元素平均分配父元素的宽度


等高布局：子元素在父元素中高度相等



全屏布局：一般是一个顶部，一个底部，中间是两栏（一个侧边栏，一个主栏）

侧边栏一般为确定宽度的，主栏为自适应，顶部和底部是占页面100%宽度，一般会设置外边距来分开

吕形布局：就是页面分为两个容器，一般第一个容器是导航栏，第二个容器是内容区，常见于移动端布局

九宫格布局