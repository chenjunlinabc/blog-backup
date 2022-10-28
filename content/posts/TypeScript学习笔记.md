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

TypeScript本身并不能在浏览器运行，需要进行编译成JavaScript


全局安装typescript

yarn global add typescript

或者

npm install -g typescript

安装ts-node

npm install -g ts-node

ts-node运行（不需要编译成js文件，直接运行ts）

ts-node hallo.ts


检查是否安装成功

tsc -v


使用TypeScript编写的文件统一以.ts为后缀，用ts写React，统一以.tsx为后缀

创建tsconfig.json

npx tsc --init

编译阶段去除注释：找到removeComments，设置为true

执行（不加任何参数，这样才能使用tsconfig.json配置文件，tsc默认对当前根目录的ts文件进行编译）

tsc

编译指定ts文件（在compilerOptions同级）

"include":["./test1.ts"] // 指定编译哪个ts文件
"excclude":["./test.ts"] // 指定不编译哪个ts文件


其他配置

rootDir（指定输入目录）
outDir（指定输出目录）
allowJs（是否处理js文件）
checkjs（是否检查js文件的语法）

编译TypeScript文件

tsc hallo.ts




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

二进制和八进制，十进制，十六进制都可以用number类型来表示


string字符串

let hallo: string = hallo;


void类型（当一个函数没有返回值时，那么其返回值类型就可以定义为void）

    function voiddemo(): void {
        let a  = 10;
    }

void类型只有两个值，undefined和null


undefined类型和null类型是所有类型的子类型



Any（当不清楚类型的变量指定一个类型，有可能这个变量的类型是动态的，想跳过类型检查就可以用这个）


let abc: any =123;

abc = hallo;



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


类型断言

as语法

let hallo any =hallo;

let abc number = (hallo as string).length;

尖括号语法

let hallo any =hallo;

let abc number = (stringhallo).length;



面向对象

以键值对的方式表示的都是对象，多个键值对用逗号,分隔

const obj = {hallo: '123world'}

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
    function abc(as: hallo) {
        console.log(as.a)
    }
    let myabc: hallo = {
        a: 123,
        b: 666,
        c: "hallo",
        xyz(){
            return 'hallo word';
        }
    }
    abc(myabc)


在属性名前使用readonly指定其为只读属性

interface和type的区别：interface表示一个对象的属性类型，而type被作为类型别名使用，例如：

    type testa = sting

testa就是sting类型的别名，而interface不能

在属性名后使用?指定其为可选的


接口用于类上

     class Test implements hallo{
        a: 1,
        b: 2,
        c: "abchhh",
        xyz(){
            return 'hallo ts';
        }
    }

不要把接口来当type用，这是错误的用法，是用来定义一个对象中有哪些属性，有哪些方法，方法里面有哪些属性，参数等等，接口本身就是为了规范一个对象的行为，使用implements关键字就表示该对象就是这个接口的实现


接口的继承


    interface hallo{
        a?: number;
        b: number;
        readonly c: string;
        [Name: string]: any;
    }
    interface Test extends hallo{
        abcxyz(): string; // 这里继承了hallo接口，并且还给自己定义了个返回值为string类型的abcxyz()方法
    }
    function abc(as: Test) {
        console.log(as.a)
    }
    let myabc: Test = {
        a: 123,
        b: 666,
        c: "hallo",
        xyz(){
            return 'hallo word';
        }
        abcxyz(){
            return 1+2;
        }
    }
    abc(myabc)


函数的接口定义

    interface abc1{
        (test: string): string;
    }
    let abc: abc1 = (a: string) => {
        return a; 
    }
    let a = abc('hallo word')
    console(a)


使用[]括起来表示这是任意数量的属性，只要不是已经存在（被定义）的属性

    let abc: {
        name: string;
        pass: string;
    } = {
        name: 'root',
        pass: '123456'
    };

    let xyz : number[] = [1,2,3,4,5,6];

    class Tabcxyz{}
    let a1: Tabcxyz = new Tabcxyz();

    let a2: () => string = () => {
        return 'hallo word'; // 函数返回值类型
    }

判断数据类型

let abc: number = 123;

typeof abc === "number"; // tuer

ts具备自动分析变量类型功能，当无法分析时，才需要使用类型注解

let domeTest = 123 // number，自动分析变量类型功

let abc: number; // 类型注解

number = 123


函数

    function abc(): number{
        return 123; // 这个number表示函数返回值的类型
    }

函数参数类型


    function abc(a:number,b:number): number{
        return a+b; // 这个number表示函数返回值的类型
    }
    let xyz = abc(1+2)

函数解构参数类型

    function abc({a,b}: {a: number,b: number}): number{
        return a+b; // 这个number表示函数返回值的类型
    }
    let xyz = abc(1+2)



---


数组与元组（tuple）


当数组元素存在不同类型时

    let arr:(number | string | boolean)[] = [1,'hallo', true, 123, 'hhh']; // 表示数组元素可以是number和string和boolean

