---
title: "HTML全局属性笔记"
categories: [ "学习" ]
tags: [ "html" ]
draft: false
slug: "22"
date: "2021-06-16 09:38:00"
---

属性

accesskey

定义快捷键获取焦点，例如

    <a href="https://cjlio.com" accesskey="q">GO\</a>

按ait+q，就会跳到指定的网页上


---


class

定义元素的类，开头必须是字母，多个类使用空格隔开，例如

    <div class = "a1 a2 a3"></div>

---

id

定义一个id，id为唯一性，不能重复，例如

    <div id = "a1"></div>

---

lang 

定义网页或者元素的语言，例如

    <div lang = "fr"></div>

---

style

定义元素的行内样式，例如

    <div style="color : #fff">hi</div>


---

tabindex

指定tab键的焦点控制，例如

    <a href="https://cjlio.com" tabindex="1">GO</a>

使用键盘的tab键盘，触发（不会跳转到网页，只是焦点）


---


contenteditable

指定元素是否为可以编辑的，例如

    <div contenteditable="true">hi</div>


---

dir

指定元素内文本的方向，例如

    <div dir = "rtl">hi</div>

ltr默认值，从左到右

rtl，从右到左


---

title

指定元素的信息，一般为鼠标移动到元素是停留一段时间，显示信息，例如

    <div title = "hi">hi</div>


---


data-xxx

用于存储一些自定义属性，data-后面必须有一个字符，不包括大写

JavaScript可以通过getAttribute获取到



---

draggable

指定元素是否可以拖动，默认情况下，只有图片和链接可以拖动

有3个可选值，true/false/auto，在JavaScript中可以配合拖动事件，例如

    <div draggable = "true">hi</div>


---

hidden

指定元素是否隐藏，有两个可选值，hidden/true，例如


    <div hidden = "hidden">hi</div>


---


contextmenu

指定div元素的菜单，目前只有 Firefox 浏览器支持




---


dropzone

指定元素被拖动时，拷贝、移动或链接被拖动数据，目前所有主流浏览器都不支持



---

spellcheck

指定元素是否进行拼写检查，有两个可选值，true/false

可以对类型为text的非密码的input元素的值，textarea 元素中的值，可编辑元素中的值


---

translate

指定渲染元素时是否要对内容进行翻译，目前所有主流浏览器都不支持


---
