---
title: "Golang进阶扩展笔记"
categories: [ "学习" ]
tags: [ "golang" ]
draft: false
slug: "128"
date: "2022-01-16 09:08:00"
---

这篇笔记是进阶学习，如果基础没有看的的话，请去看https://cjlio.com/archives/96.html

并发

go的并发靠goroutine，goroutine由go运行时调度，线程由操作系统调度，go还提供channel来给多个goroutine之间通行，goroutine和channel是go并发模式CSP（Communicating Sequential Process，通讯顺序进程）的实现基础，goroutine的调度在用户态下完成，不涉及内核态（比如内存的分配和释放，都是用户态维护的内存池，成本远比调度OS线程要低的多，可轻松做到成千上万个goroutine）

内核态：程序执行操作系统层级的程序时

用户态：程序执行用户自己写的程序时


常见的并发模型有七种，分别是通讯顺序进程（CSP），数据级并行，函数式编程，线程与锁，Clojure，actor，Lambda架构


CSP（Communicating Sequential Process，通讯顺序进程）：思想就是将两个并发执行的实体使用channel管道来连接起来，全部信息通过channel管道来传输，而且数据的传输是根据顺序来发送和接收的，CSP理论由托尼·霍尔提出

小知识：托尼·霍尔（C.A.R.Hoare），图灵奖获得者，快速排序算法（Quick Sort）也出自这位之手

go的并发编程不需要像java那样维护线程池，go在语言层面内置了调度和上下文切换机制，只需要定义任务，让go运行时来智能合理的调度goroutine的任务给每个CPU，也不需要额外写什么进程，线程，协程，只需要写一个函数，开启一个goroutine就是可以实现并发了


Go运行时会给main()函数建立一个默认的goroutine，当main()结束时，其他在main()执行的goroutine都会被结束（不管有没有执行完成）

goroutine的栈开始时为2kb（OS线程一般为2mb），而且栈不是固定的，可以增大和缩小，大小限制可以达到1GB


GPM调度器是Go对CSP并发模型的实现，是Go自己开发的一套调度系统（GPM分别表示为Goroutine，Processor，Machine）

Goroutine：go关键字创建的执行体，对应则结构体g，这个结构体存储着goroutine的堆栈信息

Processor：负责管理goroutine队列，存储则当前goroutine运行的上下文，会给自己管理的goroutine队列进行调度，例如：暂停goroutine，执行goroutine，当自己的队列处理完毕，将去全局队列中获取，全局队列处理完毕，还可以去其他P的队列去获取，用来处理G和M的通信

Machine：G运行时对操作系统内核线程的虚拟化，映射内核线程（groutine就是被放到这个内核线程的映射虚拟化M中执行）


简单来说就是P管理一组G在M上执行，当一个G阻塞在一个M时，Go运行时创建一个新的M，负责管理阻塞的那个G的P将其他G挂载在新的M上，G阻塞完成时或者G死掉了，回收旧的M


P的个数通过runtime.GOMAXPROCS设置（最大256）（1.5版本后默认为计算机物理线程数）

GPM调度器使用被称为m:n调度的技术（复用或者调度m个goroutine到n个OS线程）（可用runtime.GOMAXPROCS来控制OS线程的数量）


因为底层OS线程的切换机制是根据时间轮询来切换的，因此goroutine的切换机制也是根据时间轮询来切换

runtime.Gosched()：让当前任务让出线程占用，给其他任务执行

runtime.Goexit()：终止当前任务


通道是可被垃圾回收机制回收的，所以只有在告诉接收数据方，所有数据都已发送完毕了才需要关闭通道

对已经关闭的管道发送数据，导致触发panic，同样关闭已经关闭的管道也会导致

对已经关闭并且没有值的管道接收数据，将得到对应类型的零值，接收一个已经被关闭的管道，会一直接收数据，直到管道空了


无缓冲区管道（阻塞管道）：要求管道的发送方和接收方交互是同步的，管道容量等于0的就是无缓冲管道，如果不能满足同步，将导致阻塞，要接收者准备完毕，发送者才能进行工作

