---
title: "css3学习笔记"
categories: [ "默认" ]
tags: [ "css" ]
draft: false
slug: "11"
date: "2021-06-16 09:21:00"
---

盒子模型

盒模型在css中是用来封装html元素的，因为本质上来看像个盒子一样，所以叫盒模型或者盒子模型，术语叫box model

外边距（margin）：一般用于控制同辈元素之间的间距

边框（border）：在内边距和内容之外的边框

内边距（padding）：内边框是用于控制内容的距离

内容（content）：内容，一般为文本和图像


例子：建立一个宽度为100px，10px的内边距，10px的外边距，10px的红色边框的盒子


    div{
        width: 100px;
        borde: 10px solid red;
        padding: 10px;
        margin: 10px;
    }


top，bottom，left，right分别代表了上，下，左，右

或者padding: 上，右，下，左，只有一个数字就表示上，下，左，右都是这个数值

有两个数值就是分别代表上下和左右，有三个数值表示上和左右和下


这个盒子大小应该是内容的宽度+内边距+边框+外边距


100+20+20+20=160



两种盒模型


IE 盒子模型（box-sizing content-box）（怪异盒模型）

IE盒模型的width与height是content、padding和border的总和

调用IE盒模型
box-sizing: border-box;



W3C标准盒模型（box-sizing content-box）


标准盒模型的width与height只含content，不包括padding和border

调用W3C标准盒模型
box-sizing: content-box;



在IE6，7，8浏览器中不声明DOCTYPE类型，那么在IE6，7，8浏览器中会将盒子模型解释为IE盒子模型

如果在页面中声明了DOCTYPE类型，那么所有的浏览器都会把盒模型解释为W3C盒模型

JavaScript获取盒模型的宽和高（这里的DOM不是指整个dom，而是某个元素）

DOM.style.width和DOM.style.height（只能获取内联样式）

DOM.currentStyle.width和DOM.currentStyle.height（只支持IE）

window.getComputedStyle(DOM).width和window.getComputedStyle(DOM).height（兼容性好）

获取元素相对于视窗的位置(top,right,bottom,left都可以获取)

let data = DOM.getBoundingClientRect()
data.top




---

有BFC特性的元素是一个被“隔离”的容器，该容器里面的元素不会在布局上影响到外面的元素


满足下面其中一个条件就触发BFC特性


body 根元素

浮动元素 float除none外的值

绝对定位元素  position (absolute、fixed)

display 为 inline-block、table-cells、flex

overflow 除了 visible 以外的值 (hidden、auto、scroll)


BFC的特性：

BFC是页面上的独立容器，该容器使用类型浮动之类会影响布局的元素都不会影响到外界的布局或者元素


例子：

1.当处于同一个BFC容器时，外边距会进行重叠，为了避免重叠，可以将其放在不同的BFC容器内



2.当容器内的元素使用的浮动，将其脱离了文档流，要触发容器的BFC，那么BFC容器要包含使用了浮动的元素



3.当第一个元素使用了浮动，希望第二个元素不被浮动元素影响，要在第二个元素上触发BFC特性

---


flex为Flexible Box 的缩写，意为"弹性盒模型"，为其盒模型提供足够大的灵活性

    div{
        display: flex;
    }


使用flex布局的元素，被称为flex容器，该元素的所有子元素为该容器的成员，称为flex项目




flex-direction

一般用到四个值

row：在水平方向上，起点在左
row-reverse：在水平方向上，起点在右
column：在垂直方向上，起点在上。
column-reverse：在垂直方向上，起点在下。


---
flex-wrap

一般用到三个值

nowrap：不换行
wrap：当容器宽度不足时换行，第一行在上方
wrap-reverse：当容器宽度不足时换行，第一行在下方

---
flex-flow是flex-direction和flex-wrap简写版，它们两个的属性都可以使用
例如flex-flow: row-reverse wrap-reverse;

表达了换行，第一行在下方，起点在右

---
justify-content

定义项目在主轴上的对齐方式

flex-star：左对齐

flex-end：右对齐

center： 居中

space-between：两端对齐，每个项目之间的间隔都相等

