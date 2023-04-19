---
title: "Rust学习笔记"
categories: [ "学习" ]
tags: [ "Rust" ]
draft: false
slug: "142"
date: "2022-03-28 13:22:00"
---

Rust由非盈利组织Mozilla基金会开发，像著名的Mozilla Firefox浏览器和MDN Web Docs都出自该基金会

安装

官方推荐使用Rustup来安装（Rustup是rust的安装器和版本管理工具）

通过rustup-init来安装Rust

https://www.rust-lang.org/zh-CN/tools/install

windows直接安装pustup-init.exe来安装（需联网）,默认通过vc安装，建议选择2自定义安装

Linux或者macOS则直接执行以下命令

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

注意：Rust的一些包，可能还依赖C编译器，因此最好安装一下GCC

检查是否安装完成

rustc --version

如果喜欢用Visual Studio Code开发，可搭配rust-analyzer插件来使用

更新rust

rustup update

卸载rust和rustup

rustup self uninstall


在安装rustup的同时也会安装Cargo

Cargo是rust的项目构建工具和包管理器

检查是否安装成功

cargo --version

创建第一个rust项目

cargo new halloword

其中Cargo.toml文件是项目的依赖库文件

通过编辑Cargo.toml文件来添加依赖

rust依赖可通过https://crates.io/查找

    [package]
    name = "hallo_word"
    version = "0.0.1"
    edition = "2021"
    [dependencies]
    hyper = "0.14.20" # 来自https://crates.io/
    # hyper = { git = "https://github.com/hyperium/hyper" } # 来自第三方社区
    # hyper = { path = "../hyper" } # 来自本地文件

构建依赖到项目中

cargo build halloword

当构建完成后，会在项目的根目录中创建一个名叫Cargo.lock的文件，该文件记录项目依赖的版本

src/main.rs是该项目的程序编写文件

在src/main.rs添加以下内容

    fn main() {
        println!("hallo, world");
    };

进入该项目根目录运行该项目 

cargo run


或者直接编译main.rs

rustc main.rs
./main

注意：在windows上，需要加.exe，.\main.exe

在hallo word例子中看到，fn是创建函数的关键字，main是函数名，该函数调用了println!()，其实这是一个macro（宏），"hallo, world"作为字符串参数传递到了该宏中

另外cargo提供了一个命令，来不用生成二进制文件的前提下构建项目，来检查项目的错误

cargo check


build和run默认是debug版本的，如果需要编译release版本执行cargo build --release


---

在rust中函数是一等公民，其中main函数是程序的入口函数（和golang一样）

注释可以让rust编译器忽略，例如：// hallo word

多行注释可使用块注释，例如：

    /*
        hallo
        word
    */


文档注释，行注释例如：

    /// 1
    /// 2
    /// 3

文档块注释，例如：

    /**
        hallo
        word
    */


注意：文档注释必须位于lib类型的包中，文档注释看使用md语法

可使用cargo doc命令，来对文档注释生成html文件，放在target/doc目录下，也可以使用 cargo doc --open 生成文档后自动打开html网页


变量通过let关键字来创建


    fn main() {
        let mut abc = "hallo word";
        abc = "hhh";
        println!(" abc: {}",abc);
    };

注意：rust变量默认是不能变得，需要通过mut关键字来让变量可变

变量存在遮蔽性，如果使用let关键重复声明同一个变量，会遮蔽之前声明的变量的值和类型

变量的遮蔽性和mut关键字的区别是，mut关键字无法修改变量的类型，mut关键字修改的只能是相同类型的值


常量通过const关键字来创建

    fn main() {
        const ABC = "hallo word";
        println!(" ABC: {}",ABC);
    };

常量不可变，也不能使用mut，而且必须声明类型（字面量也算是一种类型声明）

---


数据类型

rust是静态型编译语言，因此需要在编译之前就知道全部变量的类型

rust有4大基础类型，这个基础类型又叫标量类型，分别是整型，浮点型，布尔型，字符型

