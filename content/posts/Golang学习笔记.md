---
title: "Golang学习笔记"
categories: [ "学习" ]
tags: [ "golang" ]
draft: false
slug: "96"
date: "2021-09-09 01:00:00"
---

Golang又叫go语言，golang是它的全称，是由Google开发的一种静态强类型，编译型，并发型，并具有垃圾回收功能的编程语言

go语言确保达到静态编译语言的安全和性能的同时，又达到动态语言的开发维护效率

Go语言天生支持并发，提供自动垃圾回收机制

go的源文件是xxx.go

值得一提的是哔哩哔哩网站后端就是用golang开发，这个足以说明golang的并发功能有多强大了


检查是否安装成功go

go version


环境配置

GOROOT对应着go的安装目录

GOPATH对应着go的源代码目录（可以放多个目录）

GOBIN对应着 go install安装和编译的二进制程序的安装目录

检查go环境

go env

源程序默认为UTF-8编码，;可省略


---

第一个go程序

    package main
    import "fmt"
    func main(){
        fmt.Println("hallo golang")
    }

go run hallo.go


当然作为一个编译型语言，编译成二进制文件是支持的

go build hallo.go


---


作为一个静态强类型语言，如果学过java的话，理解还是很轻松的

定义包 package : 必须在源程序上声明该文件是属性那个包的

引入包 import : 导入包，引用外部包开扩展功能


注释

// 单行

/*
多
行
*/


标识符命名规范：第一个字母必须是字母或者下划线，而且不能是关键字，特殊符号只支持下划线



常用的数据类型有：

整型：int（有符号），uint（无符号），rune，int/unint(8,16,32,64)

浮点型：float(32,64)，comple

布尔型：bool（true，false）（bool默认值为false）

字符串型：string

数组：array

结构体：struct

---

变量

var abc string = "hallo"

注意：如果声明变量了，但是没有初始化，那个该变量的值为系统默认设置的值（零值）

定义多个变量

var abc xyz string = "hallo","word"


而且go会根据提供值来判断数据类型是什么，例如：

var xyz = 666


go还提供一个特殊的运算符 :=，可以在变量不被声明的情况下使用，例如：

hallo,word := "hallo", "word"

:=在实质开发中会经常使用的


go类型强制转换（和java一样，高精度转为低精度会失真）

var abc int = 666

var xyz float = 3.14

var gg string = "123"

float(abc) // 强制转换为浮点数

int(xyz) // 强制转为整型，会失去小数点之后的数值

strconv.Itoa(abc) // 强制转为字符串

strconv.Atoi(gg) // 强制转为整型


注意：同一作用域中不能重复声明，而且必须要声明变量才能使用，而且必须要使用

声明多个变量

var abc,xyz,hallo int

    var(
        abc int
        xyz float
    )

---


常量

和其他语言一样，常量表示的就是不可修改的变量

const abc, xyz int = 123, 666

go还提供了一种特殊的定义方式，例如：

    const(
        a = "hi"
        b = "hallo"
    )


如果声明多个常量时，省略了值那么就是表示和第一行值相同，例如：

    const(
        a = 123
        b
        c
    )



注意：定义的时候必须赋值

iota常量计数器

    const(
        a = iota
        b
        _
        d
    )

iota在出现const关键字时重置为0，const每增加一行常量声明，iota加1（自增长），_（下划线，空白标识符（占位），常用于忽略函数多个返回值，例如err）为跳过某些值



---

类型

golang的数据类型分为基本数据类型和复合数据类型

基本数据类型有整型，浮点型，字符串，布尔型

复合数据类型有数组，函数，切片，结构体，字典（map），通道（channel），接口


注意：像整型有很多种，像int8，int16，int32，uint8，uint16等等，如果直接写int的话，在不同的操作系统中是不一样的，32为操作系统的int指的是int32，64位操作系统指int64

可使用unsafe.Sizeof查看变量的长度（在内存中的存储空间），例如：

    var num100 = 100
    fmt.Println(unsafe.Sizeof(num100))



值类型：值类型被声明变量后，不管有没有赋值，都会分配内存给它，也就是说值类型的声明不需要分配内存，因为在声明的时候已经分配好了内存

引用类型：变量存储的是一个地址，地址对应的内存空间就是真正存储数据的，引用类型必须申请内存才能使用，例如make()

自定义类型：可以使用type关键字来自定义类型，例如：type Testint int // 将Testint定义为int类型

类型转换

注意：高位转低位会出现精度丢失，例如

    var abc int16 = 666
    fmt.Println(int8(abc)) // -102



数字字面量语法

go1.13版本+，引入了数字字面量语法，方便使用二进制，八进制，十六进制的格式定义数字，例如:

    abc := 0b10100010101000001011
    fmt.Println(abc) // 666123

0b表示二进制

0o表示八进制

0x表示十六进制



进制的转换


    var abc int64 = 100
    fmt.Println(abc)
    a := strconv.FormatInt(abc, 2) // 二进制
    fmt.Println(a)
    b := strconv.FormatInt(abc, 8) // 八进制
    fmt.Println(b)
    c := strconv.FormatInt(abc, 10) // 十进制（默认）
    fmt.Println(c)
    d := strconv.FormatInt(abc, 16) // 十六进制
    fmt.Println(d)


浮点型（也就是小数）

Go的浮点型分为float32和float64

float32的浮点数的最大范围为3.4028234663852886e+38，可用math.MaxFloat32输出查看

float64的浮点数的最大范围为1.7976931348623157e+308，可用math.MaxFloat64输出查看


    var pi = math.Pi
    fmt.Printf("%f\n", pi)    // 默认小数点6位
    fmt.Printf("%.10f\n", pi) // 指定输出小数点后几位


字符串

go语言字符串编码为UTF-8

例如：
    hallo := "你好 Word"

