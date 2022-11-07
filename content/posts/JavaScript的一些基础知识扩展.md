---
title: "JavaScript的一些基础知识扩展"
categories: [ "技术" ]
tags: [ "JavaScript" ]
draft: false
slug: "32"
date: "2021-06-16 15:12:00"
---

JavaScript的变量具有动态性的特性，就决定了变量只是用于保存该特定的值的名字

JavaScript中有两种，基本类型值和引用类型值

基本数据类型有数字(Number)，字符串(String)，布尔值(Boolean)，null，undefined，（es6中加入了个symbol）,BigInt(ES2020)

基本类型是按值访问，可以直接操作保存在变量中的值

基本类型值是简单的数据段，而引用类型是指由多个值组成的对象

因为基本类型是按值访问的，所以可以操控保存在变量的值

引用类型的值是保存在内存中的对象，JavaScript不允许直接访问内存中的位置

操控对象，其实就是在操控对象的引用，而不是对象本身



JavaScript的逻辑运算

JavaScript的任何数据类型都能转换为布尔类型

< 小于
\> 大于
\<= 小于或者等于
\>= 大于或者等于
== 没错，这个才是等于，=这个是用来赋值的
!= 不等于
=== 绝对等于（值和数据类型相等）
!== 不绝对等于（值和数据类型其中有一个或者都不相等）


&&  必须都要符合要求

||  只有一个符合要求

!   真为假，假为真（当判断为true时返回false，判断为false时返回true）

逻辑运算的结果为布尔值




类型判断

数据类型有undefined，null，boolean，number，string，object，symbol，BigInt（ES2020）


undefined，该变量没有被赋值

null，该变量的值为空的对象指针

boolean，该变量的值为布尔值

string，该变量的值为字符串

number，该变量的值为数值

object，该变量为对象或者null

Symbol，生成一个全局独一无二的值



tyoeof操作符会返回该变量的值或者该值是什么数据类型


注意：null值表示一个空对象指针，所以使用typeof操作符检测null时会返回object，typeof个函数，会返回function

typeof判断null之类的不合适，在这种情况可以使用instanceof

instanceof会检查构造函数的prototype属性是否在某个实例的原型链上

例如：

var abc = new testa()
if(Object.getPrototypeOf(abc) === testa.prototype){}


instanceof判断null

null instanceof Object // false


返回布尔值,例如


var arr = [1,1,2,3,4,5];
console.log(arr instanceof Array); // 返回true

constructor
返回初始化该变量的值的数据类型的构造函数，例如
var home = "chenjunlin";
console.log(home.constructor); // 返回String() { [native code] }



toString.call
例如：
toString.call(null); // 返回[object Null]



---


原型与原型链

原型：每一个对象（null除外）在被创建的时候会关联另外一个对象，而这个对象就是原型，每一个对象都会在原型那里继承属性

原型链：每个构造函数都有prototype属性，通过new创建的实例可以访问到原型的属性和方法，实现的原理就是原型链，通过__proto__属性指向prototype属性，当该对象没有某个属性就会通过__proto__去查找构造函数的prototype属性，如果还是没有，那么就是会查找构造函数原型的__proto__，这个查找的过程就是原型链

每一个函数都有一个prototype属性，这个属性指向函数的原型对象

每一个对象都有一个__proto__属性，这个属性指向该对象的原型

每一个原型都有一个constructor属性，这个属性指向构造函数

构造函数通过new建立一个实例，这个实例可以通过__proto__找到它的原型,再通过constructor指向到构造函数

通过new创建一个实例，会新建一个空对象，然后将该对象的__proto__属性指向构造函数的prototype，再通过apply指向这个新对象。执行构造函数并且返回执行结果

因为实例的__proto__属性指向于实例的原型，而这个原型可以通过constructor属性指向到构造函数