整型就是没有小数的整数（分符号类型和无符号类型，区别是是否为负数，可能为负数使用有符号类型，不可能为负数使用无符号类型）

整型使用u表示无符号类型，用i表示有符号类型，再声明使用多少位的空间，例如i32，就是占32位空间的有符号类型

还有2个特殊的类型，isize和usize，这个类型取决程序执行环境的系统架构，使用64位架构就是占64位空间

整型支持十进制，八进制，二进制，十六进制和字节，rust默认使用i32类型

注意：使用有符号类型会将以二进制补码的方式来存储


浮点类型，就是带小数的数字，rust的浮点型有2个类型，分别是f32和f64，默认是f64，f64是双精度浮点型，f32类型是单精度，并且全部类型都是具备符号的（可为负数）

布尔类型，布尔类型只能是这2个值得其中之一，true和false，布尔类型使用bool关键字来声明，例如 let a: bool = true;,当然也是可以使用字面量来声明

字符类型，字符类型使用单引号包括，并且只能表示一个Unicode字符（因为字符类型只使用4个字节）


字符串类型使用双引号表示，可表示多个字符，使用String表示，例如：

    let a:String = "hallo word";
    prinln!("{}",a);

除了标量类型外还有复合类型，复合类型有6种，分别是元组和数组，枚举，结构体，字符串，切片


元组类型就是无法扩展和收缩的数组（元组可使用不同的类型），长度是固定的，例如：

    let a: (i64, f64) = (100,3.14);

获取元组的元素可通过解构的方式来完成，例如：

    let a: (i64, f64) = (100,3.14);
    let (b,c) = a;

也可以通过索引值来获取，例如：

    let a: (i64, f64) = (100,3.14);
    let b = a.1;

元组的索引是从0开始的


数组类型和元组不同，数组的类型必须是相同类型的，数组长度也是固定的

    let a = [1,233,666];

rust也提供了动态数组（vector），动态数组将被分配到堆内存空间，而普通数组会被分配到栈内存空间

动态数组使用Vec<T>的方式来声明，动态数组也不能使用不同的类型，但是动态数组可以扩展空间和缩小，而且全部元素在内存是相邻排列的，可使用Vec::new()来创建一个空的动态数组，例如：

    let a: Vev<u64> = Vec::new();

当然也可以提供初始值来创建

    let b = vec![2,4,68];

rust可以根据字面量来自动推断出类型

动态数组增加元素使用push方法，例如：

   let mut a: Vev<u64> = Vec::new()；
   a.push(1);
   a.push(2);
   a.push(3);
   a.push(4);
   a.push(5);

注意：动态数组离开当前作用域会被丢弃销毁

获取动态数组的元素，可通过get方法或者索引来完成，例如：

    let a = vec![2,4,68];
    let b = a.get(2);
    let c = &a[2];

get方法和索引的区别是，get方法访问超过数组不存在的元素是不会报错的，而是返回none，而索引访问是会导致报错，程序崩溃的

遍历动态数组

    let a = vec![2,4,68];
    fot i in &a{
        println!("{}",i);
    };

动态数组支持枚举类型，这可以让动态数组存储不同类型的值，例如；

    enum Hallo{
        Int(i64),
        Text(String),
    };
    let a = vec![
        Hallo::Int(666),
        Hallo::Text(Sting::from("hallo word")),
    ];






函数，是rust的第一个公民，main函数是rust程序入口文件，而且函数定义可在main函数之前或者之后，使用fn关键字来声明，例如：

    fn hallo_word(){
        println!("hallo word");
    };
    fn main(){
        hallo_word();
    };


函数参数

    fn main(){
        a(666);
    };
    fn a(x:i64){
        println!("{}",x);
    };


函数返回值

    fn main(){
        a(666);
    };
    fn a(x:i64) ->{
        x+123
    };

注意：返回值使用->返回，而且不能使用分号，因为语句无法作为返回值返回


---



if判断

    let a = 1;
    if a <10{
        println!("true >10");
    }else if(a>0){
        println!("true >10");
    }else{
        println!('none')
    };

