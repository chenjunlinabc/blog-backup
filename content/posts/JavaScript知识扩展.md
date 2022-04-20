---
title: "JavaScript知识扩展"
categories: [ "技术" ]
tags: [ "JavaScript" ]
draft: false
slug: "61"
date: "2021-07-18 22:35:00"
---

函数被调用时，浏览器会传递两个参数，this和arguments

this就是函数的上下文对象，而arguments是一个数组对象（也就是可以通过索引来操作数据），函数调用时传递的参数会在arguments中保存

callee属性对应着当前的函数

例如：

    function abc(){
        console.log(arguments.length);
        console.log(arguments[0]);
        console.log(arguments.callee);
        console.log(this)
    }
    abc('hallo');


---


BOM对象 (Browser Object Model) 是指浏览器对象模型

该对象提供了浏览器行为和浏览器属性方法

windows表示整个浏览器的窗口，同时也是JavaScript最顶层的对象，其他bom对象都是其的属性


navigator包含了当前浏览器的全部信息

    console.log(window.navigator)

可以看到输出了很多属性，如userAgent，language等等


location表示当前浏览器地址信息，可以用来跳转到指定地址，获取当前页面的地址等等，例如：

    console.log(window.location)
    const urlData = "https://cjlio.com/;
    window.location.href = urlData;

这个也可以实现跳转

    window.location.assign("https://cjlio.com")

这个也可以实现跳转，不过这个不会生成历史记录，是直接用这个页面来替换当前页面

    window.location.replace("https://cjlio.com")


reload方法可以重载页面，加上true参数将强制更新

    window.location.reload(true)




history表示浏览器的历史记录

    console.log(window.history)

length表示本次访问网站的数量

同时也提供了几个方法

back()返回上个页面

window.history.back()

可以绑定个点击事件，用来返回上个页面


forward()前进，一般是和back()搭配使用，一个返回上个页面，一个返回到之前的页面

window.history.forward()

go()，前进或者后退指定次数，正数为前进，负数为后退

window.history.go(-1)

screen代表用户的屏幕的信息, 获取显示器的相关信息

    console.log(window.screen)



---

call 和 apply

addEventListen()绑定事件函数

removeEventListen()移除事件函数

鼠标按下时onmousedown

鼠标移动时onmousemove

鼠标松开时onmouseup


---

undefined代表定义未赋值

null定义并赋值了, 只是值为null



---







document.execCommand可以操作剪贴板

document.execCommand('copy') // 复制

document.execCommand('cut') // 剪切

document.execCommand('paste') // 粘贴



Clipboard API新一代剪贴板操作方法，而且全部操作都是异步的，而且还可以将任何内容都放到剪贴板中


const NavigatorClipboard = navigator.clipboard



---


Web Workers


Web Workers就是创建多线程的，因为JavaScript是单线程语言


Web Workers允许主线程创建Workers线程，将任务分配给Workers线程

主线程和Workers线程在运行的时候，互不干扰，当Workers线程处理完毕再将结果返回给主进程


注意：Workers线程不能和主线程进行通信，不能读取本地文件，只能通过网络文件获取，而且Worker执行的脚本文件必须要和主线程同源


创建Worker线程

var worker = new Worker('https://xxx.xxx/data.js')


可以看出Worker构造函数的参数是一个脚本文件，例如js

注意：因为Worker不能读取本地文件，只能从网络上获取，因此会出现下载失败的情况，当下载失败的时候，不会抛出错误的


主线程和worker线程通信

worker.postMessage("hallo word")


    worker.onmessage = function (event) {
        console.log('data:  ' + event.data)
    }


当worker线程处理完毕，结果可以被event.data获取到



关闭worker线程

worker.terminate()


---

深拷贝和浅拷贝


浅拷贝：两个变量指向同一个内存地址，修改任何一个值，另一个的值也会跟着变化

例如：

     var obj = {
          name: "root"
     }
     var abc= obj
     abc.name = "admin"
     console.log(obj, adc)


深拷贝：将对象的值复制，并且给予新的变量名和地址，改变一个值，并不会引起另一个值的变化，因为引用地址不同

例如：

     var obj = {
          name: "root"
     }
     var abc = { ...obj }
     abc.name = "admin"
     console.log(obj, abc)




new的作用：实例化一个新的对象-调用构造函数并且将构造函数的this指向该实例对象-实例对象共享构造函数的方法和属性



---



JavaScript的内存管理机制与垃圾回收机制

内存管理机制就是分配内存，JavaScript的内存管理就是创建时分配，不使用就是被回收释放

JavaScript内存空间分为栈，堆，池

栈存放变量，堆存放复杂对象，池存放常量

在声明一个变量或者对象，函数的时候系统会自动分配内存，使用完毕的时候通过垃圾回收机制自动回收


浏览器最常见的垃圾回收有两个，分别是标记清除，引用计数

标记清除：垃圾回收器会给存储在内存中的变量一个标记，然后会去掉在环境中的变量及被环境中的变量引用的变量标记（闭包），而在此之后剩下的带有标记的变量视为要被删除的变量，最后垃圾回收器将释放这些变量的内存，回收它们所占用的空间


引用计数：通过跟踪记录每个值被使用的次数，调用一次值解就加一，改变值为另一个值时就减一，当次数为0时，就是在说明没有变量在使用，这个值已经无法访问，垃圾回收器在运行的时候清理回收引用次数为0的值占用的空间


注意：引用计数方法会导致内存泄露，因为其循环引用，导致闭锁


---


this指向：在严格模式中，函数内部的this被指向到undefined上，在非严格模式上会被绑定到window/global全局对象上，在使用new调用构造函数时，该构造函数内部的this被指向到这个new出来的对象上，在使用call/apply/bind方法调用函数时，该函数内部的this被指向call/apply/bind方法指定参数的对象上，在箭头函数中，this的指向取决于外部作用域，在执行上下文中，this取决于最后调用它的对象

this优先级：使用bind，apply，call，new绑定this指向的叫显式绑定，而通过调用方式来绑定叫隐式绑定，显式绑定的优先级比隐式绑定的优先级高，在显式绑定中，new优先级最高

当某个函数在对象上下文中被调用，这个函数的this会绑定在这个对象上下文中，例如：

    function abc(){
       console.log(this.a)
    }
    let a = 1
    let obj = {
        a: 123,
        abc: abc
    }
    let obj1 = {
        a: 666,
        obj: obj
    }
    obj.abc() // 哪怕在全局作用域中调用，this依然指向对象上下文
    obj1.obj.abc() // 可以看到对象属性引用链只有真正调用函数的才可以影响this


丢失this绑定（由于默认绑定，会默认绑定到全局对象或者undefined上，从而丢失了this），例如：

    function abc(){
       console.log(this.a)
    }
    var obj = {
        a: 123,
        abc: abc
    }
    var funs = obj.abc
    var a = 666
    funs()

funs变量的值为obj.abc的引用，注意不是调用，调用要加()，执行funs时就是在调用abc函数（因为funs的值就是abc函数的引用），因此abc函数的this指向了全局对象（a在全局作用域中，会变成全局对象的属性），导致丢失了想要指向obj对象的this


使用let或者const关键字声明的全局变量，会被绑定到Script对象上而不是Window对象，使用var定义的变量，就可以被绑定到Window对象上

例如：

var abc = "hallo"
console.log(this.abc) // hallo
console.log(this) // Window

let abc = "hallo"
console.log(this.abc) // undefined
console.log(this) // Window


bind，apply，call都是改变this指向的

bind()：第一个参数是this的指向，后面的是参数列表，可以多次传入，而call要求一次性传入完毕

例如：

var arr=[1,2,4,6,7,10]
var max=Math.max.bind(null,arr[0],arr[1],arr[2])
console.log(max()) // 4




apply():接受两个参数，第一个参数是this的指向，第二个参数是函数接收的参数（这个参数是通过数组形式传参），如果第一个参数为null或者undefined的时候，将指向window

const arr = [1,2,4,6,7,10]
const abc = Math.max.apply(null,arr)
console.log(abc) // 10


call():第一个参数是this的指向，后面传入的是一个参数列表（注意要一次性传完），当第一个参数为null或undefined的时候，表示指向window，call是临时改变一次this指向，并立即执行（和apply()一样）

var arr=[1,2,4,6,7,10]
var max=Math.max.call(null,arr[0],arr[1],arr[2])
console.log(max) // 4


call和apply可直接对函数进行调用，而bind是返回一个函数，这个函数已经绑定了新的this指向，需要()来调用


显式硬绑定

    function abc(){
       console.log(this.a)
    }
    var obj = {
        a: 123,
        abc: abc
    }
    var funs = function(){
        abc.call(obj)
    }
    funs()
    funs.call(windos) // 不能修改this，因为调用funs函数本身就是在绑定this



---


数字 Number
字符串 String 对象
日期 Date 对象
数组 Array
布尔 Boolean
算数 Math



---

防抖动（Debouncing）和节流阀（Throtting）

防抖和节流是web性能优化的知识点，常见于处理触发频率较高的函数，例如：滚动条监听


防抖(debounce)

触发事件，不立刻执行，而是事件发生后一段时间在执行，延迟执行，例如setTimeout()