有缓冲区管道（非阻塞管道）：可以异步发送数据接收数据，只要缓冲区存在没有使用的空间，通信就是无阻塞的，可先发送数据再接收（因为有缓冲区），而且缓冲区管道可以保存数据（不需要取完数据）


任务池：goroutine池，当goroutine任务完成，不kill该goroutine，而是获取下一个任务，并且继续执行该任务



注意：go内置的map并不是并发安全的，只有使用channel或者sync.Map才是并发安全的


锁可以避免并发冲突，但是锁对系统性能影响很大，原子操作可以减少这种消耗

原子操作：指的是某个操作在执行中，其他协程不会看到没有执行完毕的结果，对于其他协程来说，只有原子操作完成了或者没开始，就好像原子一样，不被分割

在多核中，某个核心读取某个数据是，会因为CPU缓存的原因，可能读取到的值不是最新的，在Go中，原子操作主要依赖于sync/atomic包

sync/atomic包将原子操作封装成了Go的函数，sync/atomic包提供了底层的原子级内存操作

因为Go不支持泛型，所以封装的函数很多（每个类型都有自己的原子操作函数，这里只写int64一个类型）

增或减（被操作值增大或减少，只适合int和uint类型增减）：func AddInt64(addr *int64, delta int64) (new int64)

载入（读取，避免读取过程，其他协程进行修改操作）：func LoadInt64(addr *int64) (val int64)

存储（写入，避免写入过程，其他协程进行读取操作）：func StoreInt64(addr *int64, val int64)

交换（和CAS不同，交换只赋值old值，不管原来的值）：func SwapInt64(addr *int64, new int64) (old int64)

比较并且交换（Compare And Swap 简称CAS，类似于乐观锁，只有原来的值和传入的old值一样才修改）：func CompareAndSwapInt64(addr *int64, old, new int64) (swapped bool)


使用方法：atomic.原子操作函数名


goroutine的特性：非阻塞（不等待），调度器不能保证多个goroutine的执行顺序，全部goroutine都是平等的（不存在父子关系）

固定worker工作池（用固定数目的goroutine来作为工作线程池，提升并发能力），工作通道，结果通道，worker任务管道

从worker任务管道获取任务到工作管道，处理完毕的结果传递到结果通道


context标准库（go1.7版本以及之后版本），用来跟踪goroutine调用树（因为goroutine不存在父子关系，无法靠语法来通知）

Context接口：所有的context对象都要实现该接口，一般用来当作context对象的参数类型，该接口定义了4个需要实现的方法，分别是Deadline()，Done()，Err()，Value()

Deadline()方法：需要返回当前Context被取消的时间（完成工作截止时间）
Done()方法：需要返回一个channel，这个管道会在当前工作完成或者上下文被取消之后关闭
Err()方法：返回当前Context结束的原因
Value()方法：会从Context中返回键对应的值

canceler接口：规定了通知取消的Context对象要实现的接口

empty Context结构：实现了Context接口，但是不具备任何功能（该结构体的方法是空方法），该结构被用来当做Context对象树的根（root节点），模拟一个真的goroutine树

cancelCtx类型：实现了Context接口的具体类型，并且同时实现了canceler接口，不但具备退出通知功能，还能将退出通知告诉整个children节点

timerCtx类型：实现了Context接口的具体类型，并且封装了cancelCtx类型实例，具备一个deadline变量，用来实现定时退出通知

valueCtx类型：实现了Context接口的具体类型，并且封装了Context接口实例，具备一个k/v的存储变量，用来传递通知



---







go内置的net/http包提供了http客户端和服务端的实现，性能媲美nginx（每一个请求都有一个对应的goroutine去处理，并发性能好）


http服务端

    func h(w http.ResponseWriter, r *http.Request) {
        fmt.Println(r.RemoteAddr, "连接成功")
        fmt.Println(r.Method, r.URL.Path, r.Body, r.Header)
        str := "hallo word"
        w.Write([]byte(str))
    }
    func main() {
        http.HandleFunc("/test", h)
        http.ListenAndServe("127.0.0.1:8080", nil)
    }