可输出多行字符串

    var str123 = `第一行
    第二行
    第三行`
    fmt.Println(str123)


len()：字符串长度

+或fmt.Sprintf：拼接字符串

strings.Index()：返回字符在字符串中的位置

strings.contains：返回是否包含某个字符


布尔类型


var abc = false
var xyz = true


byte和rune类型

组成字符串的元素叫字符，可遍历字符串获取字符

go语言的字符分为2种类型，分别为uint8（byte，表示ACII码的一个字符）和rune类型（表示一个UTF-8字符）

因此使用非ACII码的字符，需要使用rune类型（一个汉字占3个字节，字母只占一个字节）

注意：修改字符串必须将其转换为byte或者rune类型，完成修改后再转回string，例如：

    hallo := "你好,golang"
    abc := []rune(hallo)
    abc[0] = '您'
    fmt.Println(string(abc))


整型转字符串类型

    var a int = 100
    b := fmt.Sprintf("%d", a)
    fmt.Printf(b)


字符串转整型或者浮点型

    str := "666"
    str1 := "3.14"
    strScore, err := strconv.Atoi(str)
    fmt.Println(strScore, err) // err为转换失败的信息
    num, err := strconv.ParseInt(str, 10, 64)
    fmt.Println(num, err)
    num1, err := strconv.ParseFloat(str1, 10)
    fmt.Println(num1, err)


---





运算符

+：加，-减，*乘，/除，%求余

注意：在go中，++和--是单独使用的，是没有++i的，正确写法例如：

    var i int = 64
    i++
    // ++i 错误






---



if判断

    package main
    import "fmt"
    func main(){
        abc := 123
        if abc >666{
            fmt.Println("abc大于666")
        } else if abc < 666{
            fmt.Println("abc小于666")
        }else{
            fmt.Println(abc)
        }
    }

if的另一种写法：

    if abc:=123; xyz>=100{
        fmt.Println("abc>=100")
    }




switch判断（用于对大量的值进行判断）


    xyz := "abc"
    switch xyz {
        case "abc":
            fmt.Println("1")
        case "123":
            fmt.Println("2")
        default:
            fmt.Println(xyz)
    }

switch判断的另一种使用方法

    switch abc := "hallo"; abc {
        case "hallo":{
            fmt.Println("hallo")
            break
            // 不用break也能跳出switch语句
        }
	case "hi","hello":{
            // 多个值用逗号分隔
            fmt.Println("hi")
            fmt.Println("hello")
            break	
        }
	default:{
            fmt.Println("hallo word")
            break
        }
    }

如果想继续执行下一个case，可以使用fallthrough语句（switch最后一个分支不要使用fallthrough语句，否则报错，而且不能在case语句中间使用，必须是在case语句最后一个语句中使用）例如：


    func hallo() int {
	abc := 100 + 666 + 123
	return abc
    }
    func main() {
	switch abc := hallo(); {
	case abc > 500:
	    fmt.Printf("num < 500\n")
	    fallthrough
	case abc > 666:
	    fmt.Printf("num < 666\n")
	    fallthrough
	case abc > 123:
	    fmt.Printf("num < 123\n")
	}
    }





for循环

    for abc := 1; abc<10 ; abc++{
        fmt.Println(abc)
    }


golang是没有while循环语句，但是可以使用for循环来做出类似的功能

    abc := 1
    for abc<10 {
        fmt.Println(abc)
        abc++
    }


for循环可用break，goto，return，panic语句退出循环

break跳出循环


一次性跳出多层循环 break LOOP


continue是退出当前循环

goto是无条件转移到goto语句的行，可用于跳出循环，条件转移，例如：


    func main() {
        var abc int = 123
	LOOP:
        for abc < 666 {
            if abc == 233 {
                abc = abc + 1
                goto LOOP
            }
            fmt.Printf(abc)
            abc++
        }
    }

return可用于函数或者方法中，用于跳出当前函数或者方法，如果return语句在main函数中，将是终止程序运行，在普通函数中，将是终止当前函数执行（return后面的程序不执行了）

当return没有带返回值，将是终止，如果带返回值，那么就是终止并且返回值


go语言并没有像java那样的异常嵌套机制，go语言用panic-and-recover来替代

panic可中断原来的流程控制，某个函数调用了panic，将终止该函数的执行，如果有延迟函数（defer）则执行该defer函数，返回其调用者，一直到goroutine（goroutine是go语言的轻量级线程，由runtime管理，go语言会智能的将goroutine中的任务合理分配给每个CPU，go程序在运行时，会给main()函数建立一个默认goroutine，用go关键字创建goroutine，例如：go 函数名(参数)）的全部函数都返回，然后打印panic信息，堆栈信息，最后到终止程序

panic：在任何地方都可以触发
recover：只在用了defer修饰的函数内有效

注意：平时不要用panic-and-recover，而是errors，只有当程序不能继续执行了才用，例如出现不可恢复的错误，不能再让它继续执行了，这里举个例子：

    func abc() {
	fmt.Println("hallo word")
    }
    func hallo() {
	defer func() {
	    err := recover()
	    if err != nil {
		fmt.Println("hallo word")
	    }
        }()
        panic("runtime error!!!")
    }
    func main() {
	abc()
	hallo()
    }


errors（在go语言中，发生错误，是通过返回errprs值，来对errprs值进行修改或者忽略，当没有错误时，rrprs值为nil，因此可以通过判断errors的值是否为nil来知道是否出现错误，以及处理错误）


for range循环（可用于遍历数组，字符串，字典（map），切片（数组），channel（通道））

for range循环数组，字符串，切片返回索引和值，字典（map）返回键和值，channel（通道）返回通道内的值

例如：

    var abc = "hallo word"
    for key, index := range abc {
        fmt.Printf("%v:%c,", key, index)
    }



---