space-around：每个项目两侧的间隔相等，所以项目之间的间隔比项目与边框的间隔大一倍


---
align-items


flex-start：头部对齐
flex-end：底部对齐
center：居中对齐
baseline: 项目的第一行文字的基线对齐
stretch：当项目没有设置高度或设置为auto时，将占满整个容器的高度


---
align-content


flex-start：元素位于容器的开头
flex-end：元素位于容器的结尾
center：元素位于容器的中心
space-between：元素位于各行之间留有空白的容器内
space-around：元素位于各行之前、之间、之后都留有空白的容器内
stretch：默认值，元素被拉伸以适应容器


---


单独用于元素的属性

order
控制元素的顺序，数值越小排列就越靠前

order:1

---
flex-grow
控制元素的宽度

如果都为1，还有剩余的空间，那么将会等分剩余的空间

如果有一个为2，那么为2所占的剩余空间比为1的大1倍

flex-grow: 2

---

flex-shrink
控制元素宽度

如果都为1时，当空间不足时，将会等比例缩小

如果有一个为2时，那么当空间不足时

flex-shrink: 2

---
flex-basis
控制元素的初始宽度

\flex-basis: 100px
---

flex
控制元素都有相同的宽度，不会被内容的长度撑开
\flex: 1
---
align-self
控制元素单独对齐方式

align-self: auto | flex-start | flex-end | center | baseline | stretch;


---

背景

定位

在box盒子内部的背景默认是在盒子的内边距的左上角对齐

background-origin属性则规定background是相对于什么位置来定位的

    .box{
        width: 300px;
        height: 300px;
        padding: 10px;
        margin: 10px;
        background: url("hallo.jpg");
        background-origin: padding-box;
    }


background-origin属性值有3个，padding-box：相对于内边距定位，border-box：相对于边框定位，content-box：相对于内容区域


裁切

    .box{
        width: 300px;
        height: 300px;
        padding: 10px;
        margin: 10px;
        background: url("hallo.jpg");
        background-clip: padding-box;
    }


background-clip属性提供相对于什么位置进行背景裁切，该属性值和background-origin一样


背景尺寸

    .box{
        width: 300px;
        height: 300px;
        padding: 10px;
        margin: 10px;
        background: url("hallo.jpg");
        background-size: 100px 100px;
    }

background-size属性可以直接指定大小，也是可以cover或者contain填充

cover会按照原来的缩放比来填满整个盒子，而contain是按照原来缩放比，完整的填充盒子（不会填满）



边框


边框阴影

    .box{
        width: 300px;
        height: 300px;
        border: 1px solid red;
        box-shadow: 0px 0px 6px #ccc;
        border-radius: 6px;
    }

box-shadow，第一个值为阴影在水平方向的偏移量（正数为正方向，负数为反方向）

第二个值为阴影在垂直方向的偏移量（正数为朝下，负数为朝上）

第三个值为阴影的模糊度

第四个值为阴影的颜色


border-radius为边框圆角



边框图片

    .box{
        width: 300px;
        height: 300px;
        border: 1px solid red;
        border-image-source: url("hallo.jpg"); // 边框图片
        border-image-slice: 10; // 边框图片向内的偏移量
        border-image-repeat: round; // 边框图片的平铺方式
        border-image-width: 10px; // 边框图片的宽度
    }

border-image-repeat有4个常用值

stretch（默认值）：拉伸图形来填充

repeat：平铺（重复）图形来填充

round：如果无法完整平铺，那么就进行缩放来适应

space：如果无法完整平铺，那么就进行扩展来适应




文本阴影

text-shadow，有4个值，分别是水平方向的偏移量，垂直方向的偏移量，模糊度，阴影的颜色




伪类选择器


:first-child：选择父元素中的第一个子元素
:last-child：选择父元素中的最后一个子元素
:nth-child(n) ：选择父元素中正数第n个子元素
:nth-last-child(n) ：选择父元素中倒数第n个子元素


n的值必须要大于等于0，也可以是表达式


:target：选择被锚链接指向
::selection：选择被鼠标选中


例如：

:target{
background-color: #ccc;
}

<p>\<a href="#a">跳转至1</a></p>

...

