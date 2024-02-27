---
title: "ES6学习笔记"
categories: [ "学习" ]
tags: [ "JavaScript" ]
draft: false
slug: "57"
date: "2021-07-09 22:51:00"
---

ECMAScript

JavaScript的创造者Netscape将其提交给标准化组织ECMA，因此JavaScript的标准是ECMAScript

ECMAScript是规范，JavaScript是实现

ES6又叫ECMAScript2015，因为标准委员会决定每年的6月份正式发布标准，作为当年的正式标准，使用年份来标记，不需要之前的版本号了

变量

let关键字声明变量，使用let声明的变量具有块级作用域有效的特性

例如：

    if(true){
        let a = 666;
    }
    console.log(a); // Uncaught ReferenceError: a is not defined

注意：

使用var声明的变量不具备块级作用域有效的特性

使用let声明的变量不存在变量提升，只能先声明后使用，具有暂时性死区特性，绑定在块级作用域，不会受外部污染或者影响，let不允许在同一个作用域中，重复声明同一个变量

const声明一个只读的常量，一声明，常量的值将不能改变

const abc =123;

const一旦声明将立刻初始化，只声明不赋值是会报错的，同样不存在变量提升，存在暂时性死区

ES6的作用域：

全局作用域
函数作用域
块级作用域

---

因为部分低版本浏览器还不支持ES6，因此有一些工具可以ES6语法转ES5的语法，例如：babel

安装babel

npm install -g babel-cli

安装转换包

npm install --save-dev babel-preset-es2015 babel-cli

新建.babelrc文件，写入：

    {
        "presets":[
            "es2015"
        ],
        "plugins":[]
    }

babel main.js -o src/main.js

然后就成功将ES6语法转为ES5的语法，提高浏览器兼容性

---

变量声明方式

let，声明一个变量

const，声明一个常量

在ES6中，var是声明全局变量的，而let声明的变量是具有块级作用域的，只能在当前语句块中访问调用

let实质上就是为了避免污染全局的设计的，希望在某个语句结束后销毁该变量，而不会影响语句外部的变量

const实质就是为了保证该变量始终不变而设计的，使用const声明的变量，不能进行修改，否则会抛出错误

变量解构

ES6允许从数组或者对象中获取值，来对变量进行赋值，这个又被称为解构，例如：

let [a,b,c] = [1,2,3]

会根据位置的关系来进行变量的赋值，格式一定要相同，否则可能获取undefined

当然，解构赋值也允许使用默认值（在没有进行赋值时，会使用该默认值来代替，当进行赋值时，会该默认值），例如：

let [a,b="hello"] = ["word"]

console.log(b+a) // helloword

对象解构

let {name,pass} = {name:"root",pass:"123"}

console.log(name+pass)

注意：如果变量已经声明了，直接进行解构会抛出错误

解决方式：在解构语句块外部使用圆括号()包裹起来就好了

字符串解构

let [a,b,c] ="abc"

console.log(a+b+c)

扩展运算符

当某个函数或者方法中，传入的参数个数是不确定的时候，就可以使用该来作为参数，例如：

    function abc(...name){
        console.log(name[0])
        console.log(name[1])
    }
    abc("hallo","word")

rest运算符（和扩展运算符相反，合并为一个数组）

    function abc(a,...name){
        console.log(a);
        console.log(name);
    }
    abc("hallo","word","yes")

字符串模板

let name = "root"