注意：if判断必须提供一个布尔类型的值，否则会抛出错误

表达式是可以赋值给变量的，例如：let boola = true; let a = if  boola{1} else{2}


rust的循环有3种，分别是for，while，以及loop

for遍历循环（常用于遍历集合元素），例如：

    let a = [1,2,3,45,6];
    for b in a {
        println!("{}",b);
    };



while循环，条件为真执行，为假停止循环。例如：

    let mut a = 0
    while a <= 10{
        println!("hallo wrod");
        a +=1;
    };


loop关键字可以一直执行语句，直到被跳出（可使用break关键字），例如：

    let mut count = 0;
    loop{
        println!("hallo word");
        if (count == 5){
            break;
        };
        count +=1;
    };

loop循环可以搞个标签，来指定终止哪次循环（嵌套循环下），例如：'a': loop{ break 'a'}


continue操作符可跳出当前循环，去执行下次循环，一般搭配if分支语句来使用


---

所有权


所有权是rust的特性之一，无需gc垃圾回收机制就可以保证内存安全

作用域是一个语句块在程序中有效的范围，在该范围内的变量是有效的，离开该范围后将自动释放内存空间

释放内存空间是使用rust提供的一个叫drop函数，会在结尾}自动调用该函数

当一个变量的值是指针时，赋值给另一个变量，当这2个变量离开作用域后，会导致二次释放，因为这2个变量指向的是同一个内存空间地址，二次释放会导致内存污染，这个指针实质上是在堆上的

rust为了解决二次释放导致的内存安全问题，当一个存在指针值的变量赋值给另一个变量后，会销毁第一个变量，而无需再等到离开作用域，第一个变量将变成是无效的，这个赋值将不是复制，而是剪切板一样，移动数据

如果需要复制堆的数据，可使用clone方法，例如：

    let a = String::from("hallo word");
    let b = a.clone();
    println!("{},{}"a,b);


注意：移动数据只针对栈上的指针之类的有效，如果是赋值的是整型之类存在在栈上的，是不需要移动，因为复制指针这些"引用类型"需要耗费性能，rust通过copy trait标注来决定是否赋值给其他变量后，这个变量是否还有效，只要类型实现了copy trait就可以

支持copy trait的类型：全部整数类型，布尔类型，浮点类型，字符类型，元组（部分类型支持）



为了避免需要引用变量而导致原变量失效，rust提供了引用功能，例如：

    let a = String::from("hallo word");
    let b = &a;

引用不会具备该值的所有权，但是可以指向这个值

引用实质上是借用的，是不能修改借用的值，如果想修改可提前对具备所有权的变量声明mut可变，例如：

    let mut a = String::from("hallo");
    let b = &mut a;

注意：同一个作用域中只能有一个对这个数据进行可变引用，使用多个可变引用会报错，这是为了避免竞争

切片类型也是没有所有权的，对切片类型进行操作，是不会影响到原来的值，例如：

    let a = String::from("hallo world");
    let hello = &a[0..5];
    let world = &a[6..11];
    a.clear();




---


结构体与元组一样，元素可以是多个类型，但是和元组不同的是，结构图具备元素别名，并且使用struct来定义结构体，例如：

    struct Name{
        id: i64,
        username: String,
        email: String,
        pass: String,
    };

上面例子就是一个结构体，这个结构体的名称叫Name，具备4个字段，每个字段有自己对于的字段名称和类型


有结构体当然也有结构体实例，结构体是结构体实例的抽象

    let name1 = Name{
        id: 1,
        username: String::from("root"),
        email: String::form("a@xiaochenabc123.test.com"),
        pass: String::form("123456789"),
    };
    println!("hallo {}", name1.username);
    name1.email = String::from("b@xiaochenabc123.test.com");

注意：结构体实例必须按照结构体每个字段的类型要求进行初始化，不需要按照结构体声明的字段顺序一样