<p id="a">1</p>

或者

p::selection:{
background-color: #ccc;
}

<p>xxx</p>

...

<p>xxx</p>


::first-line：选择第一行
::first-letter：选择第一个字符


例如：

div{
width: 100px;
height: 100px;
}
div::first-line{
background-color: #ccc;
}
div::first-letter{
background-color: #000;
}

<div>
sjdasfkdskbdsalkddsfjdslsakfbsdlss


xxx
test
</div>



过渡


	.box1 {
		width: 300px;
		height: 300px;
		background-color: red;
		transition-property: all; 
		transition-duration: 3s;
	}
	
	.box1:hover {
		width: 500px;
		height: 500px;
		background-color: #ccc;
	}



transition-property：表示哪些属性要参与到过渡动画效果，all表示全部

transition-duration：表示过渡执行时间

transition-delay: 设置过渡执行前等待的时间

transition-timing-function：过渡的速度类型，linear为相同速度，ease为开始慢速，变快，然后慢速结束，ease-in慢速开始，ease-out慢速结束，ease-in-out以慢速开始和结束，cubic-bezier开始到结束的不同速度过渡，用小数来表示进度，例如cubic-bezier(0.1, 0.3, 0.6, 0.1)




---



CSS3 border-image

border-image可以在元素的边框上绘制图像，规范要求使用borde-images时边框样式必须存在

border-image: url(./logo.jpg) 10 repeat

上面这行代表使用图片logo.jpg，裁剪位置10，使用重复方式



图片：使用url()调用，路径可以是绝对或者相对，也可以不用图片，即none


裁剪位置：默认单位为px，10表示10px，不需要写单位

支持使用百分百，例如300px\*100px的图片，30%就是裁剪图片90px\*30px的四个边的大小
也可以使用4个数，代表上右下左要裁剪的数值


重复方式只是其中一种方式，可以用于设置图像边界是否要重复或者拉伸，铺满

重复（repeat)，拉伸（stretch）,平铺（round）


stretch（拉伸）为默认值，所以会裁剪出来的图片比边框小，就会拉伸，以保证填满边框



可以使用两个参数，当使用一个参数时，水平方向以及垂直方向都使用这个参数

当使用两个参数时，第一个参数代表水平方向，第二个参数表示垂直方向



---

border-image还有4个子属性

指定要用于绘制边框的图像的位置

border-image-source


图像边界向内收缩

border-image-slice


图像边界的宽度

可以使用带单位的，百分百，不带单位的（倍数），auto

border-image-width


指定在边框外部绘制 border-image-area 的量

就是原本的位置向外拉伸

border-image-outset


用于设置图像边界是否要重复或者拉伸，铺满

border-image-repeat



---


## 

引入css
内联css：<div style="color: #fff"> // 要写在开始标签中

