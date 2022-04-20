---
title: "css的一些知识扩展"
categories: [ "技术" ]
tags: [ "css" ]
draft: false
slug: "44"
date: "2021-06-23 09:16:00"
---

box-sizing 盒类型

该属性是告诉浏览器是以什么盒模型展示的

IE用的是border-box

计算方式是外边距+内边距+内容=宽度（高度）

而像谷歌浏览器之类的用的是content-box

计算方式是容器的宽度或者高度

box-sizing: content-box; // 告诉浏览器是以content-box方式计算

border-box // 告诉浏览器是以border-box方式计算


---


css样式优先级

!important > 内联 > id > 类 > 标签 > 通配符 > 默认样式 > 继承样式




---

letter-spacing 字间距

该属性控制字符之间的距离，字符之间的字符间距，默认值为0

该属性支持三种类型的数值

px（像素）

em（相对值，相对于原来设置的值，如果原来的值为16px，那么1em就是16px，可以理解为倍数）

rem（和em类似，不过它相对的是html元素，而em是相对于它本身）

例如：

letter-spacing: 6px;


---

columns

用于指定列宽和列数

例如：

columns: 100px 3; // 指的是列宽为100px，列数为3


column-gap

用于列与列之间的间隔

column-gap: 30px;


column-rule

用于指定列之间的宽度和样式，以及颜色（列的边线）

column-rule: 6px dashed #ccc;

可拆分为

column-rule-width和column-rule-style和column-rule-color


column-span

指定元素应横跨多少列

column-span: 3;

all为横跨所有列



---

media 媒体查询

媒体查询常用于响应式布局，为不同屏幕设置不同的样式

@media(max-width:768px){} // 当屏幕最大宽度只有768px时应用其下的设置

检测是否有将系统的主题色设置为亮色或者暗色（安卓开启深色模式也有效，可以用于定义夜间模式等等）

@media (prefers-color-scheme: dark){}  // 暗色

@media (prefers-color-scheme: light){}  // 亮色

---

only元素选择

.app:only-child // 选择唯一性子元素的.app类元素（必须是唯一性）


.app:only-of-type // 选择指定类型（.app）的子元素（只要是父元素下有.app类就会选择）


---

before和after插入（伪类）

.app:before // 在app类的内容之前插入新的内容

.app:after // 在app类的内容之后插入新的内容

---

nth 元素选择（伪类）

.app:nth-child(2) // 选择app类的父元素下的第2个子元素

.app:nth-last-child(2) // 选择app类的父元素下的第2个子元素中的带有.app元素

.app:nth-of-type(2) //  选择属性app类的父元素下的第2个子元素中的全部同类型的元素
 
.app:nth-last-of-type(2)  // 选择app类的父元素下倒数的第2个子元素中的带有.app元素

---

calc 计算属性

动态计算长度

width: calc(80% + 100px); // 宽度等于父元素的80%宽度加上100px

---

css单位：px（像素），em（相对于自身元素），rem（相对于根元素，html元素），vw（1vw等于视口宽度的1%，100vw就是视宽100%），vh（1vh等于视口高度的1%，100vh就是视高的100%）


注意：vw，vh是相对于视口，包括滚动条，而100%不包括滚动条，因此100%和100vw（vh）有可能是不一致的

---


IFC全称Inline Formatting Contexts，中文翻译为行内格式化上下文

只要块级元素中仅包含内联级元素就会触发IFC机制，例如：

    <div id="app">
        <span>
            hallo
        </span>
    </div>


特性：

可以根据IFC容器的属性值，对子元素进行排列，例如text-align: center;

子元素使用float浮动的优先排列

上下外边距不生效，但是左右外边距生效，IFC机制就是只计算横向样式，不计算纵向样式

IFC机制常用于文本行高不同，元素垂直显示的问题



---


缓动动画公式：(目标值 - 现在的位置) / 10


---


css3原生变量


