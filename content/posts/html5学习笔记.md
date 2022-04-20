---
title: "html5学习笔记"
categories: [ "默认" ]
tags: [ "html" ]
draft: false
slug: "12"
date: "2021-06-16 09:22:00"
---

HTML5是HTML标准的第5代标准，主要目的是语义化并且提供多媒体的嵌入

---
HTML是什么？HTML全称为HyperTextMarkupLanguage，中文叫超文本标记语言，简称HTML

而HTML5是一个标准，指的是第五代HTML标准

HTML5主要的新特性：

语义特性，本地存储特性，设备兼容特性，连接特性，网页多媒体特性，性能与集成特性，CSS3特性

</br>

HTML的块级与行级

块级元素的特征：

独占一行，不和其他元素待在同一行上，能设置宽和高

默认宽度是该元素的容器的100%，不过可以设置宽度

常用的块级元素有div，ul，li，dl，dt，h1-h6

行级元素的特征：

可以和其他元素待在同一行上，不能设置宽和高

它的宽度就是它的文字或者图片的宽度

常用的行级元素有a，span

行内块级元素的特征：

可以设置宽和高，可以一行多个


常见的行内块级元素有input，img


转换元素为块级或者行内

display：block //定义元素为块级元素

display: inline //定义元素为行内元素

display：inline-block //定义元素为行内块级元素。




这三个的区别只有三个，排列分式，设置宽高，默认宽度
</br>


html语义

语义化：使得页面可以很好的向浏览器和开发者描述其意义

语义化的好处：

方便开发团队的前期开发和后期维护，不只是作用于自己的开发团队，也方便其他国家的开发者能理解网页的结构；

在css文件丢失的情况下，也能表示出好的内容结构和代码结构，方便用户阅读；

方便辅助技术能更好的阅读或者转译网页，方便有障碍人士阅读；

良好的结构和语义，可以提高搜索引擎爬虫的有效爬取；



重点：用正确的标签做正确的事！！！

要注意可以改变样式的标签不一定是有居于语义的


在没有出现语义元素前，几乎都是使用div或者span，加类加id

没有语义的元素最适合当容器使用了


常用的非语义元素有<div>和<span>

常用的语义元素有<header>和<footer>


<header>：页眉，一般包括网站logo，主导航，搜索框等；

<nav>：导航，链接；

<main>：定义文章的主要内容，一个页面只能使用一次；

<article>：定义一份独立的内容，脱离其他内容或者其他部分，独立于文档的其余部分；

<section>：定义内容的节（段）；

<footer>：页脚，一般包含版权信息或者链接等；

<aside>：侧边栏，一般作为附属信息，例如导航索引，广告等；


</br>


meta viewport

viewport 是指 web 页面上的可见区域

<meta name="viewport" content="width=device-width, initial-scale=1.0">

device-width指设备的理想宽度,不同的设备 device-width 是不一样的

nitial-scale=1.0 是指默认缩放大小是1，也就是默认不缩放

maximum-scale=1.0 是指最大缩放大小是1



</br>


---


标签使用<和>括起来，例如<div>

html标签大多都是成对出现的，分开始标签和结束标签，结束标签比开始标签多了个/

例如：
<div></div>


html不区分大小写。所以<DIV>和<div>作用一样。建议使用小写

<!DOCTYPE html>声明这是一个html5文件，声明位于<html>前面

<html> 标识HTML文档的开始
<head>表明一些和html文档有关的信息，例如title
<body>html文档的主体

<!--这是一个注释-->



段落<p>

<span>
<h1>~<h6>

<div>


定义头部区域<header>

定义底部区域<footer>

定义区段<section>

定义侧边栏区域<aside>

换行<br>

文本输入框 <input>


select+option 选择列表


---


多媒体元素（video和audio）

例如：

<video src="hallo.mp4" controls autoplay="autoplay" loop></video>

<audio src="hallo.mp3" controls autoplay="autoplay" loop></audio>

controls属性是显示控制栏，autoplay属性是自动播放，loop属性是循环播放

注意：video元素支持的视频格式有mp4，ogg，webm，不过每个格式在不同浏览器内核兼容性不一样


source标签就完美解决这个问题，例如：

    <video>
        <source src="hallo.mp4">
        <source src="hallo.ogg">
        <source src="hallo.WebM">
    </video>


播放顺序是如果第一个支持就是只播放第一个，如果第一个不支持，第二个支持，就播放第二个



表单控件可以控制输入内容必须为指定合法的数据，例如：

    <form>	
        邮箱地址：
        <input type="email">
    </form>