利用函数返回值来完成结构体的实例化，例如：

    fn NameTest(username: String, email: String, pass: String) ->{
        Name{
            username,
            email,
            pass,
        };
    };
    fn main(){
        let name01 = NameTest("user01","test@xiaochenabc123.test.com","12356987");
        println!("hallo {}", name01.username);
    };


基于已有的实例，创建新的实例，例如：

    let name2 = Name{
        username: String::from("uesr02"),
        ..name01
    };

注意：其中..name01是表示其他字段将从name01实例中获取


元组结构体（结构体字段没名称的就叫元组结构体）

    struct Abc(i64,bool,f64);
    let abc1 = Abc(100,true,3.14);

单元结构体（这玩意和go的空结构体一样，只定义一个结构体，但是没有字段和属性）

    struct Xyz;
    let xyz1 = Xyz;

结构体的所有权：当使用已有的实例，创建新的实例时，改变的字段的所有权将会被转移到新的实例，原有的实例的那个字段将失效，其他字段正常，因此使用结构体应该避免使用引用类型


---




枚举


    enum HomeTest{
        Hallo,
        Xyz,
        Abc,
        Hahaha(i64)
    };
    struct Demo{
        home: HomeTest,
        id: i64,
    }
    fn main(){
        let test1 = HomeTest::Hallo;
        let test2 = HomeTest::Abc;
        println!(test1);
        println!(test2);
        let a = Demo{
            home: HomeTest::Abc,
            id: 1,
        }
        let b = Demo{
            home: HomeTest::Hallo,
            id: 2,
        }
    }



上面例子中，通过enum关键字声明并且定义一个枚举类型，这个枚举类型有3个枚举成员，通过::操作符来访问枚举的成员，枚举是一种类型，因此可用于函数参数类型，结构体类型等等


枚举的成员可以是任何类型的数据，也可以限制成员的数据类型，结构体使用{}，单个使用()，多个可使用逗号分割

rust的空值使用Option枚举类型来表示，而不是null

Option，Some，None都包含在prelude标准库中，不需要在源码中声明，或者引用，直接就可以使用

Option枚举类型的实现

    enum Option<T> {
        Some(T),
        None,
    };

表示一个空值


    let null1: Option<i64> = None;

如果使用None，则需要提前提供是什么类型，因为无法通过None来推断出类型

Option比null好的原因是，Option(T)和任何类型都不相同，无法将Option<i64>和i64类型的值进行处理

如果有值则：

    let num1:Option <i64> = Some(233);


如果使用Some，表示有个值存于Some中



使用Option枚举类型：

    fn Test(a:Option<i64>) -> Option<i64> {
        match a {
            Some(i) => Some(i),
            None => None,
        }
    }
    let abc = Some(666);
    let xyz = Test(abc);

上面例子中，通过Test函数传入一个Option<i64>类型的参数，并且返回一个Option<i64>类型的值，使用match模式匹配，如果传入None则返回None，传入Some(i64)，则返回其本身（也就是Some(i64)）


---

模式匹配


match语句

    enum Demo{
        Hallo,
        Abc,
        Xyz,
    };
    let test1 = Demo::Xyz;
    let test2 = match test1 {
        Demo::Hallo => "hallo word",
        Demo::Abc | Demo::Xyz => "hallo abcxyz",
        },
        _ =>"0",
    };


上面例子中进行匹配Demo对应的枚举类型，match会穷尽列出全部已知的可能，如果存在不知道的可能，使用下划线_来表示，match语句与switch语句很像，match的分支必须指向一个表达式，而且每个分支的表达式结果必须是相同类型的





假如只想对单个模式来进行处理，可使用if let语句，例如：

    let test1 = Demo::Xyz;
    if let Test2(Demo::Xyz) = test1{
        println!("hallo wrod");
    }

上面例子中，如果匹配到Demo::Xyz，输出hallo wrod，如果不匹配则忽略

注意：match和if let会在模式匹配时进行覆盖老值，绑定新的值

matches!宏匹配，可以将一个表达式和模式来进行匹配，返回结果是布尔值，例如：

    let a = 1;
    assert!(matches!(1..100 | -1..-100));