嵌入css：<style type="text/css">div{color: #fff;}</style>

外部css: <link href= "main.css" rel="stylesheet" type="text/css">

优先级：内联 > 嵌入 > 外部


---
选择器

标签选择器：div{}
id选择器：#main{}  <div id="main">
类选择器：.main{}  <div class = "main">
子选择器：.main>div{}
后代选择器：.main div{}
通用选择器：*{}
伪类选择器：a:{}
分组选择器：div,span{}



id选择器只能在文档中使用一次，类选择器可以使用多次

选择器的优先级是: 内联 > id > 类 > 标签 > 通配符

标签的权值为1

类选择器的权值为10

ID选择器的权值为100

伪类选择器是10

权值越大，优先级就越大


优先级最大的!important,例如：

div{color:#fff!important;}

---

样式
字体font-family
div{font-family:"Microsoft Yahei";}
字体大小font-size
div{font-size:16px;}
字体粗细font-weight
div{font-weight:bold;}
字体样式font-style
div{font-style:normal;}
字体颜色color
div{color:#fff;}
文本样式text-decoration
div{text-decoration:none;}
文本首行缩进text-indent
div{text-indent:2em;}
文本行间间距line-height
div{line-height:2em;}
增加或减少每一个文字的间距 letter-spacing
div{letter-spacing:10px;}
增加或减少每个英文单词之间的间距word-spacing
div{word-spacing:10px;}
文本对齐方式text-align
div{text-align:center;}




---

定位：
定位有两种，绝对定位和相当定位

相对定位和绝对定位都有四个属性

left（左），top(头)，right（右），bottom（底）

绝对定位：

设置绝对定位的元素会脱离文档流，所以不会占用空间

    div{
        position: absolute;
    }


绝对定位的元素的位置相对于元素最近的已定位的祖先元素，如果没有，那么相对于最初的包含块（也就是body）



相对定位：

相对定位的元素的位置相对于元素最初的位置，使用相对定位时，元素会占据原来的空间，所以，移动元素会覆盖其他元素

    div{
        position: relative;
    }


---
css有下面这些选择器

id选择器（#myid）
类选择器（.myclassname）
标签选择器（div,h1,p）
后代选择器（h1p）
相邻后代选择器（子）选择器（ul>li）
兄弟选择器（li~a）
相邻兄弟选择器（li+a）
属性选择器（a[rel="external"]）
伪类选择器（a:hover,li:nth-child）
伪元素选择器（::before、::after）
通配符选择器（*）

---


在css3中使用单冒号来表示伪类，用双冒号来表示伪元素

伪类一般用于匹配元素的一些状态，而伪元素一般用于匹配元素的位置


---
CSS3 圆角

使用border-radius属性

可以使用1——4个值

1个值，表示四个角都使用这个值

2个值，表示第一个值对应左上和右下，第二个值对应左下和右上

3个值，表示第一个值对应左上，第二个值对应左下和右上，第三个值表示右下

4个值，表示第一个值对应左上，第二个值对应右上，第三个值表示右下，第四个值表示左下



单独设置圆角

左上角
border-top-left-radius
右上角	 
border-top-right-radius	  
右下角
border-bottom-right-radius	
左下角
border-bottom-left-radius	

---


css阴影

box-shadow: 10px 10px 10px #fff;


第一个值是阴影水平位移，第二个值为阴影垂直位移，第三个值为模糊半径

第四个值为阴影颜色


---

transition属性
例如:
    div{
            width: 100px;
            height: 100px;
            background: #000;
            transition: width 1s;
        }
    div:hover{
            background: #f6f6f6;
        }

---

z-index属性

该属性用于堆叠元素的顺序，高级的元素会堆叠在低级的元素前面，例如：

    #app{
         z-index: -1;
    }

font-family属性

该属性用于设置元素字体，可以设置多个字体，当第一个字体不支持时，自动尝试设置下一个字体

@keyframes 规则：这个规则是可以将样式以动画方式（特定时间）逐渐地从当前样式修改为新的样式，当超过特定时间又恢复原来的样式

    #app{
        width: 100px;
        height: 100px;
        background-color: #000;
        animation-name: test;
        animation-duration: 6s;
        animation-delay: 3s;
        animation-iteration-count: infinite;
    }
    @keyframes test {
        from{
            background-color: #000;
        }
        to{
            background-color: #ccc;
        }
        
    }




background-image

该属性可以添加背景，可以添加多张背景，例如：

    #app{
        background-image: url(1.jpg),url(2.jpg);
        background-position: left top,right bottom;
        background-repeat: no-repeat,repeat;
    }

background-size

该属性可以指定背景的大小，例如：
    #app{
        background-image: url(1.jpg);
        background-repeat: no-repeat;
        background-size: 100px 100px;
    }


background-origin

该属性可以指定背景的位置区域（外边框（border-box），内边框（padding-box），内容区（content-box）），例如：

     background-color: content-box;


background-clip

该属性可以将背景裁剪到指定位置（外边框（border-box），内边框（padding-box），内容区（content-box），例如：

     background-clip: content-box;



渐变分为线性渐变（直线）和径向渐变（中心）


线性渐变（默认从上到下）

    #app{
        width: 300px;
        height: 300px;
        background-image: linear-gradient(#ccc,#000);
    }

从左到右

    #app{
        width: 300px;
        height: 300px;
        background-image: linear-gradient(to right ,#ccc,#000);
    }

从左上角到右下角

    #app{
        width: 300px;
        height: 300px;
        background-image: linear-gradient(to right bottom ,#ccc,#000);
    }