访问http://127.0.0.1:8080/test，就可以看到hallo word了，控制台还是输出一些有关请求的信息



http客户端（get请求，这个东西也可以用来做go爬虫）

    res, err := http.Get("https://cjlio.com")
    if err != nil {
        fmt.Println(err)
    }
    body, err := ioutil.ReadAll(res.Body)
    if err != nil {
        fmt.Println(err)
    }
    fmt.Println(string(body))

get参数通过r.URL.Query()进行识别


post请求


    resp, err := http.Post("https://cjlio.com", "application/x-www-form-urlencoded", strings.NewReader("test=hallo word"))
    if err != nil {
        fmt.Println(err)
    }
    defer resp.Body.Close()
    body, err := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))



---


日志标准库log（log包是go内置的）

log.Println() 普通日志
log.Fatalln() 会触发fatal的日志，写入日志后调用os.Exit(1)
log.Panicln() 会触发panic的日志，写入日志后panic

log会打印日志信息的日期，时间，以及日志信息


可通过log.SetOutput()存储日志到文件中

日志等级（从小到大，默认输出Debug及其以上基本的日志）：

log.Trace（基本输出），log.Debug（调试），log.Info（重要），log.Warning（警告），log.Error（错误），CRIT（严重危险）, ALRT（严重警告），log.Fatal（严重紧急）



---



go test测试工具

在包目录中，所有以_test.go为后缀名的文件，都认为是go test的一部分，不会被go build编译到可执行文件中

在这些以_test.go为后缀名的文件中的函数，分为3种类型，单元测试函数，基准测试函数和示例函数


单元测试函数：函数名前缀以Test开头的，用来测试程序的逻辑是否正常

基准测试函数：函数名前缀以Benchmark开头的，测试函数的性能

示例函数：函数名前缀以Example开头的，提供示例

go test会遍历这些符合规则的函数，并且生成临时的main包来调用测试函数，执行，返回测试结果，最后清理临时测试文件


测试函数必须要导入testing包，例如：func TestData(t *testing.T){}

基准测试函数：func BenchmarkData(b *testing.B)

示例函数：func ExampleData()





---



go变量如果在声明时没有指定初始值，那么该变量初始值为该变量的类型的零值

:=声明方式只能出现在函数中（由go自动判断类型）

go提供自动垃圾回收（Garbage Collector）机制，不需要关注变量的内存管理，go使用逃逸分析(Escape Analysis)技术

go会为变量在两个地方分配内存空间，这两个地方分别是全局的堆(heap)空间和每个goroutine的栈(stack)空间，因为go是自动管理内存空间的，不需要关心这些，但是栈内存和堆内存在性能差别很大

如果分配到栈中，那么当函数执行完毕后自动回收，如果是分配到堆中，会在函数执行完毕后某个时间点进行垃圾回收，而且在栈上分配内存或者回收内存花销都很低，只需要PUSH（将数据PUSH到栈空间）和POP（释放空间）两个CPU指令，因此只是将数据PUSH到内存的时间，效率和内存的I/O成正比

如果是堆中，因为go语言垃圾回收用的是标记清除算法（GC，垃圾回收）（标记要查找存活的对象，清除要遍历堆的全部对象，回收没有标记的对象）（题外知识：JavaScript垃圾回收就是用的这个标记清除算法）

逃逸分析(Escape Analysis)：由编译器决定变量是分配到栈空间还是堆空间，当变量的作用域在函数内部，那么该变量是分配到栈空间上，反之则分配在堆空间，也就是说当变量不能随着函数结束而回收时分配在堆空间（指针逃逸），另外空接口在编译阶段难确定其参数的类型，也会发生逃逸，闭包也会逃逸

另外当栈空间不足也会发生逃逸（因为栈溢出），栈空间被操作系统所限制大小（当栈空间不足时，发生栈溢出）

