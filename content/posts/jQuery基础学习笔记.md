---
title: "jQuery基础学习笔记"
categories: [ "默认" ]
tags: [ "jQuery" ]
draft: false
slug: "21"
date: "2021-06-16 09:38:00"
---

##第一个jQuery程序

   $('div').html("hello.world");

---
##DOM对象和jQuery对象互转化

jQuery对象和DOM对象是不一样的，但是都能操作DOM

get()方法（jquery对象转化为DOM对象）

   var $main =$('.main');   // jquer对象

   var main = $main.get(0); // 通过get方法，转化成DOM对象

   main.style.color = '#c7edcc'; // 操作DOM对象属性


DOM对象转化为jQuery对象

      
   var main = document.getElementsByClassName('main'); // DOM对象
        
   var $main = $(main); // jQuery对象

   $main.css('color','#c7edcc'); // 操作jQuery对象属性


---
##jQuery选择器

元素选择器

$('div')


ID选择器

$("#main")

id是唯一性，只能在页面中使用一次

类选择器

$('.main')

全选择器

$('*')

层次选择器

$('div .main')


属性选择器

$("a[href="https://cjlio.com"]") // 选择带href属性的a元素


可以使用前缀或者后缀来选择
$("div[name^="yes"]") // 选择div标签的neme属性值为yes开头的
$("div[name$="yes"]") // 选择div标签的neme属性值为yes结尾的

组合选择器

$("div[class=divs]")

组合选择器其实就是用多个选择器组合起来 

多项选择器

$("div[class=divs],a[href="https://cjlio.com")

多项选择器就是将多个选择器用逗号组合起来

层级选择器
$("ul.nev li.active") 

必须确保DOM元素之前具有层级关系，每个层级使用空格分开

子选择器

$("ul.nev>li.active") 

必须确保层级关系是父子关系（不是祖先关系）



过滤选择器

$("div:first") // 过滤出第一个div元素

$("ul li:first-child") // 过滤出每个ul元素下的第一个li元素

$("ul li:last-child") // 过滤出每个ul元素下的最后一个li元素

$("ul li:nth-child(3)") // 过滤出每个ul元素下的最后一个li元素

$("ul li:nth-child(odd)") // 过滤出每个ul元素下个数为奇数的li元素，从1开始

$("ul li:nth-child(even)") // 过滤出每个ul元素下个数为偶数的li元素

$('div').find('.main') // 选择div元素下的全部带有main类的元素

$(.main).parent() // 选择.main类的上一级（父级）元素

$(.main).parents() // 选择.main类的父（祖先）级元素


特殊选择器

$(":input") // 选择input,textarea,select,button类型的元素

$(":file") // 选择input元素type属性为file的

$(":checkbox") // 选择input元素type属性为checkbox的

$(":radio") // 选择input元素type属性为radio的

$(":focus") // 选择当前输入框焦点所在的元素

$(":checked") // 选择当前被勾上的单选框和复选框

$(":enabled") // 选择当前可以输入的元素，例如input

$(":disabled") // 和enabled相反，选择当前不能输入的

$('div:visible') // 选择当前所有可见的div

$('div:hidden') // 选择当前所有隐藏的div



查找以及过滤

查找使用find(),例如：

var yes = div.find('.container')

如果需要从父级或者祖先查找，需要用到parent()，例如：

var abc = yes.parent('.container') // 如果过滤条件不符合，返回空jQuery对象

如果在同一级中，需要用到prev()和next()，例如：

abc.prev() // 在同一级中向上

abc.next() // 在同一级中向下


过滤

这个过滤不是上面那个过滤选择器之类（那个是选择），这个是真的过滤。过滤掉不符合条件的，例如：

var abc = yes.filter('.container') // 过滤掉div元素中带有.container类的

map()方法可以将jQuery对象包含的DOM元素转换为其他对象（例如数组，数组也是一个对象）例如：

var abc = yes.map(function () {
    return this.innerHTML;
}).get();



---

##操作DOM



$(.main).css({xxx : 'xx',xxx : 'xx'}) // 操作多条样式（对象）多个样式用逗号分开

.addClass() // 为被选择元素添加一个类或者多个类（class属性），多个类要使用空格分隔开

.removeClass() // 为被选择元素移除一个类或者多个类（class属性），多个类要使用空格分隔开，如果没有指定要移除那个类，那么将移除全部类

.hasClass() // 检测被选择元素是否存在某个类，返回的值为布尔值

.hide() // 被选择元素隐藏，可选参数为时间，单位为毫秒

.show() // 被选择元素显示，可选参数为时间，单位为毫秒

.fadeOut() // 被选择元素隐藏，带淡出效果，可选参数为时间，单位为毫秒

.fadeIn() // 被选择元素显示，带淡入效果，可选参数为时间，单位为毫秒

.slideUp() // 被选择元素隐藏，带滑动效果，可选参数为时间，单位为毫秒

.slideDown() // 被选择元素显示，带滑动效果，可选参数为时间，单位为毫秒

.animate() // 可以修改被选择元素的样式，将样式过渡到设定值，单位为毫秒

.delay() // 可以和.animate()搭配来延长执行.animate()的功能，单位为毫秒

$('.main').text('hallo,world');  // 只能操作文本

.html('<p>hallo,world</p>') // 可以添加元素标签

.prepend('<p>hallo,world</p>') // 在前面添加

.append('<p>hallo,world</p>') // 在后面添加

.remove() // 删除

---

##事件


$('.main').click() // 点击触发事件（单击）

.ready() // 当文档加载完成才执行触发事件

.dblclick() // 双击触发事件

.mouseenter() // 当鼠标移动到元素触发事件（接触元素）

.mouseleave() // 当鼠标离开元素触发事件

.mousemove() // 当鼠标在指定元素中移动时触发事件

.mousedown() // 当鼠标移动元素，并且单击时触发事件

.mouseup() // 当鼠标在元素上点击，松开鼠标按键时触发事件

.hover() // 当鼠标移动到元素和离开元素时触发事件，有两个指定函数，但进入时触发第一个函数，离开时触发第二个函数

.focus() // 当焦点在元素上时，触发事件

.blur() // 当元素失去焦点时，触发事件

.keyup() // 当键盘被松开时触发事件

.keydown() // 当键盘被点击时触发事件

.keypress() // 当在输入处键盘被按键时触发（屏蔽输入法的按键，每输入一个字符，触发一次）

.scroll() // 当元素被滚动时触发（搭配overflow:scroll;使用更佳）

.resize() // 当对浏览器窗口调整大小时触发

.submit() // 当提交表单时触发（只作用在form元素）

.change() // 当元素的值被改变时，在失焦时触发（只适用文本域和 textarea元素，select 元素）

$('.main').val();   // 获取或者设置值（value属性，input元素）

.focus() // 当输入框得到焦点时触发

.select() // 当文本类型得到标记（被选择）时触发

.blur() // 当输入框失去焦点时触发

.submit() // 将表单数据提交到web服务器


解除事件绑定

例如：

abc.click(yes)

setTimeout(function(){
abc.off("click",yes)

},1000)

时间单位为毫秒

如果不是使用函数来调用的，可以直接使用off('click')来解除click事件，也可以直接用off()来解除全部被绑定事件



---

##操作元素属性


$('.main').attr("color","333") // 设置或者返回被选元素的属性和值

.prop() // 设置或者返回被选元素的属性和值

.removeAttr() // 移除属性

注意：

操作自定义属性要使用attr()，操作元素本身就携带的固有属性推荐使用prop()


---

AJAX

jQuery提供了ajax()函数，例如：

var hallo = $.ajax('url:/test.txt',dataType: 'text')


ajax()需要一个可选参数，一般的参数有：

url ： 发送请求的地址，默认为当前页面

dataType ：接收的数据格式，例如：html，text等等，如果没有提供该参数，由Content-Type来处理

async ： 是否执行异步请求，没有提供该参数时，默认为true

data ： 发送到服务器的数据，可以是字符串，对象或者数组

success ： 请求成功后的回调函数

get()

jQuery提供了get方法，例如：

var abc = $.get(url: /test.html,{
user: 'root',
pass: '123'
})

如果这样写，那么链接为/test.html?user=root&pass=123


post()

var abc = $.post(url: /test.html,{
user: 'root',
pass: '123'
})

getJSON()

var abc = $.getJSON(url: /test.html,{
user: 'root',
pass: '123'
}).done(function(datas){})


jQuery允许扩展jQuery来自定义方法

$.fn.hallo = function(){
this.css('width','100px').css('height','100px')
return this
}

调用jQuery插件

$('.main').hallo()

jQuery插件参数

$.fn.hallo = function(){
var widths = options && options.width || '1920px'
var heights = options && options.height || '1080px'
this.css('width',widths).css('height',heights)
return this
}

$('.main').hallo({
width: '800px'
height: '600px'
})