带角度的（不能指定方向）

    #app{
        width: 300px;
        height: 300px;
        background-image: linear-gradient(30deg,#ccc,#000);
    }

带透明度的

    #app{
        width: 300px;
        height: 300px;
        background-image: linear-gradient(30deg,rgb(255,0,0,1),rgb(0,0,0,0));
    }


带重复的

    #app{
        width: 300px;
        height: 300px;
        background-image: repeating-linear-gradient(#ccc, #fcfcfc 20%, #000 30%);
    }


径向渐变

    #app{
        width: 300px;
        height: 300px;
        background-image: radial-gradient(#fff, #ccc, #000,#fc0000);
    }

必须要有两个以上的颜色，第一个颜色在中间，依次往外排

同样可以设置每个颜色的占比

    #app{
        width: 300px;
        height: 300px;
        background-image: radial-gradient(#fff 10%, #ccc 20%, #000 30%,#fc0000 40%);
    }

当元素是不正的，那么径向渐变默认就是椭圆的（ellipse），也可以指定为圆形的（circle）

    #app{
        width: 300px;
        height: 250px;
        background-image: radial-gradient(circle,#fff, #ccc, #000,#fc0000);
    }

还提供了定义渐变大小的关键字，分别是closest-side，farthest-side，closest-corner，farthest-corner


当然也提供了可以重复渐变的，例如：

    #app{
        width: 300px;
        height: 250px;
        background-image: repeating-radial-gradient(circle,#000 20%, #fc0000 30% , yellow 50%);
    }



文本阴影

    #app{
        text-shadow: 5px 5px 5px #000;
    }

值分别为水平阴影，垂直阴影，模糊度，阴影颜色

元素盒子阴影


    #app{
        box-shadow: 5px 5px 5px #000;
    }


容器文本溢出处理

    #app{
        text-overflow: ellipsis;
    }

文本溢出默认为text-overflow: clip


自动强制文本换行（当单词或者某个文本太长，就会强制换行，避免文本溢出）

    #app{
        word-wrap: break-word;
    }


@font-face 规则

该规则允许使用客户端没有的字体，例如：

    @font-face {
        font-family: hallo;
        src: url("hallo.ttf");
    }

    div{
         font-family: hallo;
    }

必需属性font-family：字体名称

src：字体文件的url

在需要用字体的元素上font-family: 字体名称



过渡动画，会逐渐式加载样式，例如

    #app{
        width: 100px;
        height: 100px;
        background-color: #333;
        transition: height 3s,width 3s,transform 3s;
    }

    #app:hover{
        width: 300px;
        height: 300px;
        transform: rotate(360deg);
    }
  

当鼠标移动到该元素上，那么会在3秒内逐渐转换为指定样式，移开鼠标那么又会逐渐恢复原来的样式






column-count 属性，该属性可以将元素中的文本分成指定列，例如

    #app{
        column-count: 3;
    }

每个列之间的距离使用column-gap属性指定，例如

    #app{
        column-count: 3;
        column-gap: 20px;
    }

column-rule-style属性可以指定每个列分隔的边框样式，例如下面样式为实线

    #app{
        column-count: 3;
        column-gap: 20px;
        column-rule-style: solid;
    }

column-rule-width属性可以指定列的边框之间的宽度，例如

    #app{
        column-count: 3;
        column-gap: 20px;
        column-rule-style: solid;
        column-rule-width: 1px;
    }


column-rule-color属性可以指定列的边框的颜色，例如：

    #app{
        column-count: 3;
        column-gap: 20px;
        column-rule-style: solid;
        column-rule-width: 6px;
        column-rule-color: #000;
    }


当然列的边框的样式可以缩写，例如：

    #app{
        column-rule: 6px solid #000;
    }

如果在父元素内，某个子元素不想被分列影响，直接指定子元素，例如

     div{
         column-span: all;
     }


column-width可以指定列的宽度，例如

     div{
         column-width: 300px;
     }


column-gap属性和column-width属性之间的区别，就是column-gap控制的是列与列之间的距离，而column-width控制的却是列本身的宽度