利用逃逸分析原理提升性能：一般情况下，占用大内存的变量应该使用传指针（堆空间，虽然GC性能没有栈空间那么好，但是传指针只复制指针地址，不对值的拷贝），小内存的变量用传值（栈空间，可获得更好的性能）


---


反射：指程序在执行过程中，可以访问，监测和修改本身状态和行为的能力

Go语言提供了一种机制在运行时更新变量和检查它们的值，调用它们的方法，但是在编译时并不知道这些变量的具体类型，这称为反射机制

Go的反射基础是接口和类型系统，借助接口自动创建的数据结构实现，反射依赖于interface类型，反射的实现靠reflect标准库

Go语言类型分为2大类，static type和concrete type，其中static type是int，string这些在编码时能看到的类型，concrete type是runtime系统才能看见的类型（类型断言依赖于concrete type）

因为Go语言是静态语言，在编译阶段已经被确定了类型，使用interface类型的变量有一个pair，pair被用来记录了实际变量的值和类型，并且这个变量有2个指针，一个指向concrete type，另一个指向实际值


reflect.TypeOf()：该函数的参数是一个空接口类型，返回值为一个type接口类型，返回一个type的接口变量，通过接口抽象出来的方法访问具体类型的信息（用来获取反射对象pair的type）


reflect.ValueOf()：该函数的参数是一个空接口类型，返回值为一个Value类型的变量（用来获取pair的value对象），


reflect.Kind()方法：该方法没有参赛，返回值是字符串类型（用来获取具体类型struct）



例如：

    type Testint int64
    var num Testint = 666
    fmt.Println("type: ", reflect.TypeOf(num))
    fmt.Println("value: ", reflect.ValueOf(num))
    fmt.Println("kind: ", reflect.TypeOf(num).Kind())



---


Go 交叉编译（在一个平台上生成另一个平台的可执行程序）（Mac/Linux/Windows下）

Windows下编译Mac,Linux

Linux

SET CGO_ENABLED=0
SET GOOS=linux
SET GOARCH=amd64
go build main.go

Mac

SET CGO_ENABLED=0 
SET GOOS=darwin
SET GOARCH=amd64
go build main.go



Linux下编译Mac,Windows

Windows

CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go

Mac

CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build main.go



Mac下编译Linux,Windows

Linux

CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build main.go

Windows

CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go



其中CGO_ENABLED=0来控制go build是否使用CGO编译器，1为使用CGO编译器，GOOS表示编译成什么平台的（linux/windows/darwin/freebsd）,GOARCH表示编译成什么平台的体系架构（386/amd64/arm，32位，64位，ARM）

注意：CGO是Go代码中调用C代码交叉编译，但是交叉编译不支持CGO，因此如果存在C代码是不能编译的，需要禁用CGO

---


dep包管理工具


---


命令行参数解析（依赖于内置flag包）


导入flag包

import flag

定义参数

    pass := flag.String("pass", "123", "密码")

可以看到flag.Type方法接收3个参数，分别是flag名，默认值，提示


flag支持Int，Int64，Uint，Uint64，Float，Float64，String，Bool，Duration（时间间隔）等类型



还有一种定义参数的方式

    var pass string
    flag.StringVar(&pass, "pass", "123", "密码")


定义完毕参数后，使用flag.Parse()解析，命令行中指定pass，pass的数据将会保存在pass变量中

flag其他常用函数

返回命令行参数后（未定义的参数）的其他参数
flag.Args()

返回命令行参数后（未定义的参数）的其他参数个数
flag.NArg()

返回命令行参数（定义的）的个数
flag.NFlag()


例如：

    func main() {
        var pass string
        flag.StringVar(&pass, "pass", "123", "密码")
        flag.Parse()
        fmt.Println(pass)
	fmt.Println(flag.Args())
	fmt.Println(flag.NArg())
	fmt.Println(flag.NFlag())
    }


./test -pass hallo 


可使用-help参数查看参数提示