数组（在go中，数组是指同一系列，同一类型的数据集合，组成数组的数据叫元素，go语言的数组的元素是被分配到连续的内存地址中，索引元素是速度非常快的）

    var abc [16]int // 定义int类型，元素个数为16的数组
    abc[0] = 10
    abc[1] = 66
    fmt.Println(abc)
    var xyz = [3]string {"hallo word","golang","hahaha"}
    fmt.Println(xyz)
    var a = [...]int{1, 2, 3, 4, 5} // 自动判断数组长度
    fmt.Println(a)
    b := [...]int{1: 66, 3: 100}
    fmt.Println(b)


遍历数组

    hallo := [...]int{1: 66, 3: 100}
    for i := 0; i < len(hallo); i++ {
        fmt.Print(hallo[i], "\n")
    }


也可以用for range方法

数组是值类型，将数组赋值给另一个变量，会生成副本，修改另一个变量，只会修改副本，不会修改原来的数组，例如：

    hallo := [...]int{1: 66, 3: 100}
    abc := hallo
    abc[1] = 100
    fmt.Println(hallo, abc)

可以看到在golang中数组不是引用类型

二维数组

    var abc = [...][2]int{{100,123},{222,333},{666,60}}
    fmt.Println(abc)
    for i := 0; i < len(abc); i++ {
        for j := 0; j < len(abc[0]); j++ {
            fmt.Println(abc[i][j])
        }
    }

数组的初始值为nil（nil表示为空）

知识点：指针和引用类型的默认值为nil，需要分配空间

---


切片

切片和数组类似（切片是基于数组类型进行一层封装），不过切片是引用类型（调用的只是在内存中的地址，指针）的


    var hallo = []int{100, 2333, 666}
    abc := hallo
    abc[0] = 123
    xyz = abc[1:2]
    fmt.Println(hallo, abc, xyz)
    for i := 0; i < len(abc); i++ {
        fmt.Print(abc[i], "\n")
    }

长度用len()获取，容量用cap()获取

make函数创建切片

    var data = make([]int, 2, 8) // int类型，长度为2，容量为8
    fmt.Printf("长度：%d, 容量%d", len(data), cap(data))

append()切片扩容

    var hallo = []int64{100, 2333, 666}
    hallo = append(hallo, 123)
    fmt.Println(hallo) // 100, 2333, 666, 123
    abc := []int64{1,3,4,5,6}
    hallo = append(hallo, abc...) // 将两个切片合并
    fmt.Println(hallo) // 100,2333,666,123,1,3,4,5,6

copy()函数复制切片（可以理解为深拷贝）

    var hallo = []int64{100, 2333, 666}
    var abc = make([]int64, len(hallo), len(hallo))
    copy(hallo, abc)
    abc[0] = 4
    fmt.Println(hallo,abc)


删除切片的值（golang中并没有删除切片的值的方法，不过可以用append()实现）

    var hallo = []int64{100, 2333, 666}
    hallo = append(hallo[:2], hallo[3:]...)
    fmt.Println(hallo)

遍历切片和数组一样



---

map（字典）

map（字典）是无序的基于key-value的数据结构，在golang中map是引用类型

    var abc = make(map[string]int) // string是键的类型，int是键对应的值的类型
    abc["user"] = 123
    abc["pass"] = 666
    fmt.Println(abc)
    fmt.Println(abc["pass"])

或者

    var hallo = map[string]string {
        "user":"root",
        "pass":"abchahaha",
    }
    fmt.Println(hallo)


遍历map

    var hallo = map[string]string {
        "user":"root",
        "pass":"abchahaha",
    }
    for key, value := range hallo {
        fmt.Println("key:", key, " value:", value)
    }


判断map中某个键值是否存在

    var hallo = map[string]string {
        "user":"root",
        "pass":"abchahaha",
    }
    value, yes := hallo["pass"] // 返回2个值。value为返回结果，yes为是否存在该键值
    fmt.Println(value, yes)



delete()删除键值对

    var hallo = map[string]string {
        "user":"root",
        "pass":"abchahaha",
    }
    delete(hallo, "pass")
    value, no := hallo["pass"]
    fmt.Println(value, no)


---



通道

Golang中还有一个特殊的类型chan，这个类型一般用来线程之间的数据传输

声明chan

var a chan int
a := make(chan int, 1)
a <- 999
b := <- a

chan底层是指针，指针初始值为空，需要实例化，make()就是实例化了chan

<- 999：将值放进通道

<- a : 将 999 从通道中提取出来



---


函数


函数通过func关键字来声明和定义函数

    func hallo(a ...string){
        fmt.Println(a)
    }
    func main(){
        hallo("hallo golang!!!")
    }


匿名函数


    func main() {
	func () {
	    fmt.Println("我是匿名函数")
	}()
    }

因为没有函数名，没法像正常函数那样被调用，需要将其赋值或者作为立即执行函数


返回值


    func hallo(a, b int) (int,int,int) {
        x := a+b
        y := a-b
        z := a*b
        return x, y, z
    }
    func main(){
        abc, xyz, go := hallo(6,3)
        fmt.Println(abc, xyz, go)
    }


当然也是可以自动返回

func hallo(a, b int) (x int,y int,z int){}




golang的全局变量和局部变量

全局变量：常驻内存中，污染全局

局部变量：不常驻内存中，不污染全局

闭包：让变量可以常驻内存的同时，不污染全局

闭包的写法：



    func hallo() func() int {
	i := 123
	return func() int {
	    return i + 666
	}
    }
    func main() {
	var abc = hallo()
	fmt.Println(abc())
    }

或者

    func hallo() func(a int) int {
	var i = 10
	return func(a int) int {
	    i = i + a
	    return i
        }
    }
    func main() {
	var abc = hallo()
	fmt.Println(abc(666))
    }



defer修饰语句可以延迟执行语句，例如：

    fmt.Println("1")
    defer fmt.Println("2")
    fmt.Println("3")