每触发一次，都要经过一定时间后才进行执行，而该事件再触发，那么上一个触发的时间作为新起点

可以理解为触发事件后在一定时间内只执行一次，如果在该一定时间内又触发了事件，那么重新计算



节流(throttle)

节流其实就是连续触发多次事件，但是在一定时间只触发一次函数


防抖和节流的区别就是，一个在一定时间内只执行一次，多次触发重新计算，而另一个就是在一定时间内触发一次




---



DOM事件流/事件委托


DOM事件流实质上就是页面中接收事件的顺序，当事件被触发的时候，事件在dom节点之间按照顺序传播，这个就是DOM事件流


事件被触发，执行目标该事件的处理函数，一个事件被触发，传递给自己父级，一直到window，这个叫事件冒泡



捕获事件和冒泡只能执行其中一个，例如不想执行某个事件冒泡，就可以捕获其事件，从而达到阻止冒泡的情况发生


事件对象（event）：这个对象包含了事件的相关数据

ev = event || window.event


ev.target可以获取到是说触发了这个事件，例如：


    const ev = event || window.event
    const div = document.querySelector('div')
    div.addEventListener('click', function (ev) {
        console.log(ev.target)
    })


还有preventDefault()，stopPropagation()分别用来阻止默认事件和冒泡的




事件委托

事件委托实质上就是不在子元素上设置事件监听，而是在其父元素上，利用冒泡事件来设置给子元素


在父元素上绑定事件监听，然后通过ev.target来精确找到是那个子元素触发的，然后事件冒泡到父元素上，因为父元素有事件监听器，因此触发


this会变，但是ev.target永远会精确找到触发事件的目标元素，可以不再使用this来表示当前事件触发元素了



---

DOM加载顺序（浏览器加载顺序）：解析HTML-加载外部脚本和样式表-解析脚本并且执行-构造DOM模型-加载外部文件（例如图片视频音频等等）



浏览器渲染过程


接收获取到html/css/JavaScript等资源后

浏览器解析渲染该资源，在渲染页面之前后先构建DOM树和CSSOM树

DOM树实质就是DOM对象模型，提供了对DOM的结构的方法（例如js就是通过dom来操作结构和样式等等）

CSSOM树实质就是样式对象模型，是css的对象集合

通过DOM树和CSSOM树合并为RenderObject树（RENDER树，又叫渲染树），通过一系列的布局，绘制操作，再通过浏览器来将渲染交给GPU进程来合成，最后显示页面



因为JavaScript被设计成GUI渲染和JavaScript互斥，当JavaScript执行的时候，GUI线程会被挂起，等待JavaScript执行完毕后才会执行GUI渲染操作

因此当JavaScript执行时间过长，会导致GUI渲染DOM加载堵塞


另外CSS加载也是会阻塞DOM的渲染，因为它需要等CSSOM构造完毕才会开始渲染（RenderObject树依赖于DOM树和CSSOM树）

而且CSS会在JavaScript执行之前加载执行，因此也是会阻塞JavaScript执行


关键渲染路径(Critical Rendering Path)优化

为了完成第一次渲染，应该尽量减少关键资源加载的数量，减短关键资源的获取时间，减小关键资源文件的大小

在生产环境中，应该减少不必要的注释，空格，尽量做到min文件，可以利用GZIP来压缩文件，来减小文件大小，利用缓存机制来减少获取资源的数量


注意：浏览器遇到script元素的时候，会阻止解析器执行操作，当CSSOM树构建完毕才会执行JavaScript，再解析

可以利用script元素的async属性来让其继续执行，而且不会被CSSOM阻止

script元素的defer属性可以等解析完毕（Loaded）后再执行



浏览器的回流和重绘



当渲染树（RenderTree）某个部分出现改变（例如尺寸，属性等等），浏览器会重新渲染该部分或者全部的过程叫回流（reflow）

页面第一次渲染，视窗大小发生改变，元素位置或者大小发生改变，元素字体大小发生改变，添加或删除DOM元素等等都会导致回流

例如：增加，删除，修改dom节点，移动dom位置，修改css样式（大小），改变窗口的对象（Resize）（移动端没这个问题），滚动页面，修改网页的字体等等


重绘（Repaint）：当元素不会改变其在正常流（文档流）的时候，只是改变样式，浏览器将新样式给元素并且重新绘制其


回流和重绘危害性，肯定是回流严重，一个是重新渲染，另一个只是渲染其本身





---


性能优化

减少http请求次数（缓存或者本地存储），减少http请求文件的体积，图片优化（压缩），提升http请求速度

减少dom操作（避免浏览器回流），事件委托，DOM Fragment，防抖节流，尽量不使用全局变量，布局优先选择flex

异步加载css和js，预加载preload，页面加载动画，骨架屏，路由（组件）懒加载，组件缓存（keepalive）








---



因为浏览器有同域策略，跨域会阻止，因此要解决跨域问题


跨域资源共享(CORS)

CORS可以通过http头来告诉浏览器，准许访问来自不同源的指定资源

只需要服务器设置Access-Control-Allow-Origin就可以，只有特殊跨域才需要设置

nginx配置
    location / {
        add_header Access-Control-Allow-Origin *;
    }

谷歌浏览器标记：--disable-web-security（禁用同源策略）

JSONP是json的补充，并不是官方的标准，jsonp是利用script标签和img标签之类的可以直接引用其他域的文件（没有跨域限制）

jsonp跨域，例如：

    let script = document.createElement('script')
    script.type = 'text/javascript'
    script.src = 'https://test.cjlio.com/test.js'
    document.head.appendChild(script)

node跨域

    app.all('*', function(req, res, next) {
        console.log(req.method);
        res.header("Access-Control-Allow-Origin", "*");
        res.header('Access-Control-Allow-Headers', 'Content-type');
        res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS,PATCH");
        res.header('Access-Control-Max-Age',1728000);
        next();  
    });


预检请求


CORS将请求分成2种，简单请求（simple request）和非简单请求（not-simple-request）

简单请求（满足下面的条件就是简单请求，否则就是非简单请求）：

请求方式：必须是GET请求，POST请求，HEAD请求这三个之一

请求头：请求头只能设置Accept，Accept-Language，Content-Language，Content-Type这三个字段

Content-Type：该字段的值只能设置text/plain，multipart/form-data，application/x-www-form-urlencoded这三个值


简单请求不会触发CORS预检请求，直接向目标服务器发送请求，如果满足CORS配置，服务器则返回响应，如果不是简单请求，那么浏览器会在发送请求之前发送预检请求给服务器，检查当前请求是否符合服务器的CORS配置，如果不符合则跨域报错，符合后才发送真正的请求

CORS预检请求：使用OPTIONS请求发送预检请求到服务器上，来确定服务器是否允许该请求，该请求包含了两个字段，分别是Access-Control-Request-Method和Access-Control-Request-Headers，服务器接收到请求后，发送预检请求的响应，该响应中包括Access-Control-Allow-Headers和Access-Control-Allow-Origin这些字段，来限制请求，浏览器接收到该响应后才发送真实的资源请求

（客户端）Access-Control-Request-Method表示实质请求用什么请求方式，Access-Control-Request-Headers表示实质请求携带了什么字段，由服务端来检查，该请求是否被允许

（服务端）Access-Control-Allow-Origin表示服务器限制请求的源域，没有限制为*，Access-Control-Allow-Headers表示服务器允许请求携带什么字段，Access-Control-Allow-Methods表示服务器允许用什么请求方式来请求，Access-Control-Max-Age表示该预检请求的响应可以缓存多久（在该时间段内不用再发送同一请求的预检请求）


注意：浏览器有个最大有限时间，Access-Control-Max-Age响应头（服务端响应），该响应头表示了本次预检请求的响应可以被缓存多久，单位为秒（不同浏览器最大有限时间都不同，Firefox为86400秒，目前最新chromium为7200秒），在这个时间段内浏览器不需要再为同一个请求发送预检请求











---


浏览器事件循环




浏览器缓存

---




json全成JavaScript Object Notation，用来数据交互，以键值对方式，多个数据用逗号分隔

json支持字符串，数值，布尔值，数组，对象，null



JSON.stringify() json转为字符串

JSON.parse() 字符串转为json

JSON字符串：和普通字符串没有区别，只是符合语法规则

例如：

    let objStr = '{"name":"root","pass":"admin","true": null}'
    let objArr = '[{"name": "boom","pass": "hallo"},{"name": "aaa","pass": "yes"}]'
    let obja = JSON.parse(objStr)
    let objb = JSON.parse(objArr)
    console.log(obja)
    console.log(objb)


获取属性和值

通过对象名.属性名来获取或者修改，例如obja.name

也可以通过对象名[属性名]来获取或者修改，例如obja["name"]

删除对象属性，delete obja.name


JSON转JavaScript对象（利用eval()来解析json文本）

    let objStr = '{"name":"root","pass":"admin","true": null}'
    let objArr = '[{"name": "boom","pass": "hallo"},{"name": "aaa","pass": "yes"}]'
    let obja = eval ("(" + objStr + ")")
    let objb = eval ("(" + objArr + ")")
    console.log(obja)
    console.log(objb)


JSONP
postMessage()
websocket
Node中间件代理
cors
nginx反向代理
window.name + iframe
location.hash + iframe
document.domain + iframe