例如：

    function home(a,b){
        this.a = a;
        this.b = b;
    }
    var gpp = new home('hi','chenjunlin');
    console.log(gpp.__proto__ == home.construction);

    console.log(gpp.__proto__.constructor == home);

    console.log(home.prototype == gpp.__proto__);

结果全部为true



class

    class test{
        constructor(hi){
            this.hi = hi;
        }
        print_go(){
            console.log(this.hi);
        }
    }
    new test('hello').print_go();


继承

    class test_a extends test {
        constructor(hi, abc){
            super(hi);
            this.abc = abc;
        }
        abc_1(){
            console.log(`hi,${this.hi}`);
        }
    }
    let go = new test_a('no','chenjunlin');

    console.log(go.hi, go.abc);
    go.abc_1();

    go.print_go();




instanceof


instanceof 可以判断一个引用是否属于某构造函数

例如：

    function obj(hemo,go){
        this.hemo = hemo;
        this.go = go;
    }

    var no = new obj('go',10)

    console.log(no instanceof obj);

结果为true，说明no实例的构造函数为obj



---





作用域与闭包

作用域

当某一个执行环境的程序执行完毕后，该环境会被销毁，保存在其中的变量和函数也跟着销毁

全局执行环境当应用程序退出时才会被销毁（例如关闭网页）

全局执行环境被认为是window对象。因为所欲全局变量和函数都是作为window对象的属性和方法创建的

每一个函数都有一个自己的环境，当执行到某个函数时，该函数的环境就进入一个环境栈中，当函数执行完毕后，则环境栈会弹出该环境

当程序在一个环境中执行时，会建立一个作用域链，是为了保证有权访问该环境的所有有序变量和函数

在一个环境内部定义的变量或者函数不能影响到外部的变量或者函数，只作用于该环境内部

例如：
    var hi = "hello";
    function no(){
        var hi = "hi";
        console.log(hi);

        function go(){
            var abc = "go";
                hi = "abc";
            console.log(abc);
            console.log(hi)
        }
        go();
    
    }
    no();
    cosole.log(abc);
    console.log(hi);


有三个环境，全局环境，abc的局部环境，go的局部环境





自由变量


当在某一个作用域上要使用某个变量，但是没有在该作用域中声明，需要到其他作用域中找到这个变量，而这个变量为自由变量，例如:

    var a = 100;

    funotion c(){
        var b = 1;
        console.log(a + b); // a为自由变量
    }
    c();



取a的值要到另一个作用域中获取，要到创建这个被执行函数的作用域中（函数c是在全局作用域中被创建的）取值，所以要到创建函数c的作用域中获取到a的值，如果在创建的作用域中也没有找到该变量，那么会一直向上找，一直找到全局作用域，还是没有找到就抛出错误，而这个沿着作用域一直向上级寻找的机制叫作用域链


延长作用域链

只有两种情况可以延长：with语句和try-catch语句的catch块，例如：

    function abc(){
        var a = "/index.html";
        with(location){
            var url = href + a;
        }
            return url;
    }
    var o = abc()
    console.log(o);

父对象的所有变量，对子对象都是可见的，但是子对象的所有变量，父对象是不可见的






this

每一个作用域中都绑定了一个特别的关键字：this

this的值不取决于它所在的位置，取决于它是怎么被调用的（父级）

在全局作用域下，this表示为window对象，例如:

    var name = "this";
    console.log(this.name);
    console.log(window.name);

改变this的值：

call(),apply(),bind()

例如：
    var a = function(){
        console.log(this);
    }
    a.call('hi');


闭包：当外层函数嵌套了内层函数时，这个内层函数调用了外层函数作用域的变量，并且这个内层函数全局可访问（这个被调用的变量不会因为外层函数执行结束而被垃圾回收机制清除）






---


错误代码捕获


    try{
        var a = null;
        var b = a.length;
    }catch (error){
      console.log('出错了：' + error.message);
    }finally {
        console.log('yes');
    }