如果使用多个defer修饰语句，会逆序执行（先使用defer最后执行，最后使用defer，最早执行（相对于使用defer修饰的语句））



---



在golang中的time包提供了时间显示和测量时间的函数

获取时间（年-月-日）例如：

    timedata := time.Now()
    year := timedata.Year()
    month := timedata.Month()
    day := timedata.Day()
    fmt.Printf("%d-%02d-%02d \n", year, month, day)


格式化日期（在golang中，并且不是使用Y-m-d H:M:S模板格式化，而且是使用Go的诞生时间：2006年1月2日 15点04分）

例如：

    timedata := time.Now()
    fmt.Println(timedata.Format("2006-01-02 15:04:05")) // 24小时制
    fmt.Println(timedata.Format("2006-01-02 03:04:05")) // 12小时制


获取时间戳（时间戳是自1070年1月1日08:00:00GMT至今的总毫秒数，又叫Unix时间戳）

例如：

    timedata := time.Now()
    unixTime := timedata.Unix() // 毫秒时间戳
    unixNaTime := timedata.UnixNano() // 纳秒时间戳
    fmt.Println(unixTime,unixNaTime)

时间戳转正常日期

    var abc = time.Unix(1642746020, 0)
    var xyz = abc.Format("2006-01-02 15:04:05")
    fmt.Println(xyz)

正常日期转时间戳

    var abc = "2022-01-21 14:21:55"
    var tmp = "2006-01-02 15:04:05"
    timedata, err := time.ParseInLocation(tmp, abc, time.Local)
    fmt.Println(timedata.Unix())

时间的间隔（两个时间之间的间隔，单位为纳秒，time.Duration是time定义的类型，表示一段时间间隔，最大可以表示290年）


    data := time.Now()
    abc := data.Add(time.Hour)
    xyz := data.Add(time.Second)
    fmt.Println(abc) // 输出1个小时后的时间
    fmt.Println(xyz) // 输出1秒后的时间


---



指针

golang的指针有3个概念，地址，类型，取值

&：获取地址，*：根据地址取值


    a := 100
    b := &a
    fmt.Println(b) // 获取地址
    fmt.Println(*b) // 根据地址取值
    *b = 666 
    fmt.Println(a) // 根据地址修改值，改变内存中的值，会改变原来的变量值


注意：指针必须在创建内存后才能使用（其他引用类型也是一样，需要用make分配空间或者在定义的时候分配空间，值类型在声明的时候已经分配了默认空间，因此值类型不用分配空间）


new关键字分配内存（new是一个内置函数，调用new函数得到的是指定类型的指针，并且指针对应的值为该类型的零值）


    abc :=  new(int)
    fmt.Printf(abc)
    fmt.Println(*abc)


make和new的区别：虽然这两个都是用来内存分配的，不过make是用于map，channel，切片（slice）的初始化，返回的值为这3个类型的本身，而new是用来类型的内存分配，内存对应的值为类型的零值，返回的值为指向类型的指针



---






Golang语言是面向对象语言又不是，虽然有类型和方法，也支持面向对象的编程风格，但是go没有对象（object）这个类型，也没有类（class）的概念，在go中用结构体替代面向对象语言的类（class）

type关键字（go通过type关键字定义结构体，结构体是值类型）


自定义类型（type也可以来定义自定义类型）

    type dataStr string

dataStr自定义类型具备string类型的特性


    type dataStr string
    func main() {
        var abc dataStr = "hallo word"
        fmt.Printf("%v %T", abc, abc)
    }


类型别名（只是别名，实质上还是同一个类型）

    type dataStr = string



结构体的定义以及初始化（type关键字和struct关键字）

    type Data struct{
        user string // 定义结构体
        age int
        pass string
    }
    func main(){
        var data Data // 实例化
        data.user = "root"
        data.age = 20
        data.pass = "123456789"
        fmt.Printf("%#v", data)
    }


也可以使用new关键字实例化

    func main(){
        var data = new(Data) // 实例化
        data.user = "root"
        // 等于 (*data).user = "root"
        data.age = 20
        data.pass = "123456789"
        fmt.Printf("%#v", data) // 结构体指针
    }

在golang中支持直接对结构体指针使用，来访问结构体的属性

另外几种实例化结构体的方法

    var data = &Data{
        data.user = "root",
        data.age = 20,
        data.pass = "123456789",
    }
    fmt.Printf("%#v", data)
    

    var data = Data{
        data.user = "root",
        data.age = 20,
        data.pass = "123456789",
    }
    fmt.Printf("%#v", data)


    var data = Data{
        "root",
        20,
        "123456789",
    }
    fmt.Printf("%#v", data)



结构体的方法和接收

    type Data struct{
        // 定义结构体
        user string 
        age int
        pass string
    }
    func (d Data) DataMain() {
        // 定义方法
        fmt.Print("user:", d.user)
        fmt.Print("age:", d.age)
        fmt.Print("pass:", d.pass)
        fmt.Println()
    }
    func (d *Data) GetData(user string, age int, pass string)  {
        // 接收方法，因为结构体是值类型，需要使用指针
        d.user = user
        d.age = age
        d.pass = pass
    }
    func main() {
        var data = Data{
            "root",
            20,
            "123456789",
        }
        data.DataMain()
        data.GetData("admin", 22, "abc12345")
        data.DataMain()
    }


输出结果为：

user:rootage:20pass:123456789
user:adminage:22pass:abc12345




自定义类型方法（类型一样可以定义方法）

    type dataStr string
    func (d dataStr) dataInfo(){
        fmt.Println("hallo golang") // 自定义类型的自定义方法
    }
    func main() {
        var abc dataStr = "hallo word"
        abc.dataInfo()
    }