---

方法

rust使用impl关键字来定义方法

    struct Demo {
        id: i64,
        user: String,
    }
    impl Demo {
        fn new(id: i64, user: String) -> Demo {
            Demo{
                id: id,
                user: user,
            }
        }
        fn printa(&self){
            println!(self.user)
        }
    }
    fn mian(){
        let a = Demo{
            id: 1,
            user: "root",
        }
        a.printa()
    }


&self为借用Demo结构体，new函数是Demo方法的关联函数，用于初始化结构体实例

&self是self: &Self的简写，self有所有权，而&self是表示不可变借用，&mut self就是可变借用

impl Demo是struct Demo的实现，另外rust允许方法名与结构体的字段名相同，调用方法加()，否则就是在访问字段

rust自动引用和解引用：当一个对象调用方法时，会自动为该对象添加&，&mut，*来确保该对象能与方法匹配

关联函数：当一个存在于impl中，但是没有使用self的函数就被叫关联函数，通常存在impl中的new函数是结构体的构造实例器（rust没有使用new作为关键字，并不强制使用new）

关联函数因为不是方法，需要使用::来调用，例如Demo::new(2,"admin")

rust允许将结构体定义成多个impl块，而不需要全部都写到一块，更灵活扩展

枚举类型也可被实现

---


泛型与特征



泛型，当需要对可能存在不同类型的数据处理，但是处理逻辑是一样时，可使用泛型来避免重复，例如：


    fn test<T: std::ops::Add<Output = T>>(a:T, b:T) -> T {
        a + b
    }
    fn mian(){
        println!("{}",test(1,2));
        println!("{}",test(3.14,1.23));
    }


注意：T是泛型参数，泛型参数是可自定义的，不过一般都是用T（表示Type），T可以表示任何类型，但是不是所有类型可以进行比较运算处理的，需要通过特征（这里是<T: std::ops::Add<Output = T>）来限制T的类型



结构体泛型

    struct Demo<T>{
        a: T,
        b: T,
    }
    fn main(){
        let x = Demo{a: 1,b: 21};
        let y = Demo{a: 1.23, b: 3.14};
        let z = Demo{a: "hallo word", b: "hahaha"}
    }


注意：结构体中使用T泛型的，必须是相同类型，因为第一个字段被赋值，会推断T类型为该赋值的实质类型，因此会报错的

如果想使用类型不同，又想使用泛型，可使用多个泛型参数来分别表示不同的类型


枚举泛型，Option枚举类型就是使用了泛型的，这里就不写了

方法泛型，例如：


    struct Demo<T, U> {
        id: T,
        user: U,
    }
    impl<T,U> Demo<T,U> {
        fn new(id: T, user: U) -> Demo<T,U> {
            Demo{
                id: id,
                user: user,
            }
        }
        fn printa(&self){
            println!(self.user)
        }
    }


注意：使用泛型参数必须在提前声明


const泛型（Rust 1.51 版本引入）

const泛型是针对值的泛型，而不是针对类型，例如：

    fn Demo<T: std::fmt::Debug,const U:i64>(a:[T; U]){
        println!("{:?}" a)
    }
    fn main(){
        let a: [i64:5] = [0,123,666,888,987];
        Demo(a)
        let a: [i64:2] = [0,123];
        Demo(a)
    }


rust在编译阶段会对泛型的多个类型生成独立的代码（单态），会牺牲一些编译完成文件的大小和编译速度




特征

声明一个特征使用trait关键字，例如：


    pub trait Demo {
        fn DemoTest(&self);
    }


实现一个特征

    pub struct Home {
        pub user: String,
        pub pass: String,
    }
    impl Demo for Home {
        fn DemoTest(&self) -> String {
            format!("用户是{},密码是{}",self.user,self.pass);
        }
    }
    pub struct Abc {
        pub admin: String,
        pub rank: String,
    }
    impl Demo for Abc {
        fn DemoTest(&self) -> String {
            format!("管理者是{},管理等级是{}",self.admin,self.rank);
        }
    }

    fn main() {
        let home1 = Home{user: "xiaochen",padd: "abc123456789"};
        let xyz = Abc{admin: "main",rank: "max"};
    println!("{}",home1.DemoTest());
    println!("{}",xyz.DemoTest());
    }



