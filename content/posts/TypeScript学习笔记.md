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

TypeScript具备类型检查，静态类型，JSnext属性和方法，以及还在提案阶段的语法（例如装饰器）

TypeScript本身并不能在浏览器运行，需要进行编译成JavaScript（tsc编译）


全局安装typescript

yarn global add typescript

或者

npm install -g typescript

安装ts-node（node不能直接运行ts文件，需要安装ts-node包）

npm install -g ts-node

ts-node运行（不需要编译成js文件，直接运行ts）

ts-node hallo.ts


检查是否安装成功

tsc -v


使用TypeScript编写的文件统一以.ts为后缀，用ts写React，统一以.tsx为后缀

创建tsconfig.json

npx tsc --init

执行（不加任何参数，这样才能使用tsconfig.json配置文件，tsc默认对当前根目录的ts文件进行编译）

tsc

或者指定编译的文件

tsc ./src/index.ts

tsc ./src/index.ts -t es6 -m cjs

注意：cjs标准只能存在一个顶级导出（mudule.exports）,如果存在其他exports的话会被忽略，因此要么不用mudule.exports，要么只使用一个mudule.exports

编译指定ts文件（在compilerOptions同级）

"include":["./test1.ts"] // 指定编译哪个ts文件
"excclude":["./test.ts"] // 指定不编译哪个ts文件




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
        export function b(i: number){
            return i;
        }
    }

命名空间的实现原理就是立即执行函数搭配闭包，闭包可以减少全局变量的诞生，而如果进行导出export了，那么该被导出的变量会暴露到全局环境下

可以给函数，class，enum枚举类型声明命名空间（必须在声明函数，class，enum之后使用命名空间）
命名空间与函数，class，enum枚举类型同名，命名空间将作为补充，例如：

    function Test(){}
    namespace Test{
        let a = "hallo word";
        export function b(i: number){
            return i;
        }
    }
    console.log(Test.a,Test.b(123))



命名空间别名

    import test = Test.b

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


当ts项目需要用到外部库时（例如Jquery），需要提供其类型注解文件，声明其对外的接口，常用的库社区都会提供类型注解文件，一般情况下其类型注解文件都是在该包的源码目录下，如果没有需要进行下载，例如npm i @types/jquery


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




---



ts允许符合一个接口的前提下扩展，但是有个例外，不允许使用字面量的方式（但是可以使用as和<>类型断言来允许）

可索引接口，索引签名

函数接口

    let abc:(a:number,b:number)=> number
    interface abc{
        (a:number,b:number):number
    }

函数接口的实现要求形参和实参必须一一对应（可选参数和剩余参数（扩展）除外）


函数重载（ts允许函数名相同，但是参数个数和类型不相同）


    function abc(...a: number[]): number

    function abc(...a: string[]): string

    function abc(...a: any[]):any{
        let xyz = a[0]
        if(typeof xyz === 'string'){
            return a.join('')
        }
        if(typeof xyz === 'number'){
            return a.reduce((x,y)=>x+y)
        }
    }



多态：通过父类抽象方法，让子类可以重写该方法

类接口和实现的成员修饰符必须相等，不能接口是公有属性，实现变成的私有属性

接口可以继承多个接口，多个接口用逗号分隔，而且该接口的实现必须满足多个接口的要求

接口也可以继承类，一个子类可以继承父类的同时还可以实现接口

接口只能约束类的公有成员，接口可以抽离类的公有，私有，受保护成员




ts类型检查机制：类型推断，类型兼容性，类型保护

类型推断：ts可以根据规则来自动推断出一个类型，基础类型推断，最佳通用类型推断，以及上下文类型推断


类型断言可以覆盖ts的自动类型推断，错误的类型断言很危险


类型兼容：ts允许一个子类类型兼容父类


    type a = {
        aa: number
    }
    type b = {
        aa: number,
        ab: string
    }

    let abc: a = {aa:1}
    let xyz: b = {aa:2,ab:'hallo'}
    abc = xyz
    // xyz = abc 不兼容，因为abc不符合xyz类型的要求，但是xyz符合abc的类型（在满足类型要求上扩展类型）