type新的属性值有：

email：邮箱地址

url：网址

number：数字

color：拾色器

time：时，分

week：周

month：月

date：年，月，日

datetime-local：年，月，日，时，分，秒

range：滑块



form的一些属性

autocomplete：该属性可以实现自动输入功能，浏览器基于之前输入并且提交过的值，属性值有on和off

novalidate：该属性是是否关闭校验，搭配表单控件食用更佳



input的一些属性

autofocus：自动获取焦点

placeholder：提示信息（占位符）

required：必填项

list：带本地数据缓存列表的自动输入，例如


    <form>
        <input type="url" list="url_list" name="link"/>
        <datalist id="url_list">
            <option label="blog" value="https://cjlio.com" />
            <option label="baidu" value="https://www.baidu.com" />
            <option label="github" value="https://www.github.com" />
        </datalist>
        <input type="submit" />
    </form>



multiple：多选项

如果一个表单不被form元素包含，但是又想提交过去，那么可以指定该input元素的form属性值为form元素的id




canvas元素用于图形的绘制，该元素只是一个容器，还需要js来绘制，例如：

    <canvas width="300px" height="300px"></canvas>
    <style>
        canvas{
            border: 1px solid #000;
        }
    </style>
    
    <script>
        //获取dom对象
        var canvas =document.querySelector("canvas");
        //获取绘图上下文
        var ctx=canvas.getContext("2d");
        ctx.fillStyle="#000";
        ctx.fillRect(0,0,100,100);
        // Canvas坐标是二维的，代表左上角的坐标为0,0
    </script>



canvas画线，提供了两个方法moveTo()和lineTo()，例如:

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.moveTo(100, 100); // 线条开始坐标
    ctx.lineTo(200, 100); // 线条结束坐标
    ctx.stroke(); // 绘制


画圆圈：

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.beginPath();
    ctx.arc(100,100,80,0,2*Math.PI,false);  // 圆心的坐标（x，y），半径，开始弧度，结束弧度，方向(默认为false顺时针,true代表逆时针)
    ctx.stroke();

以圆心为中心向右（3点方向）为0度角，顺时针为正，逆时针为负

1角度=π/180度，1弧度=180度/π,一个圆有360度，也就是说一个圆是2弧度

30度=π/6,60度=π/3,90度=π/2,180度=π,360度=2π，依此类推

x坐标 = 圆心的横坐标+圆的半径*cos(弧度)

y坐标 = 圆心的纵坐标+圆的半径*sin(弧度)




画三角形

     var canvas =document.querySelector("canvas");
     var ctx=canvas.getContext("2d");
     ctx.moveTo(10, 10);
     ctx.lineTo(10, 100);
     ctx.lineTo(70, 100);
     ctx.closePath(); // 闭合
     ctx.stroke();
     ctx.fillStyle="#ccc"; // 填充颜色
     ctx.fill();


平移(坐标系圆点的平移)

translate(x,y)

注意：translate不能只设置一个值(除非使用translateX或者translateY进行单独设置值)，和moveTo()的区别就是moveTo是修改开始绘制的位置，圆点位置没有改变，而translate是改变圆点位置，例如：

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.translate(100,100);
    ctx.fillStyle="gray";
    ctx.fillRect(50,50,100,50);




旋转


    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.translate(100, 100);
    ctx.rotate(Math.PI/6);
    ctx.moveTo(0, 0);
    ctx.lineTo(200, 0);
    ctx.lineTo(0, 100);
    ctx.stroke();



缩放

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.translate(300,200);
    ctx.scale(0.3, 0.5); // x轴和y轴缩放倍数，例如0.3，0.5
    ctx.arc(10,10,200,0,2*Math.PI);
    ctx.stroke();




线与线之间链接的方式，主要用到lineJoin属性，属性值有miter（尖角，默认），bevel（斜角），round（圆角），例如：


    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.lineWidth=10; // 线的宽度
    ctx.lineJoin="bevel"; // 线与线的链接方式
    ctx.moveTo(80, 80); // 线条开始坐标
    ctx.lineTo(200, 130); // 线条结束坐标
    ctx.lineTo(150, 30);
    ctx.stroke(); // 绘制


线帽

主要实现方式就是使用lineCap()属性，例如：

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.lineWidth=10;
    ctx.lineCap="round";
    ctx.moveTo(50,50);
    ctx.lineTo(200,50);
    ctx.stroke();


该属性有三个值，分别是butt（默认），round（圆形线帽），square（正方形线帽）

butt和square的区别就是，square（round也是）会导致线条变长一点

