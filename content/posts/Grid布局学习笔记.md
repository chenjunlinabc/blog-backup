---
title: "Grid布局学习笔记"
categories: [ "默认" ]
tags: [ "css" ]
draft: false
slug: "45"
date: "2021-06-23 10:29:00"
---

grid 布局

grid布局和flex布局类似，不过grid是最强大的css布局方式

grid布局是网格布局

display: grid; // 定义grid布局，默认为块级元素

display: inline-grid // 设置为行内元素

设置为grid布局后，容器的子元素的float（浮动），display: inlne-block（行内块级元素），display: table-cell（表格单元格），vertical-align（垂直对齐方式）之类的全部都会失效


列宽和行高的设置

grid-template-columns: 100px; // 列宽

grid-template-rows: 100px; // 行高

数值也支持百分比

grid-template-columns: repeat(3, 100px); // 列宽，重复次数为3，重复的值为100px，和下面效果是一样的

grid-template-columns: 100px 100px 100px;

grid-template-rows: repeat(auto-fill, 100px); // 行高，自动填充容器，行高为100px

grid-template-rows: 1fr 2fr; // 行高，第二个的行高是第一的2倍

grid-template-rows: minmax(100px, 300px); // 生成一个长度范围，长度在这个范围内，两个参数分别代表最小值和最大值，这里表示的是行高不小于100px，不大于300px

grid-template-rows: auto; // 由浏览器决定行高

grid-template-columns: [a1] 100px [a2] 100px [a3] auto;  // 方括号[]内的的值是用于指定网格的名称

grid-template-columns: 20% 60% 20%; // 左栏20%，中间60%，右栏20%，如果是重复的值搭配repeat使用更佳

grid-row-gap: 10px; // 行间距（行与行的距离）

grid-column-gap: 10px; // 列间距（列与列的距离）

可以简写为grid-gap: 10px 20px; // 行间距，列间距，如果只提供一个值，那么行间距，列间距都为那个值

---

grid-row-start: 2; // 指定从哪条行开始，这里指定在第二行上 

grid-column-start: 2; // 指定从哪条列开始，这里指定在第二列上 

grid-row-end: 2; // 指定横跨第几行，这里指定的是第二行

grid-column-end: 2; // 指定横跨第几列，这里指定的是第二列

上面的属性默认为auto，可以简写为下面的

指定项目放在哪个区域

grid-area: b; // 指定该项目放在b区域

grid-area：2 / 2 / span 2 / span 2;  // 在第二行第2列开始，横跨两行两列

span关键字是跨域的意思，指的是跨域多少个网格

---

grid-template-areas: "a b c"; // 指定区域中对应的单元格，如果区域中不需要用到则用点.来表示

grid-auto-flow: column; // 设置为先放满列再放行，网格布局，容器的元素会根据顺序进行排列，默认是先放满第一行，再放第二行，而该属性设置为先放满第一列，再放第二列表，默认值为row

如果有一些元素的大小是不一致，浏览器认为这个位置放不下，会另外起一行放

grid-auto-flow: row dense; // 先填满行，而且是尽可能的填满，不留下空隙

grid-auto-flow: column dense;  // 先填满列，而且是尽可能的填满，不留下空隙


---



设置单元格内容的位置

justify-items: centet; // 水平位置，内容居中

align-items: centet; // 垂直位置，内容居中

stretch：拉伸，占满整个单元格的宽度（默认值）
start：对齐单元格的起始位置
end：对齐单元格的结束位置
center：单元格内容居中

可以简写为place-items: centet centet; // 水平居中，垂直居中，如果只提供一个值，那么水平和垂直都是这个值


设置单个项目的位置

justify-self: center; // 设置单个项目在网格中的水平方向位置，居中

align-self: center; //  设置单个项目在网格中的垂直方向位置，居中

和justify-items，align-items类似，只是作用于单个项目

可简写为place-self: center; // 如果不提供第二个值，那么垂直方向和水平方向都是那个值


设置网格在容器的位置

justify-content: center; // 网格在容器水平方向为居中

align-content: center; // 网格在容器垂直方向为居中



属性值和上面的类似

space-around // 网格的项目的两侧的距离相等，a项目和b项目的距离是相等，所以实质上项目与项目之间的距离是和容器之间的距离的2倍

space-between // 项目和项目之间距离相等，项目和容器没有间隔

space-evenly // 项目和项目之间距离相等，项目和容器之间也是相等间隔


可以简写为place-content: center center; // 水平居中，垂直居中，如果只提供一个值，那么水平和垂直都是这个值


---

如果浏览器自动生成一些多余网格，而要设置这些多余网格的行高和列宽

grid-auto-columns: 100px; // 多余网格的行高为100px

grid-auto-rows: 100px; // 多余网格的列宽为100px

---

项目属性定位

grid-column-start: 2; // 左边框在第二个项目的垂直线上

grid-column-end: 3; // 右边框在第三个项目的垂直线上

grid-row-start: 2; // 上边距在第二个项目的水平线上

grid-row-end: 3; // 下边距在第三个项目的水平线上

除了设置项目的定位外，还可以设置网格线的名称

grid-column-start: main-start;

可以简写为

grid-column: 2 / 3;

grid-row: 2 /  3;

效果和上面一样


---