动画，可以指定时间，在指定时间内逐渐成另一种样式，可以指定动画的过程的样式，例如

    #app{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        animation: hallo 5s;
    }

    @keyframes hallo{
        0%   {background-color: #ccc;}
        25%  {background-color: #fff;}
        50%  {background-color: #222;}
        75%  {background-color: #f3f3f3}
        100% {background-color: #777;}
    }

0%为动画开始，100%为动画结束，当动画结束时样式会恢复原来定义的样式，除了用百分比表示进度，也可以用from（起点）和to（终点）表示

能修改颜色，当然也能修改位置，例如：

    #app{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        animation: hallo 5s;
        position: relative;
    }

    @keyframes hallo{
        0%  {
            background-color: #ccc;
            top: 0px;
            left: 0px;
        }
        25% {
            background-color: #fff;
            top: 0px;
            left: 300px;
        }
        50% {
            background-color: #222;
            top: 300px;
            left: 300px;
        }
        75% {
            background-color: #f3f3f3;
            top: 300px;
            left: 0px;
        }
        100%{
            background-color: #777;
            top: 0px;
            left: 0px;
        }
    }



二维改变样式

旋转（值为正数则顺时针转，值为负数则逆时针转）

    #app{
        width: 300px;
        height: 300px;
        background-color: #ccc;
        transform: rotate(60deg);
    }

平移(根据x（左），y（上）轴决定)

    #app{
        width: 300px;
        height: 300px;
        background-color: #ccc;
        transform: translate(100px,100px);
    }

放大或者缩小

    #app{
        width: 300px;
        height: 300px;
        background-color: #ccc;
        transform: scale(2,2);
    }
宽度和高度是原来的2倍


倾斜(根据x（水平），y（垂直）轴决定，默认为0，值为负数则逆时针转)

    #app{
        width: 300px;
        height: 300px;
        background-color: #ccc;
        transform: skew(10deg,30deg);
    }



同样也可以缩写为一个属性

    #app{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        transform:matrix(0.6,1.5,-0.5,1.2,3,3);
    }

这个6个值分别代表旋转，放大或者缩小，平移，倾斜




三维改变样式

    #app{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        transform: rotateX(60deg);
    }

    #app{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        transform: rotateY(60deg);
    }

    #app{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        transform: rotateZ(60deg);
    }



旋转元素的起始位置

    #app{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        transform: rotate(60deg);
        transform-origin: 30% 60%;
    }

拓展

    #app{
        width: 500px;
        height: 500px;
        perspective: 200;
        perspective-origin: 10% 20%;
        background-color: #000;
    }

    #app div{
        width: 300px;
        height: 300px;
        background-color:#ccc;
        transform: rotateX(60deg);
        transform-style: preserve-3d;
        backface-visibility: hidden;
    }

backface-visibility属性用于指定背面是否显示

perspective-origin属性用于改变底部位置

perspective属性用于查看透视图（z只支持3D元素）

transform-style属性用于是否保留3d位置保留



---

css强制换行，避免文本溢出容器


white-space: pre-wrap;


normal：默认，空白被忽略

nowrap：文本不换行，文本在同一行继续

pre：保留空格，换行保留，不自动换行

pre-wrap：保留完整空格，保留换行符，自动换行

pre-line：保留空格（可能不完整），保留换行


---

filter属性滤镜

该属性默认值为none

背景为黑白

filter: grayscale(100%);


高斯模糊

filter: blur(3px);

亮度

filter: brightness(160%);

对比度

filter: contrast(160%);

透明度

filter: opacity(30%);

饱和度

filter: saturate(300%);

阴影

filter: drop-shadow(5px 5px 6px #ccc);

色相旋转

filter: hue-rotate(30deg);

颠倒输入

filter: invert(30%);



允许使用多个滤镜，多个滤镜有空格分开


---

表格布局（现在很少用，一般都是用div+css）

该元素会作为块级表格（例如table标签，有换行）
display: table;

内联表格（没换行）
display: inline-table;

表格行（例如tr）
display: table-row;

表格单元格（例如td）
display: table-cell;

表格标题
display: table-caption;

单元格列
display: table-column;