源类型满足目标类型的要求时，源类型可以赋值给目标类型


函数类型兼容：源函数参数个数必须小于等于目标函数，固定参数可以兼容可选参数和扩展参数，但是可选参数是不兼容固定参数和扩展参数的，扩展参数兼容固定参数和可选参数，而且参数类型必须匹配


    let abc = (a:number,b:number) =>{}
    let xyz = (a:number) =>{}
    abc = xyz

接口的类型兼容：a接口的定义满足b接口的定义时，a的实现可以赋值给b的实现（多兼容少的）

    interface A{abc: number}
    interface B{abc: number,xyz: string}
    let a = (abc: A) => {}
    let b = (abc: B) => {}
    b = a



类型保护：在某个特定的区块中保证变量属性某个确认的类型，在该区块可以放心使用该类型的属性和方法

可以用类型也可以用类
    if(abc instanceof number){}else{}

对象属性
    if('data' in abc){}else{}

基本类型
    if(typeof a === 'string'){}else{}


类型位词

    function isnumber(a: abc | xyz): a is number{
        return (a as abc).data !== undefined
    }

通过返回值来确定类型


高级类型（这些高级类型更像是工具类型）

交叉类型与联合类型


    interface A{abc: number}
    interface B{abc: number,xyz: string}
    let hhh : A & B = {
        abc: 123,
        xyz: 'hallo'
    }

hhh实现具备了A和B接口全部特性，交叉类型是取全部接口的并集


    let a: number | string = 'hallo'

联合类型还可以通过字面量来限制变量的取值

    let  a: 1 | 2 | 3


联合类型对于类只能访问其共有成员（取交集）


    class a{
        data(){}
        abc(){}
    }
    class b{
        data(){}
        xyz(){}
    }
    enum gg{a,b}
    let hhh = gg.a ? new a() : new b{}
    hhh.data()


也可以利用联合类型来根据共用成员的值来做类型保护区块




索引类型（对于索引做类型约束）

    interface B{abc: number,xyz: string}
    let key: keyof B

    let value: B['abc']

    let obj={a:'hallo',b:'abc',c:'xyz'}
    function get<T, K extends keyof T>(obj: T,keys:K[]): T[K][]{
    return keys.map(key => obj[key])    
    }
    get(obj,['a','b','c'])

当索引不存在的属性时，ts会报错提示



映射类型

    interface A{abc: string,xyz: number, hhh: boolean}
    type a = Readonly<A>

可以看到类型别名a是对于属性是只读的

Readonly的实现是索引类型

    type Readonly<T> ={readonly [P in keyof T]: T[P]}

把接口属性变成可选的

    type b = Partial<A>

Partial的实现

    type Partial<T> = {[P in keyof T]?: T[P]}

Required将接口属性变成必选

    type c = Required<A>

Required的实现

     type Required<T> = {[P in keyof T]-?: T[P]}

抽取接口的指定属性，来创建一个新的类型

    type abc = Pick<A,'abc','xyz'>

Pick的实现

    type Pick<T,K extends keyof T> = {[P in K]: T[P]}

Omit去除指定属性，来创建一个新的类型

    type abc = Omit<A,'abc','hhh'>

Omit的实现

    type Omit<T,K extends keyof any> = Pick<T, Exclude<keyof T,K>>

上面的被ts官方叫统态（不会创建新的属性）

Record通过传入的泛型参数来作为接口的属性和值

    interface A{abc: string,xyz: number, hhh: boolean}
    type xyz = 'a' | 'b' | 'c'
    let abc: Record<xyz, A> = {
        a:{
            abc: 'hallo',
            xyz: 123,
            hhh: true
        },
        b:{
            abc: "abc",
            xyz: 666,
            hhh: false
        },
        c:{
            abc: 'hhh',
            xyz: 12345,
            hhh: false
        },
    }

Record的实现

    type Record<K extends keyof any, T> = {[P in K]: T}


条件类型

    type A<T> = T extends number ? 'number': T extends string ? 'sting': T extends Function ? 'function': 'object'
    type Ta = A<string>
    type Tb = A<Function>