let data = \`hallo ${name} 来到\`

console.log(data)

当然也支持插入htm标签，例如：

let data = \`<p>hallo ${name} 来到</p>\`

document.write(data)

也可以在模板中进行运算

let [a,b,c] = [1,2,3]

let abc = \`${a+b+c}\`

console.log(abc)

判断字符串是否存在

let name = "root"

let data = "hallo root"

console.log(data.includes(name))  // true

判断开头是否存在

data.startsWith(name)

判断结尾是否存在

data.endsWith(name)

---

箭头函数

    let abc = (a,b,c) =>{
        console.log(a+b+c)
    }
    abc(1,2,3)

注意：箭头函数不能作为构造函数使用

对象的函数解构

    let obj = {
        a: "hallo",
        b: "word"
    }
    function abc({a,b}){
        console.log(a,b)
    }
    abc(obj) // hallo word

数组的函数结构

    let arr = ["hallo","word"]
    function abc(a,b){
        console.log(a,b)
    }
    abc(...arr)

判断对象或者数组中是否存在某个值

    let obj = {
        a: "hallo"
    }
    console.log("a" in obj)

---

对象

ES6允许将已经声明的变量直接赋值给对象，例如：

let abc = "hallo"
let xyz = "word"
let obj = {abc,xyz}
console.log(obj)

对象键值

let data = "name"
let obj = {[data]: "root"}
console.log(obj)

允许通过变量来传递对象值

let name = "root"
let pass = "123"
let obj= {name,pass}
console.log(obj)

自定义对象方法

    let obj={
        add:function(a,b){
            return a+b
        }
    }
    console.log(obj.add(1,2))

Object.is：严格模式相等

console.log(Object.is(NaN,NaN))
console.log(NaN === NaN)

===只是值相等，但是is是绝对相等，不只是值，连数据类型也是

合并对象Object.assign()

let a={a:'hallo'}
let b={b:'hahaha'}
let c={c:'wowowo'}
let abc=Object.assign(a,b,c)
console.log(abc)

---

json转为数组（json需要提供length属性）

    let json = {
        '0': 'a',
        '1': 'b',
        '2': 'c',
        length:3
    }
    let arr=Array.from(json)
    console.log(arr)

转换数组（Array.of）

    let arr =Array.of(1,2,3)
    console.log(arr)

toString()数组转字符串

    let arr=['hallo','abc','xyz']
    console.log(arr.toString())

arr.find（可以查找一个数组是否存在某个值或者可以查找某个索引值的元素是什么）

    let arr=[1,2,3,4,5,6,7,8,9]
    let data=arr.find(function(value,index,arr){
        return value == 6
    })
    console.log(data)

value是当前要查找的值，index是当前要查找的索引值，arr是当前数组

填充数组

    let arr=[0,1,2,3,4,5,6,7,8,9]
    arr.fill('hallo',3,5)
    console.log(arr)

for...of

这个方法比普通的for循环好用，因为它不需要提供判断区域，会遍历全部的元素

    let arr=['abc','xyz','hallo']
    for (let item of arr){
        console.log(item)
    }

如果想遍历数组的索引值，可以用arr.keys()

迭代化数组（迭代对象中数组的索引值作为 key，数组元素作为 value，例如：[0, 'abc']）

    let arr=['abc','xyz','hallo']
    let list=arr.entries()
    console.log(list.next().value)
    console.log(list.next().value)
    console.log(list.next().value)

主动抛出错误

    function add(a,b){
        if(a == 0){
            throw new Error('This is error')
        }
        return a+b
    }
    console.log(add(0,1))

除了外部程序外可以使用严谨模式，函数体内也可以使用'use strict'，可以用来针对函数体内部

forEach()遍历数组（可以获取索引值和元素值）

    let arr=['abc','xyz','hallo']
    arr.forEach((val,index)=>console.log(index,val))

filter遍历数组

    let arr=['abc','xyz','hallo']
    arr.filter(i=>console.log(i))

判断某个数组里面的值是否符合某个条件（返回布尔值）

    let arr = [1,2,3]
    let data = (a) => a > 1
    console.log(arr.some(data))

因为arr.some()会遍历元素，当找到一个符合条件的就结束遍历，返回值，因此也可以用来遍历数组，例如：

    let arr = [1,2,3]
    arr.some(a=>console.log(a))

但是这个方法有个不好的点就是，会一直遍历，当索引不到值的时候会结束遍历，并且返回false

map()

    let arr = [1,2,3]
    let arrs = arr.map(a => "hallo")
    let data = arr.map(Math.sqrt)
    console.log(arrs)
    console.log(data)

map()可以索引全部元素并且通过覆盖或者科学计算来修改数组的元素深拷贝到另一个变量中，当然也可以进行普通的遍历

join()可以在数组元素后面添加字符串（间隔）

    let arr=['abc','xyz','hallo']
    console.log(arr.join(','))

---

Generator函数，该函数和普通函数不同，其是可以暂停执行，例如：

    function* add(x){
        let y = yield x + 1
        return y
    }
    let a = add(1)
    let abc = a.next()
    console.log("值"+":" + abc.value)
    console.log(abc.done === true ? "没有执行完毕" : "执行完毕" )

Generator函数实质上就是一个被封装的异步容器，需要暂停的地方，可以使用yield语句标注

Generator函数执行后不会返回结果，而是返回指针，调用指针a的next来移动内部的指针

next方法作用就是分段执行Generator函数，每调用一次next，都返回一个对象，该对象有value属性和done属性，value属性是值，done属性是布尔值，表示是否执行完毕

---

Symbol是一个新的数据类型

let sym = Symbol('abc')
console.log(sym)
console.log(sym.toString())
console.log(typeof(sym))

Symbol特点就是一个值和另一个值是不相等的，例如：

let sym1 = Symbol('abc')
let sym2 = Symbol('abc')
console.log(sym1 === sym2)

---

Set数据结构

    let setArr = new Set(['abc','xyz','hallo'])
    setArr.add('abc')
    setArr.add('123')
    setArr.delete("xyz")
    console.log(setArr.has("xyz"))
    console.log(setArr)
    for (let item of setArr){
        console.log(item)
    }
    console.log(setArr.size)
    setArr.clear()

Set的特点就是内部元素不能出现重复值的，可以理解为去重

使用Set实例化的，可以通过add()方法来添加元素

delete()方法为删除某个元素

has查找（返回布尔值）

clear()清空全部元素

setArr.size可以获取元素个数

---
Map数据结构

    let json = {
        name:'root',
        pass:'123'
    }
    let map=new Map()
    map.set(json,'hallo')
    console.log(map)

存值

map.set('abc', "xyz")

获取值（获取json对应的值）

console.log(map.get(json))

删除指定值

map.delete(json)
console.log(map)

获取

console.log(map.size)

has查找

console.log(map.has('abc'))

清空元素

map.clear()

---

Promise

Promise有三个状态，pending: 初始，fulfilled: 成功，rejected: 失败

Promise可以将异步以同步方式来操作，例如：

    let abc = new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('hallo')
            resolve('yes')
            console.log('执行完成')
        }, 1000)
    })
    abc.then((data) => {
            console.log('resolved',data)
        }).catch((err) => {
            console.log('rejected',err)
        }
    )

async/await

在async中，await执行异步操作只能一个个执行，用同步的方式来执行异步操作，例如：

    function req(i){
        return new Promise(resolve =>{
            setTimeout(() => {
                resolve(i * 3)
            }, 1000)
        })
    }
    async function abc(){
        const a = await req(2)
        const b = await req(a)
        console.log(b)
    }
    abc()

需要在Promise异步的情况下使用，否则会同时输出的

async返回的是一个状态为fuifilled的Promise对象，具体要看return返回值

Proxy