把可以出现错误的代码放在try语句中，如果没有出错，那么catch语句不会执行（相反，如果出错则执行该语句），最终执行finally语句(无论是否出现错误，该语句都会执行)

JavaScript中有一个对象来表示错误（Error），其他错误都是继承于该类型

例如：

Error，TypeError，ReferenceError，SyntaxError，URIError，RangeError，EvalError


没有通过try-catch处理的错误都会触发window对象中的error事件，该事件有五个参数，分别是错误信息，所在文件（或者url），行，列，错误对象


window.onerror = function(message, source, lineno, colno, error){}



---



监测设备是否网络连接中，html5中提供了一个属性navigator.onLine，返回的值为布尔值，当网络离线，返回false，正常网络连接则为true

例如：


    const app = navigator.onLine;
    if(app){
        window.addEventListener("offline",function(){
            console.log("当前网络已经离线")
        })
    }else{
        window.addEventListener("online",function(){
            console.log("当前恢复网络正常")
        })
    }


这里通过navigator.onLine的初始值，来确定当前是否连接，然后监听网络变化

online事件：网络状态从离线到在线触发

offline事件：网络状态从在线到离线触发


---





数据类型

JavaScript数据类型有7种（es6），分别是数值（number），字符串（string），布尔值（boolean），undefined，null，对象（object），Symbol


Symbol不会在这里写，以后会独自写一篇es6的笔记



返回一个值是哪个数据类型的，typeof 值

数值有整数和浮点数

因为JavaScript中，数值都是用64位浮点数表示，所以JavaScript这个语言的底层中其实是没有整数这个东西的

浮点数是不精确的，主要是因为位运算，位运算虽然处理速度快，但是精度差

JavaScript的浮点数的取值范围为2的负1023次方到2的1024次方，当一个数超过这个范围则溢出（大于范围返回Infinity）（小于范围则返回0）

Number.MAX_VALUE和Number.MIN_VALUE分别是能表达的最大值和最小值

Infinity表示无穷，一般是数值太大或者数组太小导致的，当一个非0的数被0除，也会返回，Infinity也有正负之分

NaN表示非数值，一般是把字符串当成数值处理导致的，NaN是一个特殊的数值，是Number数据类型下的，NaN不等于任何值，包括它本身


字符串（string）和布尔值（boolean）就不写了，理解这些很轻松的


null和undefined


这俩个都表示“无”或者“空"，在if判断中也是定位false，==比较也表示相等

null表示一个空的对象，undefined用来表示为没有定义（例如一个变量声明了，但是没有赋值）


对象（object）

对象就是一个键值对（key-value），一个键名对应一个键值，例如：

    var obj={
        user : "root",
        pass : "123"
    }
user是键名，root为键值，多个键值对用逗号分隔

键值可以是任意数据类型，例如

    var obj={
        yes: function(a){
            return a+a*2;
        }
    };
    obj.yes(10)


对象opj的键名yes指向了一个函数

对象的键名也叫属性，如果属性的值是一个函数，那么这个属性可以叫为方法


并不需要在声明对象的时候指定属性，可以动态创建，JavaScript允许在任何时候都可以定义属性


对象是引用类型，对象实质上就是一个对象的属性指向一个地址，这个地址可以是任意数据类型，也就是说，只要有变量指向了某一个对象，那么这个变量可以读取和修改该对象的属性和值

注意：获取对象的属性，可以使用点运算符和方括号运算符（但是数值键名不能使用点运算符，会认为是小数，可以获取属性，也可以修改属性值），键名会自动修改为字符串（因此键名可以不加引号也可以不加引号）

查看对象的所有属性Object.keys(obj)

删除对象的某个属性delete obj.yes（但是不能删除继承过来的属性，只能删除对象本身带有的）


检查某个对象的某个属性是否存在   'yes' in obj


允许通过for循环来遍历某个对象的全部属性，例如：

    var obj = {
        user : root,
        pass : root
    }

    for (var i in obj){
        console.log(i);
        console.log(obj[i]);
    }