---


执行上下文和执行栈（调用栈）


JavaScript程序执行分2个阶段，预编译阶段和执行阶段，而执行上下文就是在执行阶段创建完成的

预编译阶段主要做一些预处理工作，例如创建变量对象（变量提升），执行阶段就会激活这些变量对象（赋值）

声明阶段会在编译时提升到最上面，当声明完成后，才会执行赋值操作，例如：

    var a = 1

上面会被编译器处理为

    var a
    a = 1

因此可以在编写时在未定义就使用该变量，改变编写时的位置（不再是从上往下依次执行），这就是变量提升

函数声明可以被提升，而且函数表达式不会被提升，例如：

    abc()
    function abc(){
        console.log('hallo word')
    }
    xyz() // xyz is not a function，并不是这个不是函数，而且没有声明，因此这个没有得到提升
    var xyz = function(){
        console.log('hallo hahaha')
    }

只有声明本身被提升，赋值和其他操作并不会被提升，而且按照从上往下依次执行，提升只影响声明

声明提升，函数优先，例如：

    abc() // hallo word
    var abc
    abc = function(){
        console.log('hallo hahaha')
    }
    function abc(){
        console.log('hallo word')
    }

可以看到abc变量最先出现，但是反而输出了hallo word，说明函数声明比变量声明提升的更早，变量abc因为重复声明，被忽略了




执行上下文：指的是抽象化JavaScript代码执行的环境，当JavaScript代码被执行的时候，都是在执行上下文中运行的

在JavaScript中有3种执行上下文，分别是全局执行，函数执行，Eval函数执行


全局：不在函数内部的代码都在全局执行上下文中，一个程序只有一个全局执行上下文

函数：当某个函数被调用时，为该函数创建一个函数执行上下文（如果被调用多次，会根据定义的顺序来执行（执行栈））

Eval函数：在eval函数内部执行的代码也有自己的执行上下文


执行栈（调用栈）：指的是一种存在后进先出（LIFO）的数据结构的栈，该栈被用来存储程序运行的时候被创建的全部执行上下文。在JavaScript编译代码的时候，创建一个全局的执行上下文，将这个执行上下文存储在当前执行栈中，只要当碰到函数被调用，再为该函数创建一个新的执行上下文并且存储在栈的顶部，然后执行在顶部的函数，函数执行完毕，该执行上下文从栈中弹出，轮到下一个执行上下文进行，一旦全部代码都执行完毕，全局执行上下文弹出


执行上下文会经历创建和执行俩个阶段，创建阶段会创建变量环境和词法环境，this绑定（在全局执行上下文中，this的值为全局对象，如果是浏览器的话，this为window对象，在函数执行上下文中，this取决于该函数怎么被调用的，如果是被一个对象调用的，那么this指向该对象，否则就是全局对象）


词法环境：一种规范类型，用来定义变量名或者函数名和实际对象的关联，一个词法环境有两个组件，分别是环境记录器（用来存储变量和函数声明的实际位置），外部环境的引用（可以访问父级词法环境）

词法环境有俩种类型：全局和函数，全局环境（全局执行上下文）是不存在有外部环境引用的，该环境拥有用户定义的全局变量，并且this执行全局对象。而在函数环境中，在函数内部定义的变量会被存储在环境记录器中，引用该环境的外部环境可能是全局，又或者是其他包含该函数的外部函数

环境记录器也是有俩种，分别是声明式（函数环境）和对象式（全局环境），声明式会存储变量，函数以及参数（还会传递arguments（存储索引和参数映射）和length（函数参数））。而对象式被用来定义出现在全局上下文中的变量和函数之间的关系

变量环境：在ES6中，只有使用var定义的变量才能使用变量环境来存储（let和const则是词法变量），在创建阶段，变量初始值为undefined（var）和未初始化（let和const）



JS底层运行机制：EC/VO/AO/GO/EC/ECS/Scope/Scope Chain/Lexical Environment/Variable Environment

JS执行环境（EC），变量对象（VO），活动对象（AO），GO对象，作用域（scope）作用域链（scope chain）

JS执行环境（EC），EC全称为Execution Context，又叫执行上下文

变量对象（VO），VO全称Variable Object，该对象存储了当前上下文中定义的变量和函数，在全局作用域中，变量对象就是全局对象

活动对象（AO），AO全称Activation Object，该对象在进入函数执行上下文时被创建，AO对象存储了该函数的参数变量


GO对象，GO全称Clobal Object，该对象存储了所有调属性和方法，并且在全局中创建一个window变量指向这个GO对象，因此GO对象实质上就是全局对象


ECS：ECS全称Execution Context Stack，就是执行上下文栈，又叫执行栈

Scope：作用域

Scope Chain：作用域链

Lexical Environment：词法环境

Variable Environment：变量环境


函数的运行：

编译阶段：创建一个堆内存，声明当前函数的作用域，作用域和执行上下文相关，将函数内部语句块以字符串的形式存储在堆内存中，函数名指向该函数的堆内存地址

执行阶段：创建函数执行上下文（私有），在函数执行上下文中存储活动对象（AO），初始化操作（初始化作用域链，this指向，arguments，形参赋值，变量提升），执行语句（调取堆内存中存储的字符串，以上下文的形式依次执行）

Scope Chain查找机制：检查在执行阶段的变量是否为私有变量，如果都是私有，那么直接操作私有，如果不是私有的，按照Scope Chain，向上级作用域查找，一直查找到全局作用域为止


---



所谓的变量提升只是在编译阶段识别被创建的变量，在这个阶段中使用var和function定义的变量初始化的值被设置为undefined，而使用let和const定义的变量还是未初始化（暂时不能使用的状态），直到定义被执行才被初始化


JavaScript是即时编译即时执行的机制，在执行一个函数之前，都会立刻编译该函数，然后创建执行上下文，在执行上下文中有个存储空间叫变量环境，这个环境存储着由var定义的变量，因为var定义的变量都在编译的时候已经存储了，在执行时的任何时间段都是可以访问调用的

而使用const和let定义的变量，在编译完成的时候，执行之前，将变量名存储在词法环境中，在没有声明该变量之前是会存在暂时性死区（变量已经被上下文记录，但是不能使用）


---

Eventloop（事件循环）：事件循环实质上就是避免js单线程的时候出现堵塞，也是异步的原理

a programming construct that waits for and dispatches events or messages in a program.

Event Loop 任务分为两种，分别是宏任务（MacroTask）和微任务（MacroTask）

所有同步任务会根据顺序被主线程依次执行，执行栈（execution context stack），而除主线程外，还有一个任务队列（task queue），当碰到异步任务完成了，任务队列就会告诉主线程，某个异步任务可以执行，这时这个异步任务才会进入主线程执行（异步任务结束等待，进入执行栈，开始执行），主线程反复操作，这就是事件循环

宏任务：整体代码script，setTimeout和setInterval，I/O，UI Rendering（ui渲染），setImmediate（node的特性，浏览器已抛弃该API），requestAnimationFrame，事件

微任务：Promise.then，process.nextTick（node的特性），MutationObserver

在浏览器上：JavaScript会有一个主线程和执行栈（call-stack），全部任务都会放到执行栈等主线程执行

js的执行栈是后进先出的数据结构，当某一个函数执行时，将插入到栈的顶部，当执行完毕，从栈顶部弹出。一直到栈被清空，执行栈的所有同步任务执行完成，开始查看任务队列，执行那些异步任务（只有当主线程空了，才能执行任务队列里面的）

而任务队列是先进先出的数据结构，只有异步任务才能进入任务队列（同步任务在主线程上排队），异步任务必须指定回调函数，主线程执行异步任务实质上就是在执行对应的回调函数。当某个异步任务完成，就会向任务队列添加一个事件，在顶部的任务，会优先被主线程处理


例如：

    new Promise((a) => a())
        .then(() => console.log("微任务1"))
        .then(() => console.log("微任务2"))
    console.log('hallo 我是同步任务1')
    void setTimeout(() => {
        console.log("我是定时任务1 宏任务")
    }, 1000)
    void setTimeout(() => {
        console.log("我是定时任务2 宏任务")
    }, 0)
    void setTimeout(() => {
        console.log("我是定时任务3 宏任务")
    })
    setTimeout(function() {
        console.log('我是宏任务，good bye')
    }, 1000)
    Promise.resolve()
        .then(() => console.log("微任务3"))
        .then(() => console.log("微任务4"))
        .then(() => console.log("微任务5"))
    console.log('hi 我是同步任务2')


微任务列表是先进先出，先进入的，就先执行


运行过程：同步任务执行（主线程），当同步任务为null，检查任务队列是否为null，不为null，宏任务（script任务）进入执行栈，在主线程执行前进行判断，（微任务和宏任务都是异步任务），如果微任务队列不为null（script任务执行过程中遇到微任务，将微任务添加到微任务排列），优先执行微任务，当微任务都执行完毕，执行下一个宏任务


setTimeout定时器最小延迟时间为4ms，在4ms以内的定时都将视为同一个最小延迟时间，将根据先进先出处理

例如：

    setTimeout(function() {
        console.log('hallo')
    }, 1)
    setTimeout(function() {
        console.log('hhhh')
    }, 0)
    console.log('abcabc')





