---
title: "TypeScript学习笔记"
categories: [ "学习" ]
tags: [ "TypeScript" ]
draft: false
slug: "58"
date: "2021-07-18 15:50:00"
---

TypeScript设计目标是开发大型项目，编译成纯JavaScript，让其可以运行在任何浏览器上

TypeScript可以增强IDE的交互和反馈，主流IDE都支持TypeScript

TypeScript实质上就是JavaScript的扩展，JavaScript超集

TypeScript本身并不能直接在浏览器运行，需要进行编译成JavaScript

TypeScript实质上就是具备静态类型化的JavaScript，TypeScript之父同时也是C#之父

TypeScript默认对变量做静态类型检测工作，来确保变量的类型按照所希望的那样




全局安装typescript

yarn global add typescript

或者

npm install -g typescript



检查是否安装成功

tsc -v


使用TypeScript编写的文件统一以.ts为后缀，用ts写React，统一以.tsx为后缀

创建tsconfig.json

npx tsc --init


编译TypeScript文件（转为正常js文件）

tsc hallo.ts



在终端中直接运行ts代码



npm install ts-node -g



ts-node hallo.ts



绝大多数的现代IDE都内置支持TypeScript，例如vscode，它就内置了，而且它内置不会影响到手动安装的（隔离）








---




变量声明

推荐使用const和let声明变量，而不要使用var声明（因为var具有变量提升和重定义）



基础类型

TypeScript具有强类型的特点

boolean，string，number，array，null，undefined，object，tuple，enum，void，never，any

boolean布尔值，例如：


let hallo: boolean = true


number数值，例如：

let hallo: number = 123;

二进制和八进制，十进制，十六进制都可以用number类型来表示，除了number表示数值外，还有bigint（用来表示更大的数值）

let hallo: bigint =  100n;

注意：number类型和bigint类型不兼容，number类型和bigint类型不能互相转换




string字符串

let hallo: string = hallo;


void类型（当一个函数没有返回值时，那么其返回值类型就可以定义为void）

function voiddemo(): void {
let a  = 10;
}

void类型只有两个值，undefined和null

undefined类型和null类型是所有类型的子类型



Symbol

let sym: symbol = Symbol('hallo');





Any（当不清楚类型的变量指定一个类型，有可能这个变量的类型是动态的，想跳过类型检查就可以用这个）


let abc: any =123;

abc = hallo;



除了any外，还有unknown也可以表示任何类型



let abc: unknown =123;

abc = hallo;



unknown和any的区别就是，unknown类型的变量只能赋值给unknow和any类型的变量，在处理类型检查（typeof或者instance）或类型断言（as）之前，是不能对 unknown类型的变量进行操作（例如修改对象属性等等），而且any是没有限制的



void（表示没有任何类型，当一个函数没有返回值，那么其返回值类型就是void）

声明一个void类型的变量，这个变量只能选择两个值undefined和null其中一个

let und void = undefined;


undefined和null和JavaScript中概念是一样的，空，未定义




object，就是非基础类型，和JavaScript中概念是一样的

枚举，数组，元组，对象都是object类型


let obj: object;



array数组

let arrs: number[] = [1,2,3];

let arrs: Array<number> = [1,2,3];



tuple元组：元组类型允许一个数组的元素是不同的类型的，但是必须按照要求来定义，例如

let x [string, number?];
x = ['hello', 10];

x.push("hi");

tuple元组是固定长度，固定类型，长度不能多不能少，类型必须要一一对应

元组类型允许在类型后缀一个?来说明该元素是可选的，可选元素必须要在必选元素后面，否则有一个元素后面加了?，那么其后面全部元素都要加?

注意：元组虽然可以越界添加元素（不推荐），但是不可越界访问



enum枚举（允许为一组数值赋予名字）


enum hallo {a, b, c}

hallo.a === 0 // true


默认从0开始，当然也可以手动指定数值

enum hallo {a=6, b, c}


最强大的一处是，可以根据值来得到它的名字

let yes: string = hallo[6];

console.log(yes);

如果分开声明名称相同的枚举，那么就会自动合并



bigint类型（可以表示任意大的整数，Number类型能表示的最大整数为2^53 - 1）

在整数后面加n或者调用BigInt()函数来定义bigint，也可以在BigInt()函数中用字符串，这三种方式表达的出来的值都是一样的


let abc: bigint = 8888888888888888888888888n


let xyz: bigint = BigInt(8888888888888888888888888)


abc === xyz  // true



Symbol类型（每个symbol值都是唯一的）


let abc: symbol = Symbol("hallo")

let xyz: symbol = Symbol("hallo")

abc === xyz  // false



never类型（表示那些永远不存在的值的类型，永不为真）

never类型是任何类型的子类型，也可以赋值给任何类型，但是没有类型是never子类型或者赋值给never类型（除了never本身），连any都不能赋值给never

抛出异常或者不会有返回值的函数，其函数返回值类型为never


unknown类型是any类型对应的安全类型


unknown类型只能赋值给any和unknown类型本身


let abc: unknown

let a: any =  abc // yes

let b: unknown = abc // yes

let c: number = abc // Error

unknown类型被确认为某个类型之前，不能被进行函数执行，实例化之类的操作



不写类型声明也是没有关系的，因为ts会从上下文中推导出类型，例如，上文已经使用该类型，但是下文中使用另一个类型，那么就会导致类型检测不通过了



ts会在编译阶段自动执行静态类型检测，检查变量的值是否和注解的类型相同，如果相同则检测通过，否则报错




类型断言

as语法

let hallo any =hallo;

let abc number = (hallo as string).length;

尖括号语法

let hallo any =hallo;

let abc number = (stringhallo).length;



在ts中，字面量不但可以表示值，还可以表示类型，例如：

 `let nums: 1 = 123`





面向对象

以键值对的方式表示的都是对象，多个键值对用逗号,分隔

const obj = {hallo world}

console.log(obj.hallo)

TypeScript在对象被定义时已经设置了类型，当引用不存在的，会报错



interface接口

限制传入的参数的值

    interface hallo{
        a?: number;
        b: number;
        readonly c: string;
        [Name: string]: any;
    }
    function abc(clothes: hallo) {
        console.log(clothes.a)
    }
    let myabc: hallo = {
        a: 123,
        b: 666,
        c: "hallo",
        xyz: "word"
    }
    abc(myabc)

在属性名前使用readonly指定其为只读属性

在属性名后使用?指定其为可选的

使用[]括起来表示这是任意数量的属性，只要不是已经存在（被定义）的属性


判断数据类型

let abc: number = 123;

typeof abc === "number"; // tuer



---


泛型


---

类型变换模块和命名空间

