从上面的例子中可以看出，特征是定义了一种行为，实现该特征就可使用该行为，特征和结构体，枚举类型是很像的，但是特征可被共享到每个实现上（因为rust没有继承概念，但是行为/方法是相同的）


注意：特征存在一种规则，想实现一个特征必须至少一个是在当前作用域定义的，这个规则是为了避免不会破坏特征和实现的定义



默认实现（无需实现该方法，默认使用该实现，也可对默认实现进行重载）


    pub trait Demo {
        fn DemoTest(&self) -> String {
            String::form("hallo wrod");
        }
    }


只要在实现中没有重载该方法，就会使用默认实现




特征作为函数参数

    pub fn Demo1(data: &impl Demo){
        println!("{}",data.DemoTest());
    }


在上面例子中，data函数参数实现了Demo特征，当然也可以使用其他实现了Demo特征的类型来作为参数



特征约束

    pub fn Demo2<T: Demo> (data: &T){}


上面例子就是特征约束，特征约束可限制函数参数必须是实现了Demo特征的类型


多重约束

    pub fn Demo3<T: Demo + AbcTest> (data: &T){}

或者

    pub fn Demo3(data: &(impl Demo + AbcTest)){}


上面例子就是多重约束，可限制参数必须使用Demo特征，还必须实现AbcTest特征



Where 约束（简写多重约束）

    pub fn Demo4<T>(data: &T) -> String
        where T: Demo + AbcTest,
        {}


特征约束高级应用


    impl <T:Dome1 + Dome2> Demo<T> {
        fn Test1(&self){
            println!("hallo word");
        }
    }

上面例子中，只有实现了Dome1特征和Dome2特征的Demo<T>才拥有Test1方法



impl Demo也可以声明返回了一个类型，这个类型实现了Demo特征，例如：

    fn Demo5() -> impl Demo {
        Home{
            user: String::from("xiaochen"),
            padd: String::from("abc123456789"),
        };
    }






---

集合类型



集合类型，在rust中是特殊的类型，因为集合类型可以表示多个值，而其他数据类型大多只能表示一个值，集合类型的值被分配到堆内存上，集合类型分3种，分别是vector动态数组（每个元素分配的空间都是一致的，大小，宽度，高度），HashMap KV存储（每个元素都是成对，一个k对应着另一个v），以及String类型

创建vector动态数组

    let mut a: Vec(i64) = Vec::new();

注意：rust编译器可通过a.push()自行推导出数据类型，因此不显式声明类型也是没问题的

还可以通过vec!宏来创建，例如：

    let mut a = vec![123,666,888,1000];

更新动态数组，通过push方法来完成，例如：

    let mut a = Vec::new();
    a.push(123);

读取元素，有2种方式，分别是下标索引，get方法，例如：

    let mut a = vec![123,666,888,1000];
    let b: &i32 = &a[3];
        match a.get(3){
                Some(c) => println!("{c}"),
                None => println!("没有这个元素")
        }

集合类型的下标索引从0开始，&a[3]是借用a动态数组的第4个元素，也就是1000，最后获取导该元素的引用

get方法的返回值是Option<&T>，因此需要match来匹配解构出目标值

下标索引和get方法的区别，很简单，就是下标索引会出现数组越界问题（报错），而get方法通过Option<&T>。如果不存在该值会返回None（安全，不报错）

动态数组和其他类型一样，超出作用域外，会被自动销毁

注意：动态数组的大小是可变的，因此当原数组大小不够时，会分配一个更大的内存空间，再将原数组拷贝到这个更大的内存空间的新数组上，因此在push之前不要进行任何引用避免分配到新内存空间后，之前的引用指到一块无效的内存，rust编译器会自动检查，如果在push之前进行了引用（包括不可变借用和可变借用），如果在push之后没有使用，就正常通过，如果使用了，会报错