注意：该方法不能遍历不可遍历的属性，而且还会遍历继承过来的属性，因此一般会用hasOwnProperty来判断一下是否是对象本身的属性，例如：

    for (var i in obj){
        if (obj.hasOwnProperty(i)){
            console.log(i);
            console.log(obj[i]);
        }
    }

如果想操作某个对象的多个属性时，可以使用with循环来操作，例如：

    with(obj){
        user=admin;
        pass=123;
    }

注意：请确保该对象的属性是已经存在的，不推荐使用with来操作属性，毕竟with的内部作用域是当前的，不是全局


函数

函数的定义用function关键字来定义，例如：

    function hallo(x){
        return x+x*2;
    }

将函数指向到一个变量中，那么这个函数可以叫函数表达式，使用函数表达式声明函数时，函数名只能在函数体内部有效，如果一个函数没有函数名，那么这个函数就是匿名函数


构造函数

构造函数的函数名习惯上首字母大写，调用构造函数需要new关键字，例如：

    function Yes(uesr,pass){
        this.user = user;
        thin.pass = pass;
    }

    var hallo = new Yes('root','123');
    console.log(hallo.user,hallo.pass);

当使用new关键字调用构造函数时，会创建一个空对象，用来返回的实例对象，这个空对象的原型指向了构造函数Yes的prototype属性，根据原型和原型链的关系，得出hallo.prototype=Yes.prototype

对象实例hallo的属性继承于构造函数Yes的属性，因此构造函数实质上就是给对象提供属性模板的，JavaScript内部有大量已经定义好的构造函数（有一部分来自浏览器提供），可以使用这些构造函数直接来实例化对象

如果在调用的时候没有使用new关键字，那么这个函数无法生成实例对象，为了避免这个情况，一般会使用严格模式，在函数内部第一行加上'use strict';


注意：如果一个函数被多次声明，那么后面的声明会覆盖前面的声明

函数的length属性，该属性会返回函数预期需要传入的参数的个数（固定为定义时的参数个数，不会随着传入的参数的个数而作出改变）

函数的toString()方法，该方法会返回一个字符串，内容为该函数的源码（连换行符和注释都可以返回）


函数作用域

作用域有三种，分别是全局作用域和函数作用域，块级作用域（es6）

在函数作用域中，在函数内部定义的变量，只能在该变量内部使用，因此有部分人会习惯会将全部程序放在函数中，直接调用函数，用来避免污染已经定义好的变量


注意：如果一个函数的参数有同名的，会取最后一个值

JavaScript允许函数不定数目的提供参数，而这需要用到arguments对象，例如：

    var hallo = function(){
        var a = arguments[0];
    }
    hallo("hallo word");

注意：在严格模式下，使用arguments对象来操作参数会无效


闭包

一般情况下，在函数内部定义的变量无法被外部访问到，但是可以基于链式作用域结构，让子对象一级一级的向上寻找父对象的变量，父对象的变量，子对象都可以访问，但是子对象的变量，父对象无法访问，例如：

    function abc(){
        var a = "hallo";
        function xyz(){
            console.log(a);
        }
    }

而闭包作用就是可以让子对象可以获取到父对象的同时，可以将变量始终保存在内存中，例如：

    function yes(a){
        return function(){
            return a++;
        }
    }
    var gg = yes(1)

每一次调用都是在上一次调用完的基础上计算，可以让该变量一直存在，如果不被垃圾回收机制处理，那么该变量会一直保存当前的值，提供调用


闭包具有封装对象的私有属性和私有方法的功能，例如：

    function Ayes(user){
        var name;
        function setName(a){
            name = a;
        }
        function getName(){
            return name;
        }
        return {
            name :name,
            setName: setName,
            getName: getName
        };
    }

    var i = Ayes("root");
    i.setName("chenjunlin");
    i.getName();


