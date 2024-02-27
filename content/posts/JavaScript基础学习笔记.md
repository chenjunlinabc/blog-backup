---
title: "JavaScript基础学习笔记"
categories: [ "默认" ]
tags: [ "JavaScript" ]
draft: false
slug: "46"
date: "2021-06-24 07:57:00"
---

插入js

在html页面使用js
    <script type="text/javascript"></script>

引用js文件
    <script src=""><script>


注释

//  单行注释

/*
多行注释

*/ 


变量

var home // 声明一个变量使用var

home = "hallo"  // 使用=来把字符串hallo赋值到home变量

变量命名要求：
必须以字母、下划线或美元符号开头，后面可以跟字母、下划线、美元符号和数字

建议每一行语句结束使用;来表示结束当前语句，主要是给人看


var num = 123 // 整数

num = "123" // 字符串

num = 1.23 // 浮点数（小数）

num = true // 布尔值（真，假或者对，错的意思，true是真，false为假）

num = 1+1 // 可以进行运算，也可以连接两个字符串

num = "hallo" + "javascript"

var nums = 1

nums++  // 和nums = nums+1意思是一样，所以这个时候nums = 1+1 =2

nums-- // 和nums = nums-1意思是一样，所以这个时候nums = 1-1 = 0

< 小于
\> 大于
\<= 小于或者等于
\>= 大于或者等于
\== 没错，这个才是等于，=这个是用来赋值的 
\!= 不等于


比较的结果一定是布尔值，要么true。要么false

&&  和，与的意思，必须是两个条件都符合，才会返回true，否则返回false

||   或者的意思，只要符合其中的一个条件就返回true，否则返回false

num = 1

a = !(num>2) 

//这个返回true，因为num不大于2，所以是false，但是!()可以把真看成假，把假看成真


操作符之间的优先级（高到低）:

算术操作符 → 比较操作符 → 逻辑操作符 → "="赋值符号

同级的运算是按从左到右次序进行,多层括号由里向外。


num = 1

num = numa + 1 > 2 && numa * 2 < 2

// (numa +1)>2   (numa *2) <2  所以num 的值为false


var array = new Array()  // 定义数值

array[0] = "hi"
array[1] = 'hallo'

数值从0开始，数值是值的整合，每一个数值的值都有一个索引号

array = new Array(5) //定义数值，创建8个值

创建的新数组是空数组，没有值，输出会显示undefined

array = new Array(1,2,3,4,5,6) // 定义数组，同时赋值


array = new Array[1,2,3,4,5,6] // 定义数组，同时直接输入个数组


array[99] = 66 //使用一个新的索引，为数值添加一个新的元素

document.write(array[1])  //输出array数组，索引号为1的值


array.length = 6 // 获取数组array的长度，并且修改数值的长度

alert(array.length) // 输出6


var Array = [[0,1],[2,3]] // 定义一个二维数组

array[0][1] = 5 // 把第0行的第1列的元素赋值为5




    if(条件)｛
        程序块
    ｝



条件必须为true（真）时才会执行程序块，为假（false）时忽略跳过或者执行else的内容，又或者执行else if的内容


会被认为是false（假）的有：false，0，“”，NaN，null，undefined

return //终止


    var code = 'hallo'

    if (code == 'hallo'){
    document.write('hallo')
    }else if(code == 'no'){
        document.write('no')
    }else{
        document.write('go')
    }




    switch(变量){
        case值: 
             	// 当等于或者符合某个值时，执行case下的程序块
        break

        default:
	    // 当都不等于或者不符合上面给出的某个值时，执行default下的程序块，就比如if的else一样
        break
    }



    code = 3
    switch(code){
        case 1:
        document.write('1')
        break
        case 2:
        document.write('2')
        break
        case 3:
        document.write('3')
        break

        default:
        break
    }

    for(初始值;条件;更改初始值){
        // 必须条件符合才执行程序块，当条件不符合时，跳出for循环
    }


    for(code=1;code<=3;code++){
        document.write(code)
    }



    while(条件){
        // 当条件为真时，执行该程序块，如果条件一直为真时，会一直执行下去，所以一定要保证条件结果可能为假
    }


    while (code <= 10){
        document.write('code')
        code++
    }



    code = 1
    do{
        document.write(code)

        code++
    }

    while (code <= 5)





    for (code = 1;code<=10;code++){

        if(code == 2){
            continue
        }

        if(code == 3){
            break
        }


        document.write(code)
    }



    function 函数名(传参，不强制必须要传参){
        // 当执行该函数时，执行程序块
    }

    函数名(传参，不强制必须要传参)//触发函数


在函数内部定义的变量，只作用于该函数内部，并不影响函数外部定义的变量

在函数内部定义的变量就叫局部变量，在函数外部定义的变量叫全局变量

在函数内部可以获取函数外部的变量，在函数外部，不能获取到函数外部的变量