ctx.beginPath(); // 设置新的画布

非零环绕

如果想画一个不规则的图形，然后填充颜色，那么怎么知道那里该填色那里不填色呢，那么就是需要用到一个叫非零环绕的数学方法，实现原理很简单，描绘路径一笔完成就可以了，例如：

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.arc(120,120,80,0,2*Math.PI,false); // .arc()方法中最后一个参数是设置路径方向的
    ctx.arc(120,120,100,0,2*Math.PI,true);
    ctx.stroke();
    ctx.fillStyle="#ccc"; // 填充颜色
    ctx.fill();

非零环绕：


以点为圆心,绘制一条射线,以射线为半径顺时针旋转,相交的边同向记为+1,反方向记为-1,如果相加的区域等于0,则不填充,非零区域填充

例如：假设一个点，以这个点绘制一条射线，如果这个射线和这个点的一条路径正方向相交，则为+1，或者再有一个点，如果这个点和这个点的一条路径正方向，但是又有一条路径是反方向相交的，那么就是+1-1，那么就是0，只要结果不是0，那么射线所在的区域就是在外面，不填充


线性渐变

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    var clg=ctx.createLinearGradient(0,0,200,0);
    clg.addColorStop(0,"red");
    clg.addColorStop(1,"black");
    ctx.lineWidth="50";
    ctx.strokeStyle=clg;
    ctx.moveTo(100, 100); 
    ctx.lineTo(200, 100); 
    ctx.stroke(); 

createLinearGradient()有4个参数，分别代表渐变开始的xy坐标，渐变结束的xy坐标


addColorStop()方法，第一个参数是设置开始和结束或者中间的周期，第二个参数是颜色

0代表开始，1表示结束，中间颜色可以使用小数来设置

径向渐变
    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    var clg=ctx.createRadialGradient(200,200,100,120,120,100);
    clg.addColorStop(0,"black");
    clg.addColorStop(1,"red");
    ctx.fillStyle=clg;
    ctx.moveTo(50, 50); 
    ctx.lineTo(300, 100); 
    ctx.lineTo(300, 300); 
    ctx.lineTo(100, 300); 
    ctx.lineTo(50, 50); 
    ctx.closePath();
    ctx.fill();

createRadialGradient()中有6个参数，分别代表径向渐变开始的xy坐标，渐变开始的半径，向渐变结束的xy坐标，渐变结束的半径


虚线就是实线和空白区域之间的空白距离

setLineDash()，参数是一个数组，如果数组中有两个值，那么就是分别代表实线长度，空白长度，如果是三个值，那么就是分别代表实线，空白，实线...

例如：

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.moveTo(100, 100);
    ctx.lineTo(300, 150);
    ctx.setLineDash([10,3,15]);
    ctx.stroke();


绘制动画

动画确实就是绘制一个图形，清除这个图形，然后在另一个地方绘制图形，达到这个图形好像在动的错觉

清除图形用到clearRect()，例如：


    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    var x=0; // 初始位置
    var abc=10; // 每次移动多少像素
    var i=1; // 改变方向，做到一来一回的效果
    setInterval(function(){
    	// 定时器
    	ctx.clearRect(0, 0, canvas.width, canvas.height); // 清除
    	ctx.fillRect(x, 120, 120, 180); // 绘制新的图形
    	x+=abc*i; // 处理，实质上x是移动的位置变化，x=x+abc*i
    	if(x>canvas.width-120){
    		i=-1;
    	}else if(x<0){
    		i=1;
        // i的值为1时为正方向移动，为-1时为反方向移动，将值做成互为相反数来达到效果
    	}
    },30);



绘制文本

绘制文本有两个方法，fillText()实体文字，strokeText()镂空文字，例如：


    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    ctx.textAlign="center"; // 文本水平对齐方式,left,right,center
    ctx.font="30px 宋体"; // 文本大小与字体
    ctx.textBaseline="middle"; 
    // 文字垂直对齐方式,alphabetic(默认),top,middle,bottom,hanging,ideographic
    
    ctx.shadowColor="gray"; // 文本阴影颜色
    ctx.shadowOffsetX=3; // 文字阴影的水平偏移量
    ctx.shadowOffsetY=3; // 文字阴影的垂直偏移量
    ctx.shadowBlur=6; // 设置文字阴影的模糊度
    ctx.strokeText("hallo word", 200, 200); // 文本内容，x，y
    ctx.fillText("hallo word", 200, 200);



绘制图片