可创建指定大小的Vector，通过Vec::with_capacity(10)来完成

迭代遍历动态数组元素

    let mut a = vec![123,666,888,1000];
    for i in &mut a {
        *i += 1;
        println!("{i}");
    }

通过枚举类型和特征对象来实现动态数组存储不同类型的元素，例如：

枚举类型

    enum Abc {
        id(i64),
        user(String),
        padd(String),
    }
    fn useraddr(abc:Abc){
        println!("{:?}",abc);
    }

    fn main(){
        let a = vec![
            Abc::id(1); 
            Abc::user("admin".to_string()); 
            Abc::padd(String::from("123456")); 
        ]
        for i in a {
            useraddr(i);
        }
    }

特征对象

    trait Abc {
        fn useraddr(&self);
    }
    struct id(i64);
    imp Abc for id {
        fn useraddr(&self) {
            println!("id: {:?}",self.0)
        }
    }

    struct user(String);
    imp Abc for user {
        fn useraddr(&self) {
            println!("user: {:?}",self.0)
        }
    }

    struct padd(String);
    imp Abc for padd {
        fn useraddr(&self) {
            println!("padd: {:?}",self.0)
        }
    }
    fn main() {
        let a: Vec<data<dyn Abc>> = vec![
            data::new(id(1)),
            data::new(user("admin".to_string())),
            data::new(padd("123456".to_string())),
        ];
        for i in a {
            i.useraddr();
        }
    }

注意：必须表示数组中存储的是哪个特征的对象，例如上面例子中的Vec<data<dyn Abc>>，表示是特征Abc的对象

HashMap KV存储和动态数组不同，存储的是映射的KV键值对，可通过一个键查询到值，查询效率非常高，复杂度为O(1)

创建HashMap KV存储

可通过new方法创建，例如：

    use std::collections::HashMap;
    let mut hashdata = HashMap::new();
    hashdata.insert("uesr","admin");
    hashdata.insert("padd","admin12345");

注意：HashMap没有包含在Rust的prelude中，需要手动从标准库中use引入

可创建指定大小的HashMap，通过HashMap::with_capacity(10)来完成

可以通过迭代器和collect方法创建，例如：

    use std::collections::HashMap;
    let DataList = vec![
        ("牛奶".to_string(), 3),
        ("方便面".to_string(), 4),
        ("可乐".to_string(), 3),
    ];
    let MapData: HashMap<_,_> = DataList.into_iter().collect();
    println!("{:?}",MapData)

注意：HashMap的所有权取决于kv的类型，如果类型实现了Copy特征，那么改类型被复制到HashMap中，如果没有实现，则所有权转移到HashMap

获取HashMap元素通过get方法，例如：

    use std::collections::HashMap;
    let mut abc = HashMap::new();
    abc.insert(String::from("雪糕"), 4);
    let test = String::from("雪糕");
    let testSearch: Option<&i32> = abc.get(&test);
    let testScore: i32 = testSearch.get(&test).copied().unwrap_or(0);

上面例子使用了借用规则，避免获取元素时，发生所有权的转移，并且使用Option<&i32>类型，如果没有该对象，返回None，安全

更新HashMap的中，例如：

    use std::collections::HashMap;
    let mut data1 = HashMap::new();
       data1.insert("薯条", 5);
       let a = data1.insert("薯条", 10);
    assert_eq!(a, Some(5));
       let b = data1.get("薯条");
    assert_eq!(b, Some(&10));
    let c = data1.entry("啤酒").or_insert(5);
    assert_eq!(*c, 5);

上面例子中包含覆盖值；查询新插入的值；查询目标值，如果不存在则插入新值，3种情况

注意：不是所有类型都可以作为hashmap的key，能否作为取决于类型是否实现了std::cmp::Eq特征（相等比较），目前HashMap使用的哈希函数是SipHash散列函数







---

类型转换


---

包和模块


---

格式化输出



---


生命周期


---


错误处理