闭包的本质就是在一个函数内部建立另一个函数

闭包的特性：

函数嵌套函数

在函数内部可以引用函数外部的变量或者参数

变量和参数不会被垃圾回收机制清理

例如：

    function a(){
        var b = 'abc';
        return function(){
            return b;
        }
    }
    var c = a();
    console.log(c());

a()返回的值是一个匿名函数，这个函数在a()的作用域内部，所以可以获取a()作用域中的变量b的值，然后将这个值作为返回值赋值给全局作用域中的变量c



    var a = 10;
    var b = function(c){
        if(c>a){
            console.log(c);
        }
    }
    void function(abc){
        var a = 100;
        abc(20);

    }(b);

这是通过作用域链的特性，让局部作用域外部也可以获取到局部作用域内部的变量

因为在局部作用域中可以获取到全局作用域的变量，而局部作用域通过向上返回值来达到传递值的目的



---

当某一个执行环境的程序执行完毕后，该环境会被销毁，保存在其中的变量和函数也跟着销毁

全局执行环境当应用程序退出时才会被销毁（例如关闭网页）

全局执行环境被认为是window对象。因为所欲全局变量和函数都是作为window对象的属性和方法创建的

每一个函数都有一个自己的环境，当执行到某个函数时，该函数的环境就进入一个环境栈中，当函数执行完毕后，则环境栈会弹出该环境

当程序在一个环境中执行时，会建立一个作用域链，是为了保证有权访问该环境的所有有序变量和函数

在一个环境内部定义的变量或者函数不能影响到外部的变量或者函数，只作用于该环境内部

例如：
    var hi = "hello";
    function no(){
        var hi = "hi";
        console.log(hi);

        function go(){
            var abc = "go";
                hi = "abc";
            console.log(abc);
            console.log(hi)
        }
        go();
    
    }
    no();
    cosole.log(abc);
    console.log(hi);


有三个环境，全局环境，abc的局部环境，go的局部环境



---

自由变量


当在某一个作用域上要使用某个变量，但是没有在该作用域中声明，需要到其他作用域中找到这个变量，而这个变量为自由变量，例如:

    var a = 100;

    funotion c(){
        var b = 1;
        console.log(a + b); // a为自由变量
    }
    c();



取a的值要到另一个作用域中获取，要到创建这个被执行函数的作用域中（函数c是在全局作用域中被创建的）取值，所以要到创建函数c的作用域中获取到a的值，如果在创建的作用域中也没有找到该变量，那么会一直向上找，一直找到全局作用域，还是没有找到就抛出错误，而这个沿着作用域一直向上级寻找的机制叫作用域链


延长作用域链

只有两种情况可以延长：with语句和try-catch语句的catch块，例如：

    function abc(){
        var a = "/index.html";
        with(location){
            var url = href + a;
        }
            return url;
    }
    var o = abc()
    console.log(o);

父对象的所有变量，对子对象都是可见的，但是子对象的所有变量，父对象是不可见的




---


另一篇笔记


如果在body元素中添加script元素，那么在解析和执行JavaScript程序完成之前内容不会被出现在页面中，使用defer属性避免出现页面的内容的延迟呈现


defer属性只适用于外部脚本文件，将其脚本延迟到浏览器遇到</html>后执行，会优先于DOM事件，按照先后顺序执行，把JavaScript脚本放在页面底部依然是最优的选择


async属性，同样只适用于外部脚本文件，该属性会告诉浏览器立即下载该文件，不按照先后顺序执行原则，所以使用async属性时确保该文件不依赖其它文件


async属性的目的是不让浏览器等待JavaScript下载和执行，让其在能异步处理其它内容


当浏览器不支持脚本或者支持脚本，但是脚本被禁用时，使用noscript元素，当脚本无效时才会显示该元素的内容，如果脚本有效时，是不会显示在该元素的内容