分布式条件类型

    type Tc = A<string | number>

下面的例子中diff可以过滤掉T可以赋值给U的属性

    type diff<T,U> = T extends U ? never: T
    type Ta<T> = diff<T,undefined | null>
    type Tb = Ta<string | undefined | null | number>


 
实质上diff类型官方已经提供了，叫Exclude<T,U>
Ta类型ts官方叫NonNullable<T>

Extract<T,U>是Exclude<T,U>的取反，Extract<T,U>会T抽取出可以赋值给U的类型，例如：

    type Tc = Extract<'a'|'b'|'c','a'|'c'>




ReturnType<T>，获取函数返回值的类型


    type Td = ReturnType<()=>string> 


ReturnType<T>的实现

    type ReturnType<T extends (...args: any) =>any> = T extends (...args: any) => infer R ? R : any


意思是T可以赋值给一个函数，这个函数接收不指定数量的参数，参数类型为any，返回值也是any，利用infer延迟推断类型，如果类型为R就返回R，否则返回any


ConstructorParameters获取构造函数的构造参数

    class B{
        constructor(name: string,pass: string, mail?: string){}
    }
    type Bb = ConstructorParameters<typeof B>

ConstructorParameters的实现（通过infer来推断构造参数的类型）

type ConstructorParameters<T extends abstract new (...args: any) => any> = T extends abstract new (...args: infer P) => any ? P : never

Parameters获取函数的参数

    type Tb = Parameters<(a:number,b:string,c?:boolean)=> void>

Parameters的实现

    type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never

ThisParameterType获取函数的this参数类型

    type Tc = ThisParameterType<(this: number,a: string) => void>

ThisParameterType的实现

    type ThisParameterType<T> = T extends (this: infer U, ...args: never) => any ? U : unknown

ThisType指定this的类型



OmitThisParameter去除函数中的this类型

    type Te = OmitThisParameter<(this: number, a: string) => string>

OmitThisParameter的实现

    type OmitThisParameter<T> = unknown extends ThisParameterType<T> ? T : T extends (...args: infer A) => infer R ? (...args: A) => R : T



Uppercase将字符串字面量转换成大写

Lowercase将字符串字面量转换成小写

Capitalize将字符串字面量的第一个字母转换为大写

Uncapitalize将字符串字面量的第一个字母转换为小写

    type Ta1 = Uppercase<'Hello'>
    type Tb1 = Lowercase<Ta1>
    type Tc1 = Capitalize<Tb1>
    type Td1 = Uncapitalize<Tc1>

实现分别为

    type Uppercase<S extends string> = intrinsic
    type Lowercase<S extends string> = intrinsic
    type Capitalize<S extends string> = intrinsic
    type Uncapitalize<S extends string> = intrinsic



---

declare


ts允许声明同名接口，新的同名接口的属性将作为新属性（合并接口），但是不允许重复声明接口的类型，例如：

    interface AA {
        name: string;
      }
    interface AA {
        id: number;
    }
    let aa: AA={
        name: 'root',
        id: 1
    }

注意：接口合并时，非函数类型的属性的类型必须一致，而函数类型的属性将被认为是函数重载，如果使用函数类型的属性时类型不一致，需要提供一个更宽泛的类型（参数和返回值的类型），必须包含其全部类型

合并接口，按照声明的顺序来排，后声明的接口优先级高于前面声明的接口，而接口内部是从上往下排序，但是有个例外，就是当使用字面量作为类型时，使用字面量当类型的属性将会提升到最顶端

也可以利用函数重载（后声明的接口具备更高的优先级，请不要在最后面使用泛型，在后面使用泛型会因为优先级的原因导致后面的类型全是泛型）

    interface BB {
        funs(id: any):any
    }
    interface BB {
        funs(id: string):string
    }
    interface BB {
        funs(id: number):number
    }
    let bb: BB ={
        funs(id:any){
            return id
        }
    }
    let b1 = bb.funs(1)
    let b2 = bb.funs('root')
    let b3 = bb.funs(true)


同样，命名空间也可以像这样合并，但是非export成员只能在原本的命名空间可见