声明变量

    #app{
        --textmax: 10px;
    }


textmax变量（属性）目前没有任何含义，因此，css变量也叫做css自定义属性，变量名大小写敏感


var()函数用于读取变量的值，例如：


    #app{
        font-size: var(--textmax, 13px);
    }


var()的第二个参数是表示变量的默认值，如果这个变量不存在，就使用这个默认值


注意：和less那些css预处理器不同，css原生变量只能用于属性值，不能用于属性名

如果变量值是字符串，可以进行拼接，而且变量带有单位，不可写为字符串

如果变量值是数值，需要calc()函数才能进行拼接，例如：


    #app{
        font-size: calc(var(--textmax, 13px) + 10px);
    }



作用域

css变量的优先级和css选择器的优先级是一样的， id > 类 > 标签 > 通配符



---

粘性布局

position: sticky;

该属性值还处在实验性期间，因此兼容性差的不堪入目

    position: -webkit-sticky;
    position: sticky;
    top: 20px;

当元素距离视窗顶部的距离大于20px的时候，以relative定位显示，当小于20px的时候，以fixed定位


触发生效要有top, right, bottom 或 left其中一个值

注意：当left和right同时设置时，left的优先级高，当top和bottom同时设置，top的优先级高


而且使用sticky的元素的父节点的overflow属性必须是visible，因为如果设置hidden，那么就无法滚动，就无法生效了，而设置其他relative|absolute|fixed会导致其相对于父元素，而不是相对于视窗定位了




动画间隔时间公式，因为大多数显示器默认频率是60Hz，即1秒刷新60次，因此最小间隔为1/60*1000ms = 16.7ms

---


Bootstrap

由twitter提供，支持Sass变量，v5版本移除JQuery依赖（所以使用v5版本不需要再导入jQuery了）


使用简单，直接用类名覆盖样式，就可以使用现成由Bootstrap提供的样式

Bootstrap响应式布局用的@media（media query / 媒体查询）

如果不喜欢部分Bootstrap的样式，也可以自定义（使用sass修改变量）


---


css Modules（class默认局部生效，解决css冲突）

CSS Modules不是官方规范，也不是浏览器提供的特性，而是依赖于构建工具（例如webpack）

class类名是动态生成的，唯一的，并且准确地对应源码的类样式，样式复用通过composes关键字实现

例如：

    .test{
        color: #ccc;
    }
    .demo{
        composes: test;
        box-sizing: border-box;
    }



---


layout viewport，visual viewport，ideal viewport



layout viewport是网页布局的区域，因此layout viewport可能大于视窗区域，也可能小于，当缩放为100%时，layout viewport宽度等于视窗的宽度，一般来说不会发生改变，除非布局元素被清除了


visual viewport是视窗区域，这个区域随缩放改变

ideal viewport：每个设备的ideal viewport有可能不同，可以理解为设备区域，这个可以手动设置，例如：

    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">

device-width为视窗大小，也就是visual viewport

上面例子在100%缩放中，ideal viewport的大小（不是指单位）等于设备物理大小

ideal viewport可以通过执行window.screen.width/window.screen.height获得

在我这里宽度为1463px，那么表示无论设备的物理像素多大，在这里就是用1463px表示


例如：

    <div id="app"></div>

    body{
        padding: 0;
        margin: 0;
    }
    #app{
        width: 1463px;
        height: 10px;
        background-color: #000;
    }

可以发现app元素布满了视窗宽度，说明在该设备中css的1463px等于100%


css的1px不等于设备中的1px，css的1px和设备的尺寸相关，和视窗的缩放有关（实质上都和devicePixelRatio有关）

devicePixelRatio（比例） = device pixels（设备物理像素） / device-independent pixels（设备独立像素，例如css的像素）

devicePixelRatio比例可直接通过执行window.devicePixelRatio得到（在我这边为1.75，说明css的1px等于设备的1.75px）