注意的是：只作用于该函数内部，不影响其他函数内部


    ;(functiom(){
        //....


        // 为了避免污染全局变量，可以使用这种方法
    })()






    function code(a,b){
        num = a+b
        alert(num)
        return num
    }

    code(1,2)





事件

onclick是鼠标点击事件，当点击鼠标时，执行被调用程序块



onmouseover是鼠标经过事件，当鼠标移到一个对象上，就触发onmouseover事件，并执行被调用的程序块


onmouseout是鼠标移开事件，当鼠标移开当前对象时，触发该事件，并执行被调用程序块




onfocus是光标聚焦事件，例如光标移到文本框内时，就是焦点在文本框内上，触发该事件，并执行被调用程序块




onblur是失焦事件，和光标聚焦事件相反


onselect是内容选中事件，当文本框或者文本域中的文字被选中时触发该事件，并执行被调用程序块


onchange是文本框内容改变事件，当通过修改例如文本框的内容时触发该事件，并执行被调用程序块




onload是加载事件，当页面加载完成后触发该事件，并执行被调用程序块


onunload是卸载事件，当退出页面或者页面刷新等等操作时触发该事件，并执行被调用程序块






对象


var name = new Array() // 定义个数组对象

name.length // 使用array对象的length属性来获取数组的长度，这是获取到对象的属性的例子



Date 对象

var go_date = new Date(); // 假如是2012年1月2日

document.write(go_date); // 输出go_date的值，不同浏览器，时间格式有差异



getFullYear(); // 返回年份时间

setFullYear(); // 设置年份时间


document.write(go_date.getFullYear()); //输出go_date的年份，输出2012


go_date.setFullYear(2022); // 设置go_date的年份，go_date的年份已经设置为2022

document.write(go_date.getFullYear());  // 这个时候输出go_date的年份，输出结果为2022


getDay(); // 返回星期，0到6的整数，0表示星期天

document.write(go_date.getDay())  // 输出数字，可以通过数组来返回相对于的星期

getTime(); // 返回时间，单位毫秒，返回 1970 年 1 月 1 日至今的毫秒数
setTime(); // 设置时间



length返回字符串长度
var str_01 = "hello JavaScript";
var str_02 = str_01.length;

toUpperCase()把字符串小写字母转换为大写字母
var str_03 = str_01.toUpperCase();

charAt()返回指定位置的字符串
document.write(str_01.charAt(0));



indexOf()返回指定字符串中首次出现的位置

document.write(str_01.indexOf('h');






windows对象


alert(); // 弹窗，只能确定


confirm(); // 弹窗，可以确定，也可以取消，返回是布尔值


prompt(); // 弹窗，可以输入内容（传入内容）




setTimeout(函数,时间);  // 定时器，时间是毫秒，1000毫秒=1秒，当时间到了就触发函数，只会触发一次


setInterval(函数,时间); // 每一到指定的时间就触发一次函数，可以触发n次


clearInterval(清除定时器); 




DOM

document.getElementById(); // id选择器
document.getElementsByTagName(); // 标签选择器
document.getElementsByClassName(); // 类名选择器

document.createElement(); // 创建dom
document.body.appendChild();

addEventListener(); //添加点击事件

innerText(); // 添加文本




---








ES6模块化

export语法

export default默认导出

export abc需要使用import { abc } form './test.js'的方式导入

babel编译功能

npm install --save-dev babel-core babel-preset-es2015 babel-preset-latest

npm install --global babel-cli

.babelrc文件

    {
        'presets': ['es2015','latest'],
        'plugins': []
    }


webpack

npm install webpack babel-loader --save-dev

webpack.config.js配置文件


rollup打包工具

npm install rollup rollup-plugin-node-resolve rollup-plugin-babel babel-plugin-external-helpers babel-preset-latest babel-core --save-dev

rollup.config.js配置文件

    import babel form 'rollup-plugin-babel'
    import resolve form 'rollup-plugin-node-resolve'
    export default{
        entry: 'src/index.js',
        format: 'umd',
        plugins: [
            resolve(),
            babel({
                exclud: 'node_modules/**'
            })
        ],
        dest: 'build/bundle.js'
    }


rollup -c rollup.config.js


AMD模块化标准，nodejs模块化标准（CommonJS），ES6模块化标准，兼容模块化UMD


class实质上是构造函数的语法糖，构建器constructor就是构造函数，不过class的方法是在类内部创建的，而构造函数是通过扩展原型的方式（prototype），让实例具备该方法

构造函数等于构造函数的原型的构造器（prototype.constructor）

实例.__proto__等于构造函数的原型

构造函数继承（同时也是class继承的原理）

Abc.prototype = new Xyz()

这样Abc构造函数就可以继承Xyz构造函数的属性和方法

promise

---


原型

异步

虚拟DOM


MVVM

组件化

hybrid