注意：当定义了一个类类型时，这个类类型不能合并，因为它是值又是类型的特殊对象

---

tsconfig.json配置文件应用


rootDir（指定输入目录）
outDir（指定输出目录）
allowJs（是否处理js文件）
checkjs（是否检查js文件的语法）


tsconfig.json的核心compilerOptions，compilerOptions的主要配置分别为target（指定ts编译的目标。例如ES2020，ES5，ESNext等等），module（设置ts使用的模块化系统，当target为ES3，ES5时，module默认为Commonjs，当target为es6或者更高时，默认值为ES6，支持ES2020，UMD，AMD，System，ESNext，以及None选项），jsx（指定jsx文件转译为js文件的输出方式，只影响.tsx文件的输出，支持react，react-jsx，react-jsxdev，preserve，react-native），incremental（表示是否启动增量编译，如果为true时，会将上次编译的工程图信息存储在磁盘上），declaration（表示是否为项目的ts或者js文件生成.d.ts文件，如果需要指定声明文件存储位置，可使用declarationDir属性指定目录，如果只想生成声明文件则可以开启emitDeclarationOnly属性，开启该属性，将不会生成js文件，只生成声明文件，设置声明文件目录，可使用typeRoots属性，该属性默认值为node_modules/@types，这个属性值为数组，可传入1个或者多个声明文件目录，如果只想使用指定声明文件，则可以使用types属性，这个属性值也是一个数组，只有包含在内的声明文件才能被加载引用），sourceMap（是否生成sourcemap文件，sourcemap文件将以.js.map或者.jsx.map的形式被生成与.js文件或者.jsx文件同级目录下，sourcemap文件可以允许调试器和其他工具在生成js文件时，显示原来的ts代码，如果需要生成inlineSourceMap格式的map文件，则需要开启inlineSourceMap属性，这个属性不能与sourceMap属性一起用，声明文件也想生成sourceMap文件，开启declarationMap属性即可），lib（控制lib.d.ts声明文件的库定义文件）

如果需要开启输出诊断信息，则true一下diagnostics属性，会输出编译时的信息，例如编译时间（Total time）

allowUmdGlobalAccess，是否允许在模块中以全局变量的方式来访问UMD模块

moduleResolution，模块解析的策略，默认为node（相对，非相对导入模块），还有classic策略（该策略，相对导入会解析同级目录下的ts，.d.ts文件，非相对导入会通过node_modules目录来查找模块，如果本级没有，会向上级查找，该模式可用于AMD，System，ES2015模块标准）


如果开启了incremental增量编译，会生成一个tsconfig.tsbuildinfo的文件，这个文件是增量编译的依赖文件，如果需要指定增量编译的依赖文件的存储位置，可以使用tsBuildInfoFile属性

outFile属性是将多个互相依赖的文件整合成一个文件，一般用于AMD模块标准中，属性值是被整合成的文件名以及路径


编译阶段去除全部注释：使用removeComments属性，设置为true

如果编译阶段不想输出文件（就是只编译不生成），可开启noEmit属性

如果想编译阶段发生错误时不输出文件，可开启noEmitOnErrot属性

如果在编译时不生成helper工具函数，可开启noEmitHelpers属性，并且搭配ts-helpers包使用（如果不搭配该包使用，会导致__extends方法未定义），也可以搭配importHelpers属性使用，开启该属性会使用tslib来引入helper函数（tslib就包含了__extends方法），但是这个文件必须模块，helper工具函数会增加文件体积


如果想ES3或者ES5使用迭代器（降级使用迭代器），可开启downlevelIteration属性，例如扩展符的实现支持是通过helper工具函数的__spread方法来完成的

开启严格模式，strict，为true时， alwaysStrict（是否对代码注入use strict），noImplicitAny（是否不允许隐式的any类型，开启后必须有类型注解），strictNullChecks（是否不允许将null，undefined赋值给其他类型的变量），strictFunctionTypes（是否不允许函数参数双向协变），strictBindCallApply（是否使用严格的bind，call，apply检查，开启后bind，call，apply的指向不能为空（undefined，null），并且严格按照类型传值），strictPropertyInitialization（类的实例属性是否必须初始化，开启后必须对类的实例进行初始化），noImplicitThis（是否不允许this存在隐式的any类型，由于ts对this类型检查不好，因此会对this使用any类型，开启后this将不允许存在any类型）属性都将默认为true