数组对象类型

    let objarr:{name: string, age: number, pass: string}[] = [{
        name: 'root',
        age: 18,
        pass: '123456'
    }]
    
或者

    type objclass = {name: string, age: number, pass: string}
    let abc : objclass[] = [{
        name: 'root',
        age: 18,
        pass: '123456'
    }]


或者

    class Abctest{
        name: string, 
        age: number, 
        pass: string 
    }
    let abc : Abctest[] = [
        new Abctest(),
        {
            name: 'root',
            age: 18,
            pass: '123456'
        }
    ]


元组（元组的长度和类型是固定的，数组每一项都必须对于类型的每一项）

    let abc: [sting, number, sting] = [
        'root',
        18,
        '123456'
    ]

多维元组

    let abc: [sting, number, sting][] = [
        ['root',18,'123456'],
        ['admin',16,'123456789'],
        ['test',20,'test123'],
    ]




---


类的定义与继承


    class Demo{
        name = 'root';
        Test(){
            return this.name;
        }
    }
    class Demo1 extends Demo{
        name = 'admin'; // 重写 name属性
        Test1(){
            return 'hallo ts'; // 继承Demo类的同时，自己还具备Demo不存在的Test1()方法
        }
        Test(){
            return 'hallo' + super.Test() // 如果在重写父类的方法时，还想调用父类的方法时，可使用super调用
        }
    }
    let demo1 = new Demo1();
    console.log(demo1.Test());
    console.log(demo1.Test1());

---

类的访问类型（private，protected，public）和构造器（construction）

    class Test(){
        public name: string;  // 类外可被调用
        protected pass: string; // 类内以及被继承到的子类调用
        private Testa(){
            console.log('hallo word') // 类内可被调用，类外不可调用
        }
    }
    let test1 = new Test();
    tets1.name = 'hallo';
    console.log(tets1.name);
    class Test1 extends Test{
        public Testaa(){
            cosole.log(this.pass);
        }
    }


construction构造器（construction方法是类内部提供的，当一个类被new了，那么该类construction方法就会触发）

    class Test(){
        public name: string;
        protected pass: string;
        construction(name: string){
            this.name = name;
        }
    }
    let test1 = new Test('admin');
    cosole.log(test1.name);

注意：当父类存在construction时，子类也有construction时，需要手动调用父类的construction（调用super()）


---

静态类型，setter和getter（暴露私有属性并且保护私有属性）

    class Test(){
        private _name: string; // name属性是私有属性，无法被类外调用
        construction(name: string){
            this._name = name;
        }
        get getName(){
            return this._name; // 通过getName方法对外暴露name属性
        }
        set setName(name: string){
            let Tname = name.split('')[0]; // // 通过setName方法对外暴露name属性
            this._name = name
        }
    }
    let test1 = new Test('admin');
    cosole.log(test1.getName);

单例模式

    class Test(){
        private static Testabc: Test;
        private construction(){} // 禁止通过new创建实例
        public getTest(){
            if (!this.Testabc) {
                this.Testabc = new Test(); // 判断如果Testabc不存在时，Testabc等于Test实例
                return this.Testabc; // 返回存储了Test实例的属性
            }
            return this.Testabc; // 对外暴露这个存储了Test实例的属性
        }
    }
    let test1 = Test.getTest() // 存储了Test实例的属性赋值给了test1变量
    let test2 = Test.getTest() // 存储了Test实例的属性赋值给了test2变量
    console.log(test1 === test2); // true 这两个存储了Test实例的变量实质上还是同一个

---

抽象类（抽象类只能被继承，类的抽象进行封装，复用）

    abstract class Abc{
        name: string;
        abstract getTest1(): string;
    }

    class Test extends Abc(){
        constructor(){
            super();
        }
        getTest1(){
            return 'hallo word';
        }
    }
    class Test1 extends Abc(){
        constructor(){
            super();
        }
        getTest1(){
            return 'hallo hhh';
        }
    }
    let test1 = new Test();
    console.log(test1.getTest1);

---


联合类型和类型保护

联合类型就是给一些类型出来，只要满足其中一种类型就可以，例如：

    let a : number | string = 'hallo word'
    a = 66 // a变量需要满足number或者string其中一种类型就可以了

类型保护就是通过类型断言，class或者instanceof来保护类型


enum枚举类型

枚举通过enum关键字创建，并且枚举元素一被定义将无法改变，格式和对象类似

    enum a{
        name,
        age,
        pass
    }
    console.log(a.age) // 1
    console.log(a[1]) // age

枚举元素默认第一个值从0开始递增，另外key和value是能互相访问的（这些规则只能用于数值枚举元素，其他类型无效）

    enum a{
        name = 'root',
        age = 18,
        pass = '123456'
    }

上面这个例子为异构枚举，允许数值枚举和字符串枚举混合使用

注意：当然枚举存在字符串枚举成员时，是不能存在计算的


函数泛型（泛型：generic，表示泛指的类型）

    function abc<XYZ>(a: XYZ,b: XYZ){
        return a + b;    
    }
    abc<string>('hallo','root');