例如：

    var canvas =document.querySelector("canvas");
    var ctx=canvas.getContext("2d");
    var img=document.createElement("img"); // 创建一个img
    img.src="hallo.jpg";  // 图片位置
    img.onload=function(){
        ctx.drawImage(img,100,100); // 图片对象，x，y
    }


图片设置宽度高度

ctx.drawImage(img,100,100,200,200);  // 图片对象，x，y，宽度，高度


将图片绘制到矩形区域内的指定位置

ctx.drawImage(img,100,100,200,200,0,0,300,300);  // 图片对象，图片绘制到矩形的位置xy，图片的高度和宽度，图片矩形的位置，图片矩形的高度和宽度


解决图片比例不正常：

要满足：绘制的图片宽度：绘制的图片高度等于原始图片宽度：原始高度

那么就是说绘制的图片宽度等于绘制的图片高度乘以原始图片宽度除以原始高度，依此类推，保证绘制的图片宽高比例和原来图片的比例宽高一致




---


iframe框架

用于在网页中显示网页

\<iframe src="https://cjlio.com">\</iframe>

可以设置高度和宽度

frameborder="0" // 可以移除边框


可以设置target属性来做目标

target属性：浏览器会在指定文件中寻找对应的name，并且寻找到对应name的href属性

\<iframe src="index.html" name="iframe_a">\</iframe>
\<a href="https://cjlio.com" target="iframe_a">hallo\</a>

---

defer属性规定了执行延迟，当页面加载完毕就执行，不用再担心DOM元素获取不了，例如：

    <script type="text/javascript" defer="defer">
        console.log("hallo word")
    </script>

无论这句话防止哪里，都会等到页面加载完毕（dom渲染完毕后）才会执行



---

本地存储


localStorage是用键值对来进行本地存储，localStorage方法是window方法下的


存储

localStorage.setItem("root","123")


读取

let abc = localStorage.getItem("root")


删除


localStorage.removeItem("root")



例如:

    function onAdd(){
        if (localStorage.clickcount) {
            localStorage.clickcount = Number(localStorage.clickcount) + 1
        } else {
            localStorage.clickcount = 1
        }
        document.getElementById("root").innerHTML = "当前点击数：" + localStorage.clickcount + "次"
    }
    ...
    <button onclick="onAdd()" type="button">点击</button>
    <div id="root"></div>



注意：localStorage会永久存储，一直到手动删除，如果想保存在当前会话的话使用sessionStorage更好，该会在关闭页面的时候同时也销毁这些数据



sessionStorage也是window方法下的



存储

sessionStorage.setItem("root", "123")


读取

let abc = sessionStorage.getItem("root")


删除

sessionStorage.removeItem("root")


删除全部数据

sessionStorage.clear()



---





Cache Manifest是HTML5的一种缓存机制，用来缓存应用，让其在无网络的情况下还能访问

启动Cache Manifest也很简单，直接在html元素上使用manifest属性，属性值是manifest文件

W3C建议文件扩展名为.appcache


manifest文件格式

CACHE MANIFEST
/main.js
/logo.jpg
NETWORK:
test.html
FALLBACK:
/archives /404.html

上面表示/main.js和/logo.jpg将被缓存，哪怕断开连接，都是可用的，test.html不被缓存，如果不能连接网络，archives目录将被404.html覆盖

注意：应用被缓存，那么就会被一直缓存下去，一直到清空缓存，manifest文件被修改才会重新从互联网上获取最新的数据

离线存储是通过manifest文件来管理的，还需要服务器端的支持，因为这里是nginx服务器，因此修改mime.types文件，添加manifest文件映射

添加

    text/cache-manifest                              appcache;

然后重启nginx服务器

例如：


    <html manifest="demo.appcache">
        <body>离线缓存</body>
    </html>

---


DOCTYPE是声明浏览器应该使用哪种规范解析，因为HTML5不是基于SGML（标准通用标记语言），所以不需要DTD，而HTML4.01是基于SGML，所以才需要引用DTD，XHTML是严格版本的HTML，必须正确嵌套，闭合，以及区分大小写



DTD(Document Type Definition, 文档类型定义)是一套用来定义html或者xhtml，以及xml的文件类型的语法语法规则，浏览器根据dtd来判断文件类型，使用哪种模式来说解析



---


iframe

iframe会堵塞页面的加载事件（onload）(只有当全部iframe标签都加载完毕才会触发该事件)，还存在连接池问题（部分老版本浏览器限制同一个域名多开连接，页面和iframe共享连接池），而且搜索引擎无法爬取该页面，如果需要使用，请尽量保证iframe存在src属性



---