JavaScript区分大小写，标识符的第一个字符必须是一个字母或者小划线（_）又或者美元符合（$），其他字符可以是字母，下划线，美元符号或者数字


驼峰大小写命名：第一个字母小写，剩下的每个单词的首字母大写，不用把关键字，保留字，true，false和null作为标识符


注释：

//  单行注释

/*
*    这是
*    块级（多行）注释
*/

除了/**/外，其他*不是必须要的，只是为了注释的可读性


严格模式



启用严格模式在顶部添加"use strict";


在函数内部的上方使用该指示，可以指定函数在严格模式下执行



语句结尾的分号;不是必需的，如果省略分号，那么由解析器来确定语句的结尾，推荐在语句结尾加上分号;



使用{}将多行语句组合成一个代码块，例如if语句，但是当语句只有一行时是可以不加{}，不过推荐加上{}



ECMA-262中有一组特殊用途的关键字和一些未来可能用到的保留字，这些都不能用于作标识符




定义变量要使用var关键字+变量名（标识符），如果只定义变量，没有给该变量赋值，那么该变量会有一个特殊的值——undefined




变量可以保存任意数据类型，变量不会因为赋值，就标记其为这个数据类型，可以多次赋值，每一次赋值就是覆盖该变量之前的值

在函数中定义一个变量，那么在函数执行完毕退出时，该变量会被销毁，因为在函数中定义的变量会被认为该变量是局部变量，自作用于该变量的作用域中


如果省略var关键字，那么该变量会成为一个全局变量，可以在函数外部的任何地方访问到，但是不推荐，如果在严格模式下将抛出错误



可以使用var关键字定义多个变量，每个变量有,逗号隔开



数据类型有undefined，null，boolean，number，string，object


undefined，该变量没有被赋值

null，该变量的值为空的对象（还没有创建的对象）

boolean，该变量的值为布尔值

string，该变量的值为字符串

number，该变量的值为数值

object，该变量为对象或者null

tyoeof操作符会返回该变量的值或者该值是什么数据类型





undefined类型

使用var声明变量时没有给其初始化，那么该变量的值为undefined，未经过初始化的值默认得到undefined



null类型

null值表示一个空对象指针，所以使用typeof操作符检测null时会返回object



boolean类型（布尔值）

该类型只有两个值，true和false，注意是区分大小写的

boolean类型和其他数据类型有等值的关系，使用Boolean()函数转换为boolean数据类型


空字符串（''）会返回false，非空字符串返回true，非0数字（包括无穷大）为true

0和NaN未false，任何对象（不包括null空对象指针）为true，null为false，undefined为false




number类型

表示整数和浮点数，整数可以表示十进制和八进制，十六进制。八进制必须是0开头（后面为0-7，超过了值当成十进制处理），十六进制必须是0x开头（后面为0-9和A-F）


八进制，十六进制表示的数值最终都会被转换成十进制数值

浮点数值必须包含一个小数点，而且小数点后面也必须有一位数字，如果浮点数本身表示的就是一个整数，该值会转换为整数

对于那些极大或者极小的数组，可以使用科学计数法表示，科学计数法的e表示e前面的数值乘以10的指数

最小的数值（Number.MIN_VALUE）,最大的数值（Number.MAX_VALUE）

最小的值为5e-324，最大的值为1.7976931348623157e+308

小于最小的值为-infinite（负无穷），大于最大的值为infinite（正无穷），infinite不能参与计算


检查一个数是否在最小的值和最大的值之间，可以使用ifFinite()，当一个数在在最小的值和最大的值之间，那么返回true，否则返回false



NaN类型

NaN表示非数值，任何涉及NaN的操作，都是NaN，NaN不和任何值相等，包括NaN本身

检查一个值是否"不是数值"，可以使用isNaN()，当不是数值时返回true，否则返回false

但是isNaN会尝试转换为数值，如果可以被转换为数值的依然会返回false，所以类似"6"，true(可以转换为1)之类的也会返回false