如果还需要额外的进行检查，可开启noUnusedLocals（是否不允许存在只声明，但是不使用的局部变量），noUnusedParameters（是否不允许未使用的函数参数），noImplicitReturns（函数中是否必须要有返回值），noFallthroughCasesInSwitch（是否不允许switch语句的case贯穿（fallthrough错误））属性

switch语句的case贯穿：当switch语句没有break时，switch语句会依次执行




模块解析策略，moduleResolution，当mould配置为AMD，UMD，System，ES6时，默认为classic，否则为node

设置基准目录，baseUrl，解析非绝对路径模块名时的基准目录，当为'./'时，表示在tsconfig.json所在的目录开始查找文件

路径设置，paths，将模块路径映射到相对于baseUrl定位的其他路径配置，例如：

    "paths": {
          "@src/*": ["src/*"],
          "test": ["src/test/test.ts"]
      }
      ...
      import test from 'test'


指定多个目录作为根目录（创建一个虚拟目录），rootDirs，允许编译器在这些目录中解析相对于的模块导入，例如：

    'rootDirs': ['src','build']

这个将表示src目录和build目录属于同一个虚拟目录，如果build目录存在依赖，但是ts源码文件在src目录下，需要使用build目录的依赖时，可以使用这个，不需要编译后再修改依赖路径

指定类型文件的根目录，typeRoots，默认node_modules/@types下的包都是可见的，如果指定了typeRoots将表示从该目录里查找类型文件


指定全局范围的包，types，默认情况下typeRoots的包都会被包含到编译过程中，除非指定types，只有列出的包才能被包含在全局范围中，例如：

    "types": ['node','jest','express']


如果想在编译阶段打印输出的文件，可开启listEmittedFiles属性

如果想在编译阶段打印进行编译的文件（其中包括引用的声明文件），可开启listFiles属性


allowSyntheticDefaultImports表示是否允许默认导出，当为true时表示一个模块没有默认导出，也认为其他模块也能像默认导出一样导入使用该模块



esModuleInterop表示是否开启ES模块的互操作，可以使用ES模块的方式导入Commonjs包，当模块没有类型时，可直接require()方式引用，如果具备类型声明，那么需要使用import的方式导入，开启该选项，会默认开启allowSyntheticDefaultImports


开启esModuleInterop属性，将允许export = xxx的形式导出，并且使用import = xxx 的方式导入

sourceRoot表示指定调试器需要定位的ts文件位置，而不是相对于源文件的路径

mapRoot表示指定调试器需要定位的sourcemap文件的位置，而不是生成的文件位置

inlineSourceMap是否开启将sourcemap文件内容生成内联字符串的方式写入到对于的js文件中，而不是生成.js.map文件

inlineSources，是否开启将源文件的所有内容生成内联字符并且写入到sourcemap文件中，和inlineSourceMap功能类似



experimentalDecorators，是否开启装饰器的特性

emitDecoratorMetadata，是否允许装饰器使用反射数据的特性

skipLibCheck，表示是否可以跳过检查声明文件，开启可以省去不少编译时间，但是会牺牲类型系统的准确性

因为ts是大小写敏感的，为了避免开发者在不同大小写敏感规则下的操作系统下开发而导致的问题，可以开启forceConsistentCasingInFileNames，开启该选项表示当开发者正在使用和系统不一致的大小写入规则时，会抛出错误

files表示指定ts项目需要包含的文件列表

include表示指定需要包含在ts项目的文件或者文件匹配路径，如果配置了files，那么默认值为[]，如果没有设置默认值为['**/*']，就是包含目录下的所有文件

exclude表示指定解析include配置中需要跳过的文件或者文件匹配路径，例如node_modules



extends配置指为字符串，表示声明当前配置需要继承另一个配置的路径，ts会向加载extends的配置文件，在使用当前的tsconfig.json文件的配置来覆盖继承文件里的配置




