结构体匿名字段（go允许字段在声明的时候没有字段名，只有类型，因为结构体要求字段名唯一，因此在同一个结构体中同种类型的匿名字段只能出现一次）



    type Data struct{
        // 定义结构体
        string 
        int
        string
    }
    func main(){
        var data = Data{
            // 结构体匿名字段
            "root",
             20,
             "123456789",
        }  
    }


注意：结构体的字段类型可以是任意类型（包括自定义类型，结构体类型），但是如果类型是引用类型（例如map，指针）需要先使用make分配空间再使用


结构体嵌套


    type Data struct{
        // 定义结构体
        user string 
        age int
        pass string
        datamain DataMain //嵌套DataMain结构体
    }
    type DataMain struct{
        // 定义结构体
        email string
        phone string
    }

    func main() {
        var d Data
        d.user = "xiaochen"
        d.age = 20
        d.pass = "123456789"
        var datamain DataMain
        datamain.email = "a@cjlio.com"
        datamain.phone = "18888888888"
        d.datamain = datamain
        fmt.Printf("%#v", d)
    }

输出结果为main.Data{user:"xiaochen", age:20, pass:"123456789", datamain:main.DataMain{email:"a@cjlio.com", phone:"18888888888"}}

嵌套结构体可能出现字段名相同，go默认先从父结构体查找，如果没有再到子结构体中查找，这时如果子结构体存在相同的字段，会报错，因为不知道该设置哪个字段（所以字段名要全局唯一）


结构体继承（可以理解为类的继承，实质效果和结构体嵌套类似）


    type Data struct{
        // 定义结构体
        user string 
        age int
        pass string
        DataMain //通过结构体嵌套实现继承
    }
    func (data Data) datamax()  {
        fmt.Printf("email:  %v \n", data.email)
    }
    type DataMain struct{
        // 定义结构体
        email string
        phone string
    }
    func (datamain DataMain) dataabc()  {
        fmt.Printf("email: %v  \n", datamain.email)
    }
    func main() {
        var data = Data{
            user: "root",
            DataMain: DataMain{
                email: "a@cjlio.com",
            },
        }
        data.datamax();
        data.dataabc();
    }

可以看到Data结构体拥有DataMain结构体的方法

注意结构体的字段首字母要大写，表示公有，小写为私用

go结构体和json序列化

将结构体转换为json叫json序列化，将json转换为结构体叫json反序列化


json序列化和json反序列化主要依赖于encoding/json包的json.Marshal()方法和json.Unmarshal()方法


    type Data struct{
        // 定义结构体
        User string 
        Age int
        Pass string
    }
    func main() {
        var data = Data{
            User: "root",
            Age: 20,
            Pass: "123456789",
        }
        // 结构体转换成Json（返回值为是byte类型的切片）
        jsonByte, _ := json.Marshal(data)
        // byte类型转string类型
        jsonStr := string(jsonByte)
        fmt.Printf(string(jsonByte))
        fmt.Printf(jsonStr)
    }


json字符串转结构体

    type Data struct{
        // 定义结构体
        User string 
        Age string
        Pass string
    }
    func main() {
        // Json字符串转换成结构体
        var str = `{"User":"root","Age":"20","Pass":"123456789"}`
        var data = Data{}
        // 第一个参数是传入byte类型的json字符串，第二个参数需要传入转换的地址
        err := json.Unmarshal([]byte(str), &data)
        if err != nil {
            fmt.Printf("转换失败 \n")
        } else {
            fmt.Printf("%#v \n", data)
        }
    }



结构体标签（tag）

tag是结构体的元信息，可以在运行时通过反射的机制读取出来，tag在结构体字段后面定义，用反引号包裹，tag是以键值对的方式组成，不同的tag用空格分隔


    type Data struct{
        // 定义结构体，并且使用结构体标签
        User string `json:"user"`
        Age string `json:"age"`
        Pass string `json:"pass"`
    }
    func main() {
        var data = Data{
            User: "root",
            Age: "20",
            Pass: "123456789",
        }
        jsonByte, _ := json.Marshal(data)
        // byte类型转string类型
        jsonStr := string(jsonByte)
        fmt.Printf(jsonStr)
        var str = `{"User":"admin","Age":"22","Pass":"abc12345"}`
        var datajson = Data{}
        err := json.Unmarshal([]byte(str), &datajson)
        if err != nil {
            fmt.Printf("转换失败 \n")
        } else {
            fmt.Printf("%#v \n", datajson)
        }
    }



---






包(package)是多个源码的集合，是一种代码复用方案，像fmt，time，encoding/json都是go的内置包

go中的包分为3种，内置包（go提供的内置包，可以直接引入使用），自定义包（自己写的包），第三方包（也是自定义包，不过不是自己写的，需要下载到本地才能使用）


包管理器（go mod）（在1.11版本之前需要使用自定义包的话，需要将项目放在GOPATH环境变量中，1.13之后将彻底不需要GOPATH）


初始化项目（生成go.mod来管理项目的依赖（包括go版本和要使用到的包））

go mod init go_test

如果要引入go_test项目的包，需要import "go_test/包名"，包名根据package设置


package 包名


注意：包名不能和文件夹的名字相同，包名不能出现-符号，一个文件夹中直接包含的文件只能归1个package，同一个package的文件不能在多个文件夹中，而且只有引入了包名为main的程序，编译后会得到可执行文件，如果没有包含main包的程序编译不会得到可执行文件


init()初始化函数：导入包，自动触发包内部的init()函数的调用

go会先从main包开始检查其导入的所有包，每个包又可能导入了其他包，因此形成了一个树状包引入关系，根据引入的顺序来决定编译的顺序

最后导入的包最先初始化并且调用其init()函数

golang第三包仓库https://pkg.go.dev/

go install 编译并安装包（当存在GOBIN环境变量时，编译完成的二进制文件放到$GOBIN下，如果不存在默认放到GOPATH/bin下，源码默认在$HOME/sdk下）

go install github.com/tal-tech/go-zero@1.4.1