有三个函数可以把非数值转换数值，Number()，parseInt()和parseFloat()


Number()可以用于任何数据类型，而其他两个则是把字符串转换为数值


Number()的转换

true为1，false为0

数值，没有变化

null为0

undefined为NaN

字符串，当字符串只包含数字（浮点数，十六进制）时，转换为十进制的数值

字符串为空（不包含任何字符）时为0

字符串中包含除上面的外的字符，转换为NaN


关于一段代码块后面是否要加分号，取决于个人习惯，如果习惯写Python，那么就可以不加，如果习惯写Java，那么加上就好，个人开发者怎么舒服怎么来，团队开发看团队习惯，就算没有加分号，浏览器解析时候也会正常解析的，并没有多大影响，不过推荐加分号


---


document.querySelector("")；

选择器只能选中第一个元素

document.querySelectorAll("");
	   
可以选中所有符合选择器规则的元素
	  


.classList.add("类名") : 添加指定类样式

.classList.remove("类名") : 移除指定类样式

.classList.contains("类名"); 判断是否包含指定类样式

.classList.toggle("类名");  切换指定类样式




---
自定义属性（dataset）

标签中自定义属性名必须为data-开头

.dataset： 获取全部自定义属性的属性名和属性值（属性名不包括data-），输出返回的结果为DOMStringMap

该方法也可以直接定义新的属性和属性值，例如：

document.querySelector(".app").dataset.name="root";

或者document.querySelector(".app").dataset["pass"]="root";





---

文件读取(FileReader)

FileReader中提供3个方法来将读取文件返回的结果进行处理在result中

分别是：
readAsText：文本
readAsBinaryString：字符串		 
readAsDataURL：dataurl
readAsArrayBuffer：ArrayBuffer对象
abort：终止文件读取操作

FileReader也提供了几种事件，分别是：

onload：文件读取完成时触发
onloadend：文件读取完成时触发，不管是否读取成功，只要调用了FileReader()，那么最后都会触发该事件
onloadstart：读取开始时触发
onprogress：读取中时触发，该事件在读取过程每50ms就会触发一次，可以利用该事件设置进度条来表示读取的进度
onerror：读取出错时触发，该事件中有一个属性code，就是错误码，1表示没有找到该文件，2表示安全性错误，3表示读取中断，4表示文件不可读，5表示编码错误
onabort：读取中断时触发

例如：

    <input type="file" name="">
    <script>
    	var mainfile = document.querySelector("input");
    
    	mainfile.onchange = function(){
        
    		var files = this.files;
    
    		var file = files[0];
    
    		console.log(file.name); // 获取文件名
    
    		var  maindata = new FileReader(); // 创建读取器
    
    		maindata.readAsDataURL(file);
    		maindata.onload = function(){
    			console.log(maindata.result);
    			// 转换为data_url
    		}
    	}
    </script>



---



JavaScript引用类型

JavaScript的变量具有动态性的特性，就决定了变量只是用于保存该特定的值的名字

JavaScript中有两种，基本类型值和引用类型值

基本数据类型有数字，字符串，布尔值，null，undefined，（es6中加入了个symbol）

基本类型是按值访问，可以直接操作保存在变量中的值

基本类型值是简单的数据段，而引用类型是指由多个值组成的对象

因为基本类型是按值访问的，所以可以操控保存在变量的值

引用类型的值是保存在内存中的对象，JavaScript不允许直接访问内存中的位置

操控对象，其实就是在操控对象的引用，而不是对象本身



---







---

this

每一个作用域中都绑定了一个特别的关键字：this


在全局作用域下，this表示为window对象，例如:

    var name = "this";
    console.log(this.name);
    console.log(window.name);

改变this的值：

call(),apply(),bind()

例如：
    var a = function(){
        console.log(this);
    }
    a.call('hi');



---







面向过程：根据步骤来一步步来实现解决问题

面向对象（OOP）: 设置对象的功能来解决问题，封装性，继承性，多态性