---


DOM事件级别

DOM0 函数赋值到一个事件中（可以绑定多个同类型事件），例如onclick=function(){}或者在元素内添加事件

DOM2 使用监听，可以移除或者添加监听，例如addEventListener('click', test, false)

DOM3 在DOM2级基础上添加更多事件类型，例如键盘事件addEventListener('keyup', test, false)


DOM1因为没有定义事件相关的内容，所有没有DOM1级模型

捕获（从祖先元素捕捉到子元素，精确到目标元素）
冒泡（从目标元素往上冒泡，一直到window元素）


---


创建对象的方法

let a = {data: "hallo word!!!"}
let b = new Object({data: "hallo word!!!"})
let c = function(){this.data="hallo word!!!"}
let d = new c()
let e = Object.create(a)

继承的方法

通过原型链继承

    function test(){
        this.data = 123
    }
    function main(){
        this.type='main'
    }
    main.prototype = new test() // main.prototype.__proto__ = test.prototype
    console.log(new main)

缺点就是如果进行修改对象，那么其他通过其父类实例继承的对象，都会被修改(引用类型），而且也不能向父类构造函数传递参数



通过构造函数继承



    function test(){
        this.data = 123
    }
    function main(){
        test.call(this)
        this.type='main'
    }
    console.log(new main)


实质上是通过改变test的this指向，将其指向到main，这个只能继承父类构造函数的属性，不能继承原型链上的方法（prototype）



盗用构造函数（对象伪装/经典继承）


    function test(){
        this.data = [123,666,"hahahah"]
    }
    function main(){
        test.call(this)
    }
    let a = new main()
    let b = new main()
    a.data.push("hallo word")
    console.log(a,b)

会发现a和b的data都是独享的，不会存在改变a，b也跟着改变，原理就是通过call改变this指向，指向当前的test，因为使用的call，还可以给父类传递参数，缺点就是无法复用父类方法，因为每次实例化，都要重新声明父类的方法


组合继承（伪经典继承，将原型链和盗用构造函数方式结合）

原理就是通过原型链继承父类的属性和方法，再用盗用构造函数继承实例的属性（可以确保方法可复用，每个实例都有自己的独享属性）


    function test(){
        this.data = [123,666,"hahahah"]
    }
    function main(){
        test.call(this)
    }
    main.prototype = new test()
    let a = new main()
    let b = new main()
    a.data.push("hallo word")
    console.log(a,b)

通过盗用构造函数继承父类的data属性，然后通过原型链，继承父类的原型对象

缺点就是会调用2次test构造函数（分别是在new，call各调用一次），而且实例和原型都存在相同的属性和方法



原型式继承（实例继承）（直接给对象赋值给构造函数的原型）

    function test(data){
        function hallo(){}
        hallo.prototype = data
        return new hallo()
    }




寄生式继承

    function test(data){
        let hallo = Object.create(data)
        hallo.abc = function(){
            console.log(this.name+this.pass)
        }
        return hallo
    }
    let data1 = {
        name:"root",
        pass:"hallo"
    }
    let main = test(data1)
    main.abc()

通过接收参数，该参数为被继承的对象，main是基于data1对象创建一个新对象，这个对象有data1的属性和方法，也有自己的abc方法，继承父类之后，进行增强了



寄生式组合继承（组合继承加寄生式继承）

    function test() {
        this.data = [123,666,"hahahah"]
    }
    function main() {
        test.call(this)
    }
    main.prototype = Object.create(test)
    let a = new main()
    let b = new main()
    a.data.push("hallo word")
    console.log(a,b)




extends继承（es6继承，通过extends关键字继承）


    class test {
        constructor(name="root",data="hallo") {
            this.name = name
            this.data = data
        }
        hallo(){
            console.log(`${this.name} ${this.data}`)
        }
    }
    class main extends test {
        constructor(name="admin",data="hahaha") {
            super(name,data)
        }
        hallo(){
            super.hallo()
        }
    }
    let abc=new main('user')
    abc.hallo()


通过extends关键字创建父类的实例this，然后在子类class中修改this，必须通过父类构造函数来得到父类的属性和方法（super方法）



---


浏览器缓存的分类

强缓存（本地缓存，不用请求，直接拿来用）， expires和control

expires是http1.0定义的缓存字段，expires字段为资源的过期时间，这个字段是时间戳，这个会导致客户端时间和服务端时间之间时间差问题（因为请求的时候使用的客户端的事件）

control（cache-control）是http1.1中定义的字段，用来解决expires存在的问题，该字段是一个时间长度，表示多少秒后过期（相对于客户端），当expires和cache-control都存在时，cache-control优先级高



协商缓存（当拿到该缓存，会向服务端进行确认，确认其是否要用，缓存有没有更新），last-modified和etag


last-modified会在获取到某资源的时候记录资源的修改时间，当第二次获取到的时候，会对比时间，精确到秒（1秒内的更新，不会被记录），如果对比修改时间，发现没有修改，那么继续使用该缓存并且返回304，如果被修改那么重新获取，并且记录新时间，而且存在时间上修改了，但是内容没有修改的问题，etag就是解决这个的



etag会基于内容编码生成唯一的标识，只要内容不一样，标识就一定不一样，判断是否被修改，直接判断标识是否一致，一致则直接使用缓存并且返回304，不一致就是获取新的缓存，并且记录etag值


---



因为JavaScript没有类型声明，可以进行类型的强制转换，手动转换（明确转换）的叫显式转换类型，自动转换（隐式，由编译器自动处理）的叫隐式转换类型


Object.prototype.toString()返回表示该对象的字符串

Object.prototype.valueOf()返回对象的原始值

Symbol.toPrimitive效果和valueOf()类似，只是优先级比valueOf()高，并且接受一个hint参数（该参数被用来指定要转换的原始值的具体类型）


隐式转换过程：优先调用对象的valueOf()方法，返回原始值，如果不能处理则调用对象的toString()方法转换为字符串，进行字符串拼接，否则报错（cannot convert object to primitive value）





隐式类型转换例子：

    true + 1 === 2 // true

布尔值加整数，居然绝对等于（类型和值都等于）一个整数，这说明类型已经被转换了


+加号不但可以用来算术运算，还可以做字符串拼接

字符串加数字，数字自动转字符串

    "123" + 666 // '123666'

数字减字符串，字符串自动转数字，如果字符串不是纯数字，那么输出为NaN

    123 - "100" // 23


相等

==，当类型不相同时，进行类型转换，再进行比较

===，首先进行判断类型是否相同

    undefined==null // true

    "0" == false // true

比较

    "3" > "10" // true

字符串和字符串比较就算了，为啥字符串3大于字符串10，类型转换了也不应该啊

实质上并没有做类型的转换，因为其比较的时候调用了charCodeAt()方法，返回值是整数，这个整数表示则该字符的Unicode编码

所以实质上是"3".charCodeAt() > "10".charCodeAt() 

表示"3"的Unicode编码为51，"10"的为49

charCodeAt()方法可以将字符串转Unicode编码，该方法必须要写index参数，如果没写默认参数值为0

    let abc = "你好"
    console.log(abc.charCodeAt(1)) // 22909 该值为"好"的Unicode编码



同样也可以将Unicode编码转字符串fromCharCode()方法

    let a = String.fromCharCode(51)
    console.log(a) // 3
    console.log(typeof(a)) // 'string'


---




proxy对象属于一种元编程（meta programming），proxy可以拦截对象，外部进行调用该对象时，必须通过这个拦截，这个拦截器可以对外部的调用进行过滤和查找赋值等等，另外Vue3.0的数据双向绑定（响应式）实现原理就是proxy

ES6提供了Proxy构造函数，来new Proxy()实例，Proxy实例具备有2个参数，分别是target, handler

let proxy = new Proxy(target, handler)


target参数是要拦截的对象，handler参数为拦截后的行为（也是对象）

Proxy支持13种拦截行为，分别是：

get()：读取属性

set()：修改属性

has()：in操作符，判断某个属性是否存在于这个对象之中

apply()：当Proxy拦截函数时

construct()：当使用new关键字时

deleteProperty()：当使用delete删除对象的属性时

defineProperty()：当使用Object.defineProperty修改属性修饰符时

ownKeys()：当使用Object.getOwnPropertyNames()，Object.getOwnPropertySymbols()，Object.keys()，Reflect.ownKeys()获取对象的信息时

getPrototypeOf()：当使用Object.getPrototypeOf()读取对象的原型信息时

setPrototypeOf()：当使用Object.setPrototypeOf()设置对象的原型时

isExtensible()：当使用Object.isExtensible()判断对象是否可以添加新的属性时（Reflect.isExtensible()操作也会触发）

preventExtensions()：当使用Object.preventExtensions()设置对象不能添加新的属性时

getOwnPropertyDescriptor()：当使用Object.getOwnPropertyDescriptor()获取该对象的自有属性（自有属性指该对象所有，不需要通过原型链获取该属性）的属性描述时


这里只写最常用的get()和set()


    const abc = {
        name: "chenjunlin",
        pass: "123456789"
    }
    const proxy1 = new Proxy(abc, {
        get(target, property, receiver) {
            if (property in target){
                return target[property]
            }else{
                return '读取失败'
            }
        },
        set(target, property, value, receiver){
            
            target[property] = value
            console.log(`${property}属性被设置值`, value)
            
            
        }
    })
    console.log(proxy1.name)
    console.log(proxy1.pass)
    console.log(proxy1.hallo)
    proxy1.name = "root"
    console.log(proxy1.name)
    proxy1.pass = "123"
    console.log(proxy1.pass)


target是拦截对象，property是被拦截获取到的属性，value是新的属性值，receiver是Proxy对象本身（指向this对象，如果是继承的，那么该值为继承的Proxy对象）



Reflect是提供拦截对象操作的方法，其不是构造函数，不可通过new创建和调用Reflect，其拦截的方法和Proxy相同

例如：

    const obj = {
        name: "root",
        pass: "123"
    }
    Reflect.get(obj, "name")




---


Babel编译工作

Babel的核心功能就是编译ESNext，进行降级，保证兼容性，编译的核心就是通过AST（语法抽象树）对源码进行分析，并且转换为目标代码，解析成AST是方便计算机理解源码是干什么的，而Babel解析器是Babylon

像解析let const时，直接转换为var，并且判断编译阶段的const是否存在二次赋值，如果存在就是报错，块级作用域通过立即调用函数实现（例如匿名函数），针对暂时性死区，使用JavaScript严格模式实现


---


前端性能指标

首次绘制时间（FP）：页面发生第一次绘制时间，首次不同于上次跳转之间的内容

首次有内容绘制时间（FCP）：浏览器渲染完成DOM的部分内容（可以是文本，也是可以是其他带内容的（或者说有视觉效果的））的时间

首次有意义绘制时间（FMP）：页面关键内容的绘制时间（根据不同需求而定）

首屏时间：完成整个屏幕内容渲染的时间（不包括滚动）

用户可交互时间：用户可与页面进行交互的时间，DOMReady（html元素转换为DOM树完成（DOMContentLoaded事件），不包括加载外部文件）


视觉稳定性指标（CLS，Cumulative Layout Shift）：指页面从一帧到另一帧时，不稳定元素的偏移量（也叫布局偏移量）

FID（First Input Delay，首次输入延迟）

PSI（Perceptual Speed Index，视觉变化率）

FPS（Frames Per Second，每秒显示帧数）


白屏时间：从页面发送刷新或者跳转后到页面出现第一个字符的时间

白屏时间FP = domLoading - navigationStart

可能导致白屏时间过长的原因：dns查询，tcp请求连接，服务端响应速度，关键资源下载，解析，渲染，资源体积


首屏时间（服务端渲染应用用DOMContentLoaded时间，客户端渲染应用MutationObserver）：白屏时间加渲染时间

MutationObserver接口可以监视DOM元素，因为客户端渲染应用是在客户端通过ajax请求的方式进行显示内容的，DOMContentLoaded时间并不能实质表示客户端渲染应用的首屏时间，应该监听某个最后的DOM元素，当其加载完成了，这个时间才算首屏时间，当然MutationObserver并不能计算图片加载时间，因为图片加载是异步的

首屏时间数据：平均值，分位值（例如P99，将全部首屏时间排序，第99位的首屏时间就是P99，除了P99，还有P50,P90）,秒开率（1秒内打开的用户的比率，还有1.5秒开率，2秒开率）

首屏时间优化：懒加载（加载关键内容，延迟加载其他内容，按需加载），缓存（减少重复请求），离线化，请求并行化（域名散列或者HTTP2.0多路复用）


DNS查询优化：浏览器提供预获取DNS接口，可在浏览器建立DNS缓存，减少DNS查询所耗费的时间

DNS查询优先级：浏览器缓存>hosts>路由器缓存>DNS服务器缓存（ISP）>根服务器缓存

Chrome浏览器默认缓存60秒，如果之间访问过了该域名，那么在60秒内将不会重新获取DNS缓存，而是直接调用浏览器的DNS缓存，减少DNS查询耗费的时间

开启DNS预解析
<meta http-equiv="x-dns-prefetch-control" content="on"/> 

尝试对a.cjlio.com域名做预解析（不能用来对当前域名做预解析，因为当得到这个资源时，早就得到当前域名的解析IP）
<link rel="dns-prefetch" href="https://a.cjlio.com"/> 

预连接

<link rel="preconnect" href="https://a.cjlio.com">


预加载（会提升该资源加载的优先级，加载和执行是不同的，加载完成并不会执行，需要手动执行）

<link rel="preload" as="script" href="./jquery.min.js"/>
<script src="./jquery.min.js"></script>

预判加载（会降低该资源加载的优先级，因此只有当空闲时才会加载这个）

<link rel="prefetch" as="script" href="./jquery.min.js">


HTTP请求优化：解决请求阻塞（请求阻塞（Stalled）是浏览器为了确保访问速度，对同一域名下的资源限制在一定的请求数，超过该请求数就会阻塞）

以目前版本的Chrome浏览器来说，其最大请求数是6个（http1.1），超过6个请求数时，只能等待前面的请求后再进行请求，请求阻塞只针对同一域下的，只需要将资源用不同的域名散列（实质请求数限制将表示为域名数 * 浏览器最大请求数）

例如将某个静态资源服务器，分成多个域名，例如，a.cjlio.com，b.cjlio.com，那么请求数就是可以达到12个



资源Gzip压缩，避免301和302重定向，骨架屏











性能数据获取

window.performance

    const TimeData= window.performance
    TimeData.memory // 该字段表示JavaScript对内存的占用
    console.log(TimeData.memory)


memory字段有三个属性（这些属性都是只读的，而且只有Chrome内核的浏览器才有这些属性，是非标准），分别是jsHeapSizeLimit（内存大小的限制），totalJSHeapSize（可以使用的内存），usedJSHeapSize（JavaScript对象占用的内存（包括V8内部对象））

    console.log(TimeData.navigation)

navigation字段有两个属性，分别是redirectCount（获取某个资源需要重定向几次才能得到）和type（该属性有4个值，0表示是通过正常访问的（TYPE_NAVIGATENEXT），1表示通过刷新访问的（TYPE_RELOAD ），2表示通过前进后退按钮访问的（TYPE_BACK_FORWARD，历史记录），255表示不是这些方式之外访问的（TYPE_UNDEFINED））

timeOrigin字段

    const datea = new Date(TimeData.timeOrigin)
    console.log(datea)
    console.log(TimeData.timeOrigin)
返回性能测量开始时的高精度时间戳



timing字段虽然已被w3c标准抛弃，但是这玩意的接口很强大

    console.log(TimeData.timing)

可以看到输出了21个属性，这些属性分别是

connectEnd: http连接成功的时间（建立连接）
connectStart: http开始建立连接的时间（开始连接）
domComplete: dom解析完毕的时间，资源也准备就绪（Document.readyState值为complete）
domContentLoadedEventEnd: DOMContentLoad事件结束时间，dom解析完毕，并且资源加载完毕
domContentLoadedEventStart: DOMContentLoad事件开始时间，dom解析完毕，开始加载资源
domInteractive: 解析html文档，dom树创建完毕的时间，资源还没加载（Document.readyState值为interactive）
domLoading: 开始解析渲染dom树的时间（Document.readyState值为loading）
domainLookupEnd: DNS查询域名结束时间（当使用了本地缓存时或者持久连接，不需要dns查询，该值等于fetchStart的值）
domainLookupStart: DNS查询域名开始时间（当使用了本地缓存时或者持久连接，不需要dns查询，该值等于fetchStart的值）
fetchStart: 浏览器准备发起第一个资源请求的时间（检查缓存之前）
loadEventEnd: load事件结束的时间
loadEventStart: load事件开始时间
navigationStart: 前一个页面的unload时间（当前窗口的前一个页面的关闭并且发生unload事件时），如果没有前一个页面，则表示fetchStart时间
redirectEnd: 最后一个http重定向发生时的时间
redirectStart: 第一个http重定向发生时的时间
requestStart: http连接完成并且请求真实文档的时间
responseEnd: http返回响应并且全部接收完成的时间（接收最后一个字节）
responseStart: http开始接收响应的时间（接收第一个字节）
secureConnectionStart: http连接开始的时间（和connectEnd类似），如果不是安全连接返回0
unloadEventEnd: 前一个页面中的unload事件所绑定的回调函数执行完毕的时间
unloadEventStart: 前一个页面中的unload事件触发的时间，如果没有前一个页面或者前一个页面和当前页面不在一个域中时，返回0

单位： Unix毫秒时间戳（基于UTC1970.01.01 00:00:00到现在的总毫秒数）

重定向时间：redirectEnd - redirectStart

dns查询时间：domainLookupEnd - domainLookupStart

tcp建立连接并且完成握手时间：connectEnd - connectStart

内容加载完成时间：responseEnd - requestStart

DNS缓存时间：domainLookupStart - fetchStart

卸载页面的时间: unloadEventEnd - unloadEventStart

request请求耗费时间（重定向）：redirectEnd - redirectStart

解析DOM树耗费的时间：domComplete - responseEnd

白屏时间：domLoading - fetchStart

用户可交互时间：domContentLoadedEventEnd - fetchStart

前一个页面卸载耗费的时间：unloadEventEnd - unloadEventStart

onload回调函数的执行时间：loadEventEnd - loadEventStart

用户等待页面完全可用时间（页面加载完成）：oadEventEnd - navigationStart




---




cookie

cookie默认是临时的，浏览器关闭进程就会自动销毁，并且cookie是纯文本

通过document.cookie = "xxx"来修改cookie

cookie存储大小限制在4kb


---


Fetch api允许在JavaScript发起http请求和响应，使用Promise


    fetch('https://api.github.com/')
        .then(response => response.json())
        .then(data => console.log(data))
        .catch(err => console.log('Request Failed', err))

可以看到fetch接收一个url参数，并且具备then和catch方法，并且response接收到数据是stream对象，并且将这个stream对象转换为json对象

fetch只有当网络故障或者请求被阻止时才报错，因此不管是否请求成功（例如404或者502）

response对象还有一些属性，例如response.ok（表示是否请求成功，http状态码在200-299的范围时），response.status（http状态码），response.url（请求的url，如果有跳转，那么该属性值为最终的请求地址），response.redirected（请求是否发送跳转），response.headers（请求头）等等

例如：
    async function getData(url = "",data={}){
        const response = await fetch(url, {
            method: 'GET',
            headers: {
                "Content-type": "application/x-www-form-urlencoded",
            },
        })
        return response.json()
    }
    getData('https://api.github.com/')
    .then(data => {
        console.log(data)
    }) 




---






---




基础类型存储在栈内存中，在引用或者拷贝时，创建一个完全相等的变量

引用类型存储在堆内存中，存储的是内存地址，多个引用可以指向同一个地址（内存共享）（return会改变引用的地址）


类型检测

typeof（不能用来检测null类型，null不是object类型，另外object类型除了function可以检测出来外，其他都是objet，因此typeof可以检测基础类型以及function，其他类型就不适合了））

instanceof（不能正确判断基础数据类型，但是可以判断引用类型）



在不知道数据类型的情况下做数据类型判断，可以先用typeof abc !=='object' || abc !==null筛选出基础类型（null除外，如果是null直接返回null），如果是基础类型直接用typeof获取数据类型，如果不是再用instanceof判断引用类型


Object.prototype.toString（该方法可以返回某值的数据类型，返回值类型是字符串，格式统一为[object 数据类型]），如果不想要object，只要后面的数据类型，只需要将返回值进行正则一下就是可以了（replace(/^\[object(\S+)\]$/, '$1')）


    function a(){
        console.log("hallo")
    }
    let b = {
        name: "abc",
        pass: "123"
    }
    let c = [1,3,5,7,9]
    let d = new a
    let e = 'hallo word'
    console.log(typeof a) // function
    console.log(typeof b) // object
    console.log(typeof c) // object
    console.log(a instanceof Function) // true
    console.log(b instanceof Object) // true
    console.log(c instanceof Array) // true
    console.log(d instanceof a) // true
    console.log(e instanceof String) // false
    console.log(Object.prototype.toString.call(null)) // [object Null]
    console.log(Object.prototype.toString.call(e)) // [object String]



数据类型转换（显式类型转换（强制类型转换）和隐式类型转换（自动类型转换））


强制类型转换方法：Number()，Boolean()，String()，parselnt()，toString()，parseFloat()

Number()规则：如果值是布尔值，true转1，false转0。如果是整数，返回自身。如果是null，返回0。如果是undefined，返回NaN。如果是字符串，包含整数，转换为十进制，包含浮点格式，转换浮点数，空字符串为0，其他情况均为NaN。如果是Symbol，抛出错误，如果是对象，调用该方法，如果返回值为NaN，调用其对象的valueOf()方法

Boolean()规则：undefined，null，0，+0，-0，''，false，NaN均为false，其他情况都为true



隐式类型转换

==：如果类型相同，不转换类型，如果有个值为null或者undefined，只有当另一个是null或者undefined，才返回true，否则都为false，如果有个值为Symbol类型，返回false。如果俩个值为Sting和Number类型，Sting类型的那个将转换为number类型，如果有值的类型为boolean，则转换为number，如果有值类型为object，另一个值为number，string或者symbol时，先将object转换为原始类型再判断

传说中的console.log(abc==1 && abc==2 && abc==3)，就是利用了==的隐式类型转换规则，这个abc实质上是个对象，==隐式类型转换如果是对象，会先尝试将其转换为基础类型，如果不行会调用其valueOf()方法，而且这个valueOf()方法就是定义其返回值的，并且通过this获取当前被调用的返回值

    let abc = {
        value: 0,
        valueOf: function(){
            this.value++
            return this.value
        }
    }
    console.log(abc==1 && abc==2 && abc==3)


+：加号被用于数值相加和字符串拼接，如果两边值都是字符串，则直接拼接字符串。如果有个是字符串，另一个是unll，Boolean类型或者undefined时，调用toString()方法（该方法的返回值为字符串类型的），然后再进行字符串拼接。如果有个是数值，另一个是number类型，则进行加法处理，如果另一个是null，undefined，boolean则转换为number类型再加法处理。如果一个是number类型，另一个是string类型，则将number类型那个转换为string类型，然后再字符串拼接


object隐式转换规则：如果有Symbol.toPrimitive()方法，则优先调用该返回再返回，如果没有该方法，并且object不能转为基础类型，则自动调用valueOf()方法，如果该方法返回了基础类型，则返回，如果还是不行则调有toString()方法（实质上就是返回其字符串），如果能转换为基础类型，则返回，如果还是没有返回基础类型，则抛出错误









---






浅拷贝：如果是基础类型，拷贝的是基础类型的值，如果是引用类型，拷贝的是内存中的地址，如果改变该引用类型的值，会影响使用了该内存地址的全部引用类型的值

Object.assign()方法可以实现浅拷贝（不拷贝继承属性，不拷贝不可枚举属性，但是可以拷贝Symbol类型的对象）

对象的不可枚举属性通过enumerable: false设置

    let hallo = {}
    let hi = {
        name: 'admin',
        pass: 'abc123'
    }
    Object.assign(hallo,hi)
    console.log(hallo)
    hi.name = 'xiaochen'
    console.log(hallo)
    console.log(hi)


扩展运算符(...abc)


    let hi = {
        name: 'admin',
        pass: 'abc123',
        data: {
            id: 1,
            text: "hallo word"
        }
    }
    let hallo = { ...hi }
    console.log(hallo)
    console.log(hi)
    hi.name = 'user'
    console.log(hallo)
    console.log(hi)
    hi.data.id = 10
    console.log(hallo)

扩展运算符，只有单层对象属性或者数组时，是深拷贝，修改其属性，不会影响到其他使用该对象的属性，如果是一层以上的数据时，会变成浅拷贝，只能影响一层以上的数据，在一层的数据不受影响


concat()方法（可用于浅拷贝数组）


    let hallo = [2,4,5,6,7,8,9,12]
    let hi = hallo.concat()
    console.log(hallo)
    console.log(hi)
    hi[0] = 66
    console.log(hallo)
    console.log(hi)


slice()方法（可用于浅拷贝数组）

    let hallo = [{data: 66},2,4,5,6,7,8,9]
    let hi = hallo.slice()
    console.log(hallo)
    console.log(hi)
    hi[0].data = 123
    console.log(hallo)
    console.log(hi)


Object.assign()（将所有可枚举属性的值从原有对象拷贝到目标对象中）

    let obj = {
        name: 'xiaochen',
        pass: 'abc123',
        data: {
            id: 1,
            text: "hallo word"
        }
    }
    let obja = Object.assign({},obj)
    console.log(obja)

该方法缺陷就是只能拷贝可枚举的属性， 好处就是Symbol类型的属性可以被拷贝



手写浅拷贝（通过传入目标对象参数，得到一个经过浅拷贝的对象）


    let obj = {
        name: 'xiaochen',
        pass: 'abc123',
        data: {
            id: 1,
            text: "hallo word"
        }
    }
    let objfu = (obj) =>{
        if(typeof obj ==='object' && obj !== null){
            // 使用typeof判断obj参数是否是对象，并且还不是null
            let obja = Array.isArray(obj)?[]:{} // 使用Array.isArray()判断obj是否是数组（是数组返回true，否则返回false），这里是一个三元表达式（为true时返回[]，为false时返回{}）
            for(let i in obj){ // in关键字遍历obj的属性（或者数组）
                if(obj.hasOwnProperty(i)){ // hasOwnProperty方法可以判断该对象是否存在某个属性，存在则返回true，否则返回false
                    obja[i] = obj[i] // 判断存在后，将obj的属性拷贝到obja的属性中（浅拷贝），属性值和属性一一对应
                }
            }
            return obja
        }else{
            return obj
        }
    }
    console.log(objfu(obj))
    let hhh = objfu(obj)
    hhh.data.id =66
    console.log(objfu(obj))
    console.log(hhh)

深拷贝：创建一个新对象，然后复制原有的对象的基本类型值，其完全是在堆内存中创建新的内存地址，不受原有的对象的改动影响


JSON.stringify()，通过生成一个json字符串，json字符串再转为新的对象

    let obj = {
        name: 'xiaochen',
        pass: 'abc123',
        data: {
            id: 1,
            text: "hallo word"
        }
    }
    let str = JSON.stringify(obj)
    let obja = JSON.parse(str)
    console.log(obja)
    obj.data.id = 66
    console.log(obja)
    console.log(obj)


注意：如果对象中的值有函数，undefined，symbol类型，使用JSON.stringify()会导致json字符串中的这个键值对会消失，不能拷贝不可枚举属性，Data类型变成字符串，不能拷贝对象原型链，拷贝RegExp类型时会变成空对象，不能拷贝正则表达式，另外对象的属性值含有NaN，Infinity（Infinity表示无穷大），-Infinity（-Infinity表示负无穷大）时，JSON序列化后变成null，不能拷贝对象的循环应用（对象成环 ，obj[key] = obj）



手写递归深拷贝

    let obj = {
        name: 'xiaochen',
        pass: 'abc123',
        data: {
            id: 1,
            text: "hallo word"
        }
    }
    function objfu(obj) {
        let obja = {}
        for(let i in obj){
            if(typeof obj[i] ==='object'){
                obja[i] = objfu(obj[i])
            }else{
                obja[i] = obj[i]
            }
        }
        return obja
    }
    let hhh = objfu(obj)
    obj.data.id = 123
    console.log(hhh)
    console.log(obj)


这个手写递归，依然不能复制不可枚举属性和symbol类型，而且只能对一些普通对象有效，像函数之类就没有效果了


---





原型链继承的缺点是原型链是内存共享的，一个发生改变，其他继承同一个的，都会发生改变

构造函数继承的缺点是只能继承父类的方法，属性，实例，不能继承原型的实例和方法


new关键词的作用：执行一个构造函数，并且返回一个实例对象，根据构造函数来确定是否可以接受参数的传递

new执行后必然返回一个对象，这个对象可以是实例对象，也可以是构造函数return返回的指定对象

手写new


    function _new(funs,...args){
        if(typeof funs !=='function'){
            throw "no function"
        }
        let obj = new Object()
        obj.__proto__ = Object.create(funs.prototype)
        let res = funs.apply(obj,...args)
        let isObj = typeof res === 'object' && typeof res !== null
        let isFuns = typeof res === 'function'
        return isObj || isFuns ? res:obj
    }                


闭包：利用作用域链特性，访问局部变量


IIFE（立即执行函数）本身创建了闭包，保留了全局作用域和当前作用域

什么是闭包？例如：

    function abc(){
        let a = 666
        function xyz(){
            console.log(a)
        }
        return xyz
    }
    let haha = abc()
    haha()

可以看到函数abc的返回值是xyz函数，变量haha被赋值abc函数，并且调用了xyz函数，神奇的获取到变量a的值

关键点：xyz可以访问abc的作用域。在执行完毕abc函数，作用域并没有被销毁，而在垃圾回收中，只有被使用才不会被回收，在这里abc函数的作用域被xyz函数所使用，而且haha被调用时，执行的就是xyz函数，得到abc函数的整个定义时的词法作用域


简单的说就是，利用作用域链（这个特指函数作用域，函数作用域具备遮蔽性），内部作用域可以获取外部作用域的变量，只有当内部作用域拥有访问整个外部作用域时，内部作用域在某个时刻被调用了，自然可以访问到外部的定义时的词法作用域，利用这个特性，内部作用域只要能在定义时词法作用域之外的地方调用，就可以实现闭包，从而访问外部定义时的词法作用域


除了通过返回值外，还可以在定义时调用外部函数，外部函数再调用具备外部词法作用域访问能力的内部函数，也可以访问到这个定义时词法作用域，例如：

    function abc(){
        let a = 666
        function xyz(){
            console.log(a)
        }
        haha(xyz)
    }
    function haha(funs){
        funs()
    }
    abc()

闭包会导致内存溢出



数组


Array数组构造器

let arr = Array(3)


当Array构造器的参数长度为0或者大于等于2时，传入的参数按顺序变成新数组的元素（参数长度为0，返回空数组）


当Array构造器存在1个数值参数时，这个参数最大不能超过2的32次方


Array.of()方法可以将参数按照顺序变成数组的元素，然后返回这个新数组，这个参数可以是数值（而且不会被用来创建指定长度的数组）


Array.from()方法有3个参数，类似于数组的对象，加工函数，this作用域（加工函数中this的指向）


    let arr = {
        0:'abc',
        1: 'xyz',
        2: 'hallo word',
        length: 3
    }
    let abc = Array.from(arr,function(value,index){
        console.log(value,index,this)
        return value.repeat(3)
    },arr)
    console.log(abc)




Array.isArray方法可以判断是否是数组



类数组的对象（和数组类似，但是没有数组的方法）：函数的arguments，HTMLCollection，NodeList


类数组转真数组：

    let abc = {
        0: 'hhh',
        1: 'bbb',
        2: 'abc',
        length: 3
    }
    console.log(abc)
    let a = Array.from(abc)
    console.log(a)
    let b = Array.prototype.concat.apply([], abc)
    console.log(b)
    let c = Array.prototype.slice.call(abc)
    console.log(c)




数组扁平化（将一个多维数组转换为只有一层的数组）



通过递归实现，只要元素还是个数组，就进行递归操作

通过reduce函数迭代

通过扩展运算符实现

通过toString().split(',')实现

通过flat()方法实现

例如：

    let arr = [2,[3,5,6,10,[1,2,3],56,88],44,66]
    function arrData(arr){
        return arr.flat(Infinity)
    }
    console.log(arrData(arr)) // [2, 3, 5, 6, 10, 1, 2, 3, 56, 88, 44, 66]

通过JSON.stringify()和正则实现（先将数组转换为json字符串，然后通过正则(/(\[|\])/g）过滤掉[]，得到的字符串再通过JSON.parst()转换回



v8引擎垃圾回收

基础类型使用栈内存（按值访问），可以使用操作系统进行处理，而引用类型的堆内存需要经常变化（使用引用内存地址访问），大小不固定，需要使用垃圾回收机制来处理

Sting和Number和Boolean和null和undefined和Symbol类型的变量将存储于栈内存，其他类型都存储于堆内存中



v8引擎的堆内存分为2类，新生代内存和老生代内存

在64位操作系统中，新生代内存空间为32MB

新生代中的变量经过一次Scavenge算法回收后，就可以被放入老生代内存中

Scavenge算法将新生代内存分为from-space和to-space两个区域

新生代内存中变量先存储在from-space区，存活的变量将转移到to-space区域，然后将那些在from-space区中不活跃的变量清除释放，执行完毕，再to-space和from-space互换


老生代内存垃圾回收机制，Mark-Sweep（标记清除）和Mark-Compact（标记整理）


Mark-Sweep（标记清除）：先遍历堆上所有对象，并且打上标记，在代码执行完成后，对使用过的变量进行标记取消，而且存在标记的变量就是没有使用过的变量，将这些变量进行垃圾回收

Mark-Compact（标记整理）：（解决标记清除机制的内存碎片化的影响）将内存空间往一端靠拢，直接清除边界外的内存

内存泄露：已经分配堆内存地址的对象长时间没有释放或者无法释放，造成内存长期占用，导致应用响应变慢或者崩溃的情况


内存泄露优化：减少全局变量，使用完定时器后及时清除定时器，避免死循环

将一个强引用类型赋值为null，并不会直接导致其被GC，而是解除其引用，方便下次GC能回收它

解决因闭包导致的内存泄露，在不使用一个闭包函数时，应该手动将其引用变量设置为null，解除引用



V8引擎执行JavaScript程序的阶段（因为v8使用了java虚拟机，c++编译器的技术，使JavaScript执行存在编译器）

Parse阶段：将JavaScript代码转换为AST（抽象语法树）

Ignition阶段：解释器将AST转换为字节码，然后解析执行字节码，并且提供编译所需要的信息

TurboFan阶段：编译器使用Ignition阶段的编译信息，将字节码转换为可执行的机器码

Orinoco阶段：垃圾回收阶段，将不使用的内存空间进行垃圾回收


生成AST有两个阶段，词法分析和语法分析

词法分析阶段会将JavaScript代码解析拆成不能再分的词法单元（token），直接忽略空格

语法分析阶段将词法单元转换为由元素逐级嵌套而成的语法结构树，这个树叫抽象语法树




---





JavaScript借用方法（重用其他对象的方法和函数，无需从该对象继承）

例如一个类数组（类数组并没有数组的方法），借用数组的方法，通过call()，apply()和bind()方法实现

    let arrObj = {
        0:'abc',
        1: 'xyz',
        2: 'hallo word',
        length: 3
    }
    let arr1 = Array.prototype.join.bind(arrObj,',')
    let arr2 = Array.prototype.pop.call(arrObj)
    let arr3 = Array.prototype.push.apply(arrObj,['hhh'])
    console.log(arr1())
    console.log(arr2)
    console.log(arr3)
    console.log(arrObj)

从Array.prototype可以看到这些方法是在类型的原型上定义的，当然其他自定义方法也是可以通过对象.方法的方式借用



通过字面量（Literals）实现

在JavaScript中，使用字面量可以代表值。它们是固定值，不是变量，就是在脚本中按字面给出的。

    let arrObj = {
        0:'abc',
        1: 'xyz',
        2: 'hallo word',
        length: 3
    }
    let arr1 = [].join.bind(arrObj,',')
    console.log(arr1())


[]表示Array.prototype，还有''表示stringObject


---



createDocumentFragment && requestAnimationFrame渲染大量

document.createDocumentFragment()会返回一个DocumentFragment对象，这个对象是DOM节点，但又不是主DOM树的一部分，其是用于创建文档片段，文档片段在内存中，将元素插入文档片段不会导致回流，而且文档片段是直接附加在主DOM树上

window.requestAnimationFrame()，该方法会在浏览器的重绘之前执行一个回调函数

DocumentFragment对象是一个没有父对象的文档对象，DocumentFragment和document区别就是DocumentFragment不是真实DOM树的一部分，但是它可以挂载在真实DOM树上



实现渲染10000条div数据


   <div id="app"></div>

    let count = 0
    function callback(){
        const nums = 10000 // 一共插入10000条
        const num = 100 // 一次插入100条
        let loop = Math.ceil(nums / num) // 计算需要执行多少次
        const app = document.querySelector("#app")
        const data = 'hallo word'
        const vmDOM = document.createDocumentFragment()
        for (let i = 0; i < num; i++) {
            const div = document.createElement("div")
            div.innerHTML = data
            vmDOM.appendChild(div)
        }
        app.appendChild(vmDOM)
        count++
        if (count < loop) {
            window.requestAnimationFrame(callback)
        }
    }
    callback()

document.createDocumentFragment()和直接操作DOM，测试结果说明并不是每操作一次dom都会导致渲染，而且是将短时间的dom操作合并在一起，然后统一渲染，操作10000次DOM，并不会触发10000次渲染，而是在一定的时间内的多次操作合并成一次操作，并且在一定的时间内执行该操作，最后渲染，这得益于现代浏览器的优化


---


Promise/A+规范

Promise有很多规范，ES6使用的就是Promise A+规范

Promise A+规范：

Promise是具备then方法的对象或者函数

Promise需要存在3个状态：等待态（pending），成功态（fulfilled），失败态（rejected）

并且Promise初始状态为等待（pending），在等待态（pending）状态时可以转为成功态（fulfilled）或者失败态（rejected）其中之一

当处于成功态（fulfilled）时，不能改变其状态，还需具备一个不可改变的返回值（value）

当处于失败态（rejected）时，也不能改变其状态，还需具备一个不可改变的返回原因（reason）

需要提供then()方法来返回成功的结果或者失败的原因（传入2个可选参数，onFulfilled和onRejected，如果是函数的话，会在Promise执行完毕后调用，如果不是函数的话会忽略，onFulfilled和onRejected都只能被调用一次）

当处于成功态（fulfilled）之后，执行onFulfilled，返回value，当处于失败态（rejected）之后，执行onRejected，返回reason

then()可在同一个Promise被多次调用，then()必须返回一个promise对象


手写Promise




---



JavaScript引擎（例如v8）可以让JavaScript符合了一些编译语言的特点（词法分析/分词，语法分析/解析，编译转换）（虽然不是提前编译执行）

词法分析/分词(Tokenizing/Lexing)：将程序源代码分成一个个的词法单元（token），而词法分析就是将源代码转换为词法单元，具体通过符号表来确定是变量还是函数什么的

语法分析/解析：将词法单元流转换为抽象语法树（AST, Abstract Syntax Tree）,这个抽象树表示了整个程序的语法结构

注意：并不是词法分析器获取完全部的词法单元后在再进行语法分析的，而是先获取一个词法单元后直接进行语法分析

编译转换：将AST语法树转换为可执行的机器码（引擎可根据语法树一边编译一边执行）

一边编译一边执行有个缺点，当某段代码频繁执行时（例如函数）就会导致重复编译（提前编译的可以在执行前就是处理好），因此添加了一种叫JIT（Just-in-time，即时编译）


JIT在JavaScript引擎中添加了一个监视器，可观察运行的代码，并且记录每段代码的运行次数和变量的类型

当某段代码执行了多次，监视器就会给这段代码标识为Warm，如果是执行很多次的话，会标识为Hot


当某段代码被标识为Warm后，JIT将其发送给基线编译器（Baseline compiler）编译，并且将编译结果存储起来，并且以行号，变量类型作为索引，当执行了符合索引的代码时，直接执行之前编译的代码，不需要重复编译

当某段代码被标识为Hot后，JIT将其发送给优化编译器（Optimizing compiler）编译优化，是通过假设（根据概率模型）变量类型（当存在多个假设时（如果只有一个假设时，直接执行对应的编译结果），会在执行前进行类型检查，来证明假设，如果假设不成立，会导致反优化（将使用编译器或者基数编译器处理（假设多次失败实质比直接编译还慢，因此浏览器做出了限制，当出现多次优化失败时，会放弃处理这个）））



---

作用域

作用域是一套可以根据名称来查找变量的规则（每个作用域负责管理自己地方的变量），当多重嵌套作用域时，会形成作用域链，在当前作用域中查找不到，会向上一级查找，一直查找到全局作用域，如果还是没有找到就是报错

LHS查询和RHS查询


LHS查询是获取变量的实质内存（赋值操作，例如let a = 123），RHS查询是获取变量的值（例如console.log(a)，这个就是RHS查询）

注意：函数传递参数也是LHS查询，只不过变量变成函数的参数而已，而且当作用域链找不到LHS查询的目标时，作用域会创建一个同名的变量到全局作用域中（隐式创建变量，前提是要在非严格模式中）

在作用域链中，当前作用域的RHS查询或者LHS查询任务完不成，是可以到上一级作用域中完成的


词法作用域

作用域有两种，词法作用域和动态作用域，在JavaScript中的作用域就是词法作用域，虽然JavaScript没有动态作用域，但是可以用this来实现相似的效果

词法作用域就是在词法分析/分词阶段的作用域，一般情况下词法作用域在定义后是不会改变的（有个例外是，欺骗词法作用域，例如利用eval或者with欺骗）

函数的词法作用域是由函数被定义时所处的位置决定，不会被函数如何调用，在哪调用而影响

由于作用域查找会在找到第一个目标时会停止的特性，在多重嵌套作用域环境下，其他和目标同名的会被遮蔽找不到，除了全局对象外（全局作用域下的变量会自动变成全局对象的属性（例如window.abc）,可以通过查找全局对象属性来访问到被遮蔽的变量），其他被遮蔽的无论如何都访问不了


欺骗词法作用域

eval()允许接收一个字符串作为其参数，并且执行，例如

    function abc(str,b){
        eval(str)
        console.log(a,b)
    }
    var a = 123
    abc("var a = 666",100) // 666 100

这里利用eval()传递并且执行了"var a = 666"到abc()函数中，改变了该函数的词法作用域，同时遮蔽了全局作用域中的a变量，这个前提是在非严格模式中，因为在严格模式中eval()在运行时会拥有自己的作用域，导致无法修改其所在的作用域

with()可以扩展语句的作用域链（注意：在ES5严格模式中，with被完全禁用了）

    let abc ={
        a: 123,
        b: 666
    }
    with(abc){
        a = 666
        b = 123
    }

with()可以将对象处理成词法作用域，在with内部中对象的属性也会变成这个词法作用域的词法标识符，从而改变原来的词法作用域，例如：


    let abc ={
        a: 123,
        b: 666
    }
    function xyz(obj){
        with(obj){
            a = 666
            b = 123
        }
    }
    xyz(abc)
    console.log(abc.a,abc.b)


函数作用域（函数会为其创建一个它私有的作用域）

在函数中定义的变量将成为其词法作用域中的标识符，无法在外部作用域中获取，在多重嵌套作用域中，外部作用域定义的变量，可以在内部作用域访问，但是外部作用域无法访问内部作用域

函数作用域可以用于隐藏自身不影响外部，因此也可以避免污染外部环境（命名冲突）

IIFE（立即执行函数表达式）

由于正常的函数名也会污染外部环境，因此可使用IIFE解决，当不需要函数名时，这玩意很有用（就算有函数名也不会污染外部）

有两种编写方式，分别是(function abc(){})()和(function abc(){}())


    (function abc( funs ) {
        let xyz = {
            a: 123
        }
        funs(xyz)
    })(function funs(xyz) {
        let a = 666
        console.log(a)
        console.log(xyz.a)
    })




块作用域

在let出现之前，可以使用try/catch创建块作用域，其声明的变量只能在catch语句块中使用，例如：

    try{
        throw undefined // 手动制造异常
    }catch(a){
        a = 123
        console.log(a)
    }
    console.log(a) // a is not defined


let关键字可以将变量隐式绑定到其所在的块作用域中（{...}内部），只有其作用域内部可以访问，在外部访问会报错，使用let定义的变量是属于一个全新的作用域，而不是当前的函数作用域或者全局作用域，除let关键字外，还有const关键字也可以，只不过const定义的是常量，在定义后不能修改（修改导致报错）




---

高阶函数（惰性函数/柯理化函数/compose组合函数）


---

class类的装饰器（Decorator）


---

Set/Map/WeakSet/WeakMap



---


Generator生成器和Interator迭代器