go get 全局安装包（Go 1.17版本中已被弃用，推荐使用go install ）

go get github.com/tal-tech/go-zero


go mod download 全局安装包

依赖自动下载到$GOPATH/pkg/mod目录，多个项目可共享缓存的mod，使用该命令之前需要在项目引入第三方包

go mod vendor 将依赖复制到当前项目的vendor中，需要在项目引入第三方包

go mod vendor

其他命令

go mod edit 编辑go.mod文件

go mod tidy 自动处理go.mod中多引入和少引入的包（没有使用的module移除，缺少引入的module将自动引入构建，确保go.mod与模块中的源代码一致）

go mod  graph 打印模块依赖图

go mod  verify 校验依赖，检查下载的第三方库是否本地修改，如果没有修改则返回0（校验成功），否则返回非0（校验失败）

go mod why 解释为啥需要包

使用Modules

GO111MODULE：1.12版本之前的，要设置环境变量GO111MODULE，之后就不需要了通过设置GO111MODULE来开启或者关闭go module

GO111MODULE = off  禁用go module，编译时在GOPATH和vendor中查找包
GO111MODULE = on  启用go module，编译时忽略GOPATH和vendor，只根据go.mod下载依赖
GO111MODULE=auto 默认值，当项目在GOPATH/src之外，并且项目的根目录有go.mod文件时启用go module

windows设置GO111MODULE

set GO111MODULE=on|off|auto

MacOS或者Linux设置GO111MODULE

export GO111MODULE=on|off|auto


off和auto，下载的包安装在GOPATH/src目录下

no，下载的包安装在GOPATH/pkg/mod/下，也在这个目录下查找包（不在GOPATH/src查找）

也可以手动修改环境变量，GO111MODULE变量，值为on|off|auto

GOPROXY：GO代理服务器，是Go官方提供的中间代理的方式来包下载，需要设置GOPROXY环境变量

常见的代理服务器地址：

goproxy.io；
goproxy.cn：由国内的七牛云提供

一键设置GOPROXY：

windows：go env -w GOPROXY=https://goproxy.cn,direct

Linux或者macOS：export GOPROXY=https://goproxy.cn

注意：go语言在1.13版本后，GOPROXY默认为https://proxy.golang.org，如果下载缓慢或者无法访问请设置为https://goproxy.cn

也可以手动修改环境变量，GOPROXY变量，值为https://goproxy.cn

依赖的安装（注意：需要移除把项目从GOPATH移除（GOPATH下不允许有go.mod），否则报错$GOPATH/go.mod exists but should not）

go get下载指定版本的依赖包

go get -u 升级项目中的包到最新的次要版本或者修订版本
go get -u=patch 升级项目中的包到最新的修订版本
go get 包名@版本号 下载对应包的指定版本或者将对应包升级到指定的版本，版本号可以是v1.x.x之类的，也是可以是git的分支，tag，git提交的哈希值


手动修改go.mod，执行go mod download或者使用go get



---





---



go的接口（interface）是一种抽象数据类型，接口定义了对象的行为规范，只负责定义规范，并不实现，接口的规范实现由具体的对象来实现

接口是一组函数method的集合，接口不能存在任何变量，接口中的全部方法都没有方法体



    type TestData interface{
        // 定义一个TestData接口
        test()
        data()
    }
    type Data struct {
        // 实现接口
        User string
    }
    func (d Data) test()  {
        fmt.Println(d.User, "Test")
    }
    func (d Data) data()  {
        fmt.Println(d.User, "Data")
    }
    func main() {
        var datamain TestData = Data{
            "root",
        }
        datamain.test()
        datamain.data()
    }



空接口（接口允许不定义任何方法，不定义任何方法的接口就是空接口）


    type Maxdata interface {
        // 定义一个空接口，空接口表示没有约束，任何类型都能实现空接口
    }
    func main() {
        var abc Maxdata
        var str = "hallo word"
        abc = str
        fmt.Println(abc)
    }

Go1.18版本已添加any关键字来表示泛型

    func main(){
        var abc any
        abc = 'hallo any'
        fmt.Println(abc)
    }

实质上any还是空接口interface{}的类型别名，type any = interface{}

空接口也可用来当做类型，表示任意类型（类似于Java中的Object类型）

空接口还可以用来当做函数的参数，表示可以接收任意类型的函数参数

    func Data(abc interface{}) {
        fmt.println(abc)
    }

使用空接口来实现可以保存任意类型的map

    var dataInfo = make(map[string]interface{})
    dataInfo["uesr"] = "root"
    dataInfo["age"] = 20
    dataInfo["pass"] = "123456789"

使用空接口来实现一个空接口类型的切片

    var dataslice = make([]interface{}, 6, 6)
    dataslice[0] = "root"
    dataslice[1] = 20
    dataslice[3] = "123456789"


类型断言

接口的值是由具体类型和具体类型的值组成，称为接口的动态类型和动态值

判断空接口的值的类型，需要使用类型断言，语法格式为：类型为interface{}的变量.(断言这个变量可能是的类型)

例如：

    var abc interface{}
    abc = 123
    value, isInt := abc.(int)
    if isInt {
        fmt.Println("int类型, 值为：", value)
    } else {
        fmt.Println("不是int类型，断言失败")
    }



---







Go并发

还是讲一下进程和线程的区别

进程（Process）是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源的分配和调度的基本单位，每个进程都拥有一个自己的地址空间，进程至少有5种基本状态，分别是：初始状态，执行状态，等待状态，就绪状态，终止状态

线程（Thread）是进程的执行实例，是程序执行的最小单位，是操作系统能够进行运算调度的最小单位，一个进程可以创建多个线程，同一个进程的线程共享进程的内存信息，同一个进程的多个可以并发执行，一个线程要执行，必须至少有一个进程

并发和并行的区别