类和对象

对象共用的属性和行为封装成一个类，对类进行实例化，获取类的对象

对象是一组无序的相关属性和方法的集合，对象由属性（特征）和方法（行为）组成


类

关键字class声明的一个类，通过类实例化一个具体的对象


创建类和生成实例

创建类

class name{}

实例化对象

var name = new name();


constructor构造函数

constructor()方法是类的构造函数（默认），用来传递参数，返回实例对象，通过new关键字生成的对象实例时，自动调用该方法

例如：

    class names{
        constructor(name,age){
            this.name = name;
            this.pass = pass;
        }
    }
    var abc = new names("root",123);
    console.log(abc);
    console.log(abc.name);

共有方法：

在类里面的函数不需要写function，同时多个函数或者方法不需要加逗号分隔例如：

    class names{
        constructor(name,age){
            this.name = name;
            this.age = age;
        }
        xyz(yes){
            console.log("欢迎" + this.name+ "," + yes);
        }
    }
    var abc = new names("root",18);
    abc.xyz(666);


类的继承

子类可以继承父类的一些方法和属性，通过extends关键字，将父类属性和方法继承到子类中，例如：

    class names{
        constructor(name,age){
            this.name = name;
            this.age = age;
        }
        xyz(yes){
            console.log("欢迎" + this.name+ "," + yes);
        }
    }
    class gg extends names {
    
    }
    var abc = new gg("root",18);
    abc.xyz(666);


super关键字，可以用于访问和调用父类是的函数，调用父类的构造函数或者普通函数


    class names{
        constructor(name,age){
            this.name = name;
            this.age = age;
        }
        xyz(yes){
            console.log("欢迎" + this.name+ "," + yes);
        }
    }
    class gg extends names {
        constructor(name,age){
            super(name,age);
        }
    }
    var abc = new gg("root",18);
    abc.xyz(666);





    class names{
        constructor(name,age){
            this.name = name;
            this.age = age;
        }
        xyz(yes){
            console.log("欢迎" + this.name+ "," + yes);
        }
    }
    class gg extends names {
        xyz(yes){
            console.log(super.xyz(yes));
        }
    }
    var abc = new gg("root",18);
    abc.xyz(666);

注意：

继承中的属性或者方法查找是就近原则

继承中，一个实例化子类输出一个方法，如果子类有这个方法，就先执行这个子类的方法

继承中，如果子类没有，就去查找父类有没有这个方法，如果有就执行父类的方法

super必须在子类构造函数this之前调用

在es6中类没有变量提升，需要先定义类，再通过类来实例化对象

类共有的属性和方法要加this使用


this指向问题

在constructor里面的this指向的是创建的实例对象

在方法里面的this指向的是实例对象，因为实例对象调用了该方法

this的指向取决于谁调用了，怎么调用的







 

---

JavaScript模块化

传统开发模式的问题

命名冲突

文件依赖


模块化：指的是单独的一个功能封装到一个模块中，模块之间相互隔离，但是可以通过特定的接口公开内部成员，也建议依赖别的模块


浏览器端模块规范

AMD，CMD

服务器端模块化

commonjs




ES6模块化（推荐使用，浏览器端与服务器端通用规范）

在ES6语法规范中定义了ES6模块化规范

ES6模块化规范中定义：

每个js文件都是一个独立的模块

导入模块成员使用import关键字

暴露模块成员使用export关键字



模块化的基本语法

默认导出：

export default 需要导出的成员名

例如：

let a = 666

export default a


默认导入：

import 接收名称 from "模块标识符"

例如：

import abc from "./hallo.js"


---


前后端交互模式


接口调用方式

原生ajax，jQuery的ajax，fetch，axios


传统的url

schema：协议

host：域名或者ip

port：端口

path：路径

query：参数

fragment：锚点（哈希hash）


Restful的url

get查询

post添加

put修改

DELETE删除






































