类泛型和泛型类型

    class Demo<XYZ>{
        ages: XYZ[]
        getDemo(a:number): XYZ{
            return this.ages[a]
        }
    }
    let demo = new Demo<number>([1])
    demo.getDemo(0)


命名空间namespace（在命名空间内部定义的变量，类只能在该命名空间内部金额访问到，如果需要外部访问需要export导出）

    namespace Test{
        let a = "hallo word";
        function b(i: number)
            return i;
    }

命名空间的实现原理就是立即执行函数搭配闭包，闭包可以减少全局变量的诞生，而如果进行导出export了，那么该被导出的变量会暴露到全局环境下

合并命名空间

如果是同名的命名空间，会自动合并成一个（注意：合并只针对导出的成员，而且不能重名）

如果是不同文件下命名空间，例如：

a.ts

    namespace Test{
        let a = "hallo word";
        function b(i: number)
            return i;
    }

b.ts
    /// <reference path="a.ts"> // 会在编译b.ts之前编译a.ts
    namespace Test{
        import a = Test.a
        console.log(a)
    }






---




类型注解文件（.d.ts）

类型注解声明需使用declare或export关键字，declare是定义全局，可直接引用，而export需要使用import导出

注意：当定义interface接口或者type类型的时候，不需要加declare或export关键字

定义全局变量abc，它的类型是string（除了var关键字可以定义外，还有let，const都可以定义）

    declare var abc: string;

定义全局函数abc()，该函数的参数为string类型，返回值也是对象，并且定义该对象中存在一个data方法，该方法接收一个string类型的参数，该方法的返回值是string类型

    declare function abc(param:string): {
        data: (data: string) => string;
    };
     
     abc('test').data('hallo word');


定义接口 Abc，该接口被用于函数abc()上，表示abc()函数的返回值类型

    interface Abc{
        test: (str: string) =>{};
    }
    declare function abc(): Abc;

函数重载：类型注解是允许多次定义函数的类型注解声明

声明对象Abc，该对象下存在着Test对象，Test对象下存在Data()构造函数

     declare namespace Abc {
          namespace Test {
               class Data {}
          }
     }
     
     new Abc.Test.Data()


模块化的类型注解

定义一个Test模块的类型注解，该模块存在一个Abc方法，该方法也存在一个test方法，test方法的返回值为对象

     declare module 'Test' {
          interface Abc{
               test: (str: string) =>{};
          }
          export = Abc;
     }

     import Abc form 'Test'
     Abc.test('hallo word')



---


泛型的keyof关键字



    interface Test{
        name: string;
        pass: string;
        age: number;
    }
    class Abc{
        constructor(private data: Test)
        getData<T extends keyof Test>(key: T): Test[T] {
            renturn this.data[key]
        }
    }
    const TestData = new Abc({
        name: 'root',
        pass: '123456',
        age: 20
    })
    const test = TestData.getData('name');
    console.log(test)


从上面例子中的<T extends keyof Test>，可以看到keyof的作用就是遍历类型的属性，keyof的返回值类型其实是个联合类型（key也是可以用来搞class）

上面例子输入的key参数不为Test接口定义属性时会提示错误





---





装饰器：装饰器本身是函数，装饰器通过@来调用，通过装饰器来扩展目标的功能

注意：装饰器目前是实验功能，需要手动开启实验功能，在tsconfig.json找到experimentalDecorators和emitDecoratorMetadata设置为true


类的装饰器（会在类创建完毕后立即执行装饰器，注意：对类装饰，并不对实例装饰，类装饰器的参数是构造函数）

    function Abc(constructor: any){
        console.log('hallo word');
    }
    @Abc
    class Test{}
    let test = new Test();


方法的装饰器（方法装饰器有3个参数，分别是类的构造方法，方法名，方法的属性描述符）



属性的装饰器（属性装饰器有2个参数，分别是类的原型，属性名）




参数的装饰器（函数的参数装饰器有3个参数，分别是类的构造函数，参数名，参数在参数列表中的索引）







---




协变和逆变

TypeScript允许子类型赋值给父类型，也允许父类型赋值给子类型，因为这是类型安全的，是完全允许的，例如：

    type Test = {
        name: string;
    }
    type Abc = {
        name: string;
        pass: string;
    }
    let a: Test = {
        name: 'root'
    }
    let b: Abc = {
        name: 'root'
        pass: '123456'
    }
    b = a


子类型可以赋值给父类型，而这个就叫协变，允许赋值时给一个更全面的类型或者相同的类型（父类是子类的超类）

TypeScript不需要通过extends继承来确定，而是通过类型结构（structual type）来确定父子关系的


逆变

父类型可以赋值给子类型，函数参数具有逆变的特性，例如：

    type Test = {
        name: string;
    }
    type Abc = {
        name: string;
        pass: string;
    }
    let hallo = (a: Abc) =>{
        console.log(a)
    }
    let hi = (b: Test) =>{
        console.log(b)
    }
    hallo = hi


注意：协变和逆变作用只生效于父子类型，非父子类型不会发生





