并发：指一个时间段中有多个线程（程序）被快速的轮换执行（处于启动运行到运行完毕之间），在一个时间段中只有一个线程（程序）在执行，在宏观上感觉是多个线程同时被处理，如果多线程操作在一个cpu上，操作系统将cpu运行时间分隔为若干个时间段，将时间段分配给各个线程执行，在一个时间段的线程执行时，其他线程将处于挂起状态，这就叫并发

并行：当系统拥有多个cpu（1个以上）时，一个cpu执行一个线程，另一个线程执行另一个线程，同时进行处理，这就叫并行

并发和并行的区别：并发一个时间段只能执行一个线程（多个线程需要排队执行），并行可以在一个时间段中同时执行多个线程

当多线程程序在单核CPU上执行时，就是并发，在多核CPU执行时，就是并行，当线程数大于CPU核数，那么既有并行又有并发

go的主线程和协程（goroutine）：主线程可调起多个协程，而多协程就是可以实现并行或者并发了

每个goroutine (协程) 默认占用内存远比 Java 、C的线程少（goroutine是2kb，加上协程调度内存开销也比线程少，因此goroutine是轻量级线程）

通过go关键字开启goroutine即可实现协程功能，goroutine的调度由golang运行时进行管理，例如


    var wg sync.WaitGroup // 协程计数器
    func data1() {
        for i := 0; i < 5; i++ {
            fmt.Println("hallo word")
            time.Sleep(time.Millisecond * 100)
        }
        wg.Done() // 程序结束则协程计数器减1
    }
    func data2() {
        for a := 0; a < 5; a++ {
            fmt.Println("hallo golang")
            time.Sleep(time.Millisecond * 100)
        }
        wg.Done() // 程序结束则协程计数器减1
    }
    func main() {
        wg.Add(1) // 协程计数器加1
        go data1()
        wg.Add(1) // 协程计数器加1
        go data2()
        for i := 0; i < 5; i++ {
            fmt.Println("hallo hahaha")
            time.Sleep(time.Millisecond * 100)
        }
        wg.Wait() // 等待所有的协程执行完毕
        fmt.Println("主线程结束")
    }

通过time.Sleep设置100毫秒定时，可以看到并没有顺序输出，因为这里使用了多协程


go运行时的调度器通过GOMAXPROCS参数来确定使用多少个OS线程来执行，默认值为计算机的cpu核心数，例如32核的计数器，调度器将程序同时调度到32个OS线程上，通过runtime.GOMAXPROCS()设置当前程序发时的CPU逻辑核心数，runtime.NumCPU()获取计算机的CPU核心数

注意：go1.5版本之前，默认使用单核心，1.5版本之后默认使用全部核心数

    Cpu := runtime.NumCPU() // 获取cpu个数
    fmt.Println("cpu核心数:", Cpu)
    runtime.GOMAXPROCS(runtime.NumCPU() - 1) // 设置要使用的CPU数量


看看多协程实质执行效果

    var vg sync.WaitGroup
    func data(num int) {
        for i := 0; i <= 10; i++ {
            fmt.Printf("协程%v输出的%v条数据 \n", num, i)
        }
        vg.Done()
    }
    func main() {
        for i := 0; i <= 10; i++ {
            go data(i)
            vg.Add(1)
        }
        vg.Wait()
        fmt.Println("主线程结束")
    }



通道（channel）是传输数据的一种数据结构（类型），被用来多个goroutine之间传递信息通讯，可以让goroutine发送特定值给另一个goroutine

go语言的并发模型是CSP（Communicating Sequential Processes），goroutine是并发体，channel是通信

channel遵循先入先出（First In First Out）的规则，保证数据的顺序，channel类型是引用类型


声明管道（通过chan关键字声明）

    var ch1 chan int // 声明传递整型的管道
    var ch2 chan string // 声明传递字符串的管道

因为其是引用类型，需要使用make()声明内存空间才能使用

    ch1 = make(chan int, 10) // 创建一个可以存储10个int类型的管道
    ch2 = make(chan string, 10) // 创建一个可以存储10个string类型的管道


管道具备发送数据，接收数据和关闭管道功能，其中发送和接收都使用<-符号表示，例如：

    ch2 <- "hall word" // 将hall word发送到ch2管道里
    data := <- ch2 // 接收ch2管道的数据
    close(ch2) // 通过内置的close函数关闭管道

管道也有容量，长度，用cap(), len()获取

管道阻塞：当管道没有数据，还进行接收，就会出现阻塞，同样当管道容量不足了，还进行发送数据，也会导致阻塞

注意：当管道被关闭，还继续给管道添加数据或者接收数据将导致panic: send on closed channel报错，如果goroutine执行完毕，管道不关闭，将抛出fatal error: all goroutines are asleep - deadlock!错误

另外使用for range循环在管道取值，在使用for range之前一定要关闭管道，使用for循环遍历管道就不需要关闭管道了

goroutine和channel协作



    func data(ch1 chan int) {
        for i := 0; i <= 5; i++ {
            fmt.Println("写入:", i)
            ch1 <- i
            time.Sleep(time.Millisecond * 100)
        }
        wg.Done()
    }
    func dataGet(ch1 chan int) {
        for a := 0; a <= 5; a++ {
            fmt.Println("接收:", <-ch1)
            time.Sleep(time.Millisecond * 100)
        }
        wg.Done()
    }
    func main() {
        ch1 := make(chan int, 5)
        wg.Add(1)
        go data(ch1)
        wg.Add(1)
        go dataGet(ch1)
        wg.Wait()
        fmt.Println("主线程结束")
    }

data函数写入数据到ch1，dataGet函数接收数据，并且可以看到管道写入数据后，会等待接收数据



单向管道（限制管道在函数中只能接收数据或者只能发送数据，管道默认可接收可发送）

声明只可发送的管道（不能接收）

    var ch = make(chan<- int, 5)
    ch <- 10

声明只可读的管道（不能发送）

    var ch1 = make(<-chan int, 5)
    <- ch1

使用select关键字实现多路复用，来从多个管道接收数据，因为管道的特性，没有数据可以接收会导致阻塞

select关键字类似于switch语句，有case分支和default分支，一个case负责一个管道的发送和接收，select会等待某个case通信操作完成，再执行case分支的语句

例如：


    var ch1 = make(chan int, 10)
    ch1 <- 1
    ch1 <- 2
    ch1 <- 10
    ch1 <- 8
    ch1 <- 0
    ch1 <- 23
    var ch2 = make(chan string, 10)
    ch2 <- "hallo word"
    ch2 <- "hallo golang"
    for {
        select {
            case data:= <- ch1:
            fmt.Println("读取ch1的数据：", data)
            case data:= <- ch2:
            fmt.Println("读取ch2的数据：", data)
            default:
            fmt.Println("所有的数据获取完毕")
            return
        }
    }


可以看到管道发送数据和接收数据是按照顺序的，另外使用select获取数据时，不要关闭管道（当使用完再关闭）


并发安全和锁

在并发环境中，可能会出现并发访问的问题，需要使用互斥锁

互斥锁是并发时对共享资源进行控制访问的手段，使用sync标准库的Mutex结构体定义，sync.Mutex有两个指针方法，分别是Lock()和Unlock()


    var mutex sync.Mutex  // 定义锁
    mutex.Lock() // 上锁
    mutex.Unlock() // 解锁

当禁止访问公共资源时上锁，当需要访问公共资源时解锁

互斥锁实质上就是当一个goroutine访问时，其他goroutine不能访问，避免竞争，如果只读不写的话，也不会出现资源竞争的情况，因为写数据，需要保证数据同步，当写数据，又不能保证数据同步就是会出现资源竞争了

读取数据和读取数据之间不会资源竞争的特性衍生出另外一种锁，叫做读写锁

读写锁可以将多个读取并发，同时读取，并且对修改数据是完全互斥的，当一个goroutine修改数据（写）时，其他goroutine不能读取数据也是不能写数据

在go语言中，读写锁用sync.RWMutex定义

    var mu sync.RWMutex // 定义读写锁
    mu.RLock() // 上锁
    mu.RUnlock() // 解锁
    





---


操作文件和目录

使用os.Open读取文件

    file, err := os.Open("./data.txt") // 读取文件
    defer file.Close() // 关闭文件流
    if err != nil {
        fmt.Println("打开文件出错")
    }
    var bytedata = make([]byte, 1024)
    for {
        n, err := file.Read(bytedata) // n为字节数, err是判断是否读取到末尾，值为nil没读取到末尾，值为io.EOF读取到末尾
        if err == io.EOF {
            fmt.Printf("读取完毕")
            break
        }
        fmt.Printf("读取到了%v 个字节 \n", n)
        var strdata []byte
        strdata := append(strdata, bytedata...)
        fmt.Println(string(strdata))
    }


另一种读取方式（bufio，读取大文件推荐使用这个）


    file, err := os.Open("./data.txt") // 读取文件
    defer file.Close() // 关闭文件流
    if err != nil {
        fmt.Println("打开文件出错")
    }
    reader := bufio.NewReader(file)
    var fileStr string
        var count int = 0
        for {
            str, err := reader.ReadString('\n')
            if err == io.EOF {
                fileStr += str
                fmt.Println("读取结束", count)
                break
            }
            if err != nil {
                fmt.Println(err)
                break
            }
            count ++
            fileStr += str
        }
    fmt.Println(fileStr)


读取小文件（ioutil）

    Strdata, err:= ioutil.ReadFile("./data.txt")
    if err != nil {
        fmt.Println("打开文件出错")
    }
    fmt.Println(string(Strdata))


写入文件（os.OpenFile）



    file, err := os.OpenFile("./data.txt", os.O_RDWR | os.O_APPEND, 777) // 打开文件
    defer file.Close() // 关闭文件流
    if err != nil {
        fmt.Println("打开文件出错")
    }
    str := "hallo word"
    file.WriteString(str) // 写入文件



这里os.OpenFile接收3个参数，分别是要打开的文件的路径，打开文件的模式（多个模式用|隔开），文件的权限（和Linux文件权限一样，用八进制表示，读（04），写（02），执行（01），一般为777或者755）

os.O_WRONLY：只读
os.O_CREATE：创建
os.O_RDONLY：只读
os.O_RDWR：读写
os.O_TRUNC：清空
os.O_APPEND：追加



bufio写入

    
    file, err := os.OpenFile("./main/test.txt", os.O_RDWR | os.O_APPEND, 777) // 打开文件
    defer file.Close()
    if err != nil {
        fmt.Println("打开文件出错")
    }
    writer := bufio.NewWriter(file)
    writer.WriteString("hallo word")
    writer.Flush()



ioutil写入

    str := "hallo word"
    ioutil.WriteFile("./data.txt", []byte(str), 777)


复制内容到文件

    Str, err := ioutil.ReadFile("./data.txt")
    if err != nil {
        fmt.Println("读取文件出错")
        return
    }
    ioutil.WriteFile("./data1.txt", Str, 777)



创建目录

os.Mkdir("./test", 777)



删除文件和目录


    os.Remove("data1.txt") // 删除文件
    os.Remove("./test) // 删除目录
    os.RemoveAll("./test1") // 和Remove一样，不过这个会递归删除所有子目录和文件


重命名

    file := "./data.txt"
    err1 := os.Rename(file,"test.txt")
    if err1 != nil {
        panic(err1)
    } else {
        fmt.Println("文件重命名成功")
    }
    folder := "./demo"
    err2 := os.Rename(folder, "demo1")
    if err2 != nil {
        panic(err1)
    } else {
        fmt.Println("目录重命名成功")
    }


---










