---
title: "python基础语法笔记"
categories: [ "默认" ]
tags: [ "Python" ]
draft: false
slug: "28"
date: "2021-06-16 14:40:00"
---

安装python

推荐安装anaconda3（linux）

wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2021.05-Linux-x86_64.sh

dash Anaconda3-2019.10-Linux-x86_64.sh

根据提示安装，如果还是系统自带的python

sudo gedit ~/.bashrc

export PATH="/home/pc/anaconda3/bin:$PATH"

\# pc为系统的用户名，请修改成自己的系统用户名，请勿在root用户下安装anaconda3

Windows和mac 到官网下载安装包，直接下一步安装

mac安装了Homebrew，可以使用brew install python3

Windows设置环境变量，PATH 安装路径

现在liunx一般都会自带有python3,如果没有可以安装一下

apt install python3

yun install python3

注意一下python2.x和python3.x这两个版本是不兼容的，要区分开

检查是否安装成功，在命令行或者终端输入python3 回车 没有反应或者报错就说明没有安装成功或者可能出问题了
Windows的一定要注意PATH系统变量

因为Python语言从规范到解释器都是开源的，所以存在多个解释器

例如CPython，安装python3.x时就可以直接获取到一个官方版本的解释器：CPython，因为是使用C语言开发的，所以叫CPython

在命令行或者终端，输入输入python3 回车，如果出现了>>> 那么当前状态是python的交互模式

在交互模式下输入exit()，退出python的交互模式

在交互模式下执行第一个程序
print("hello,world")

回车输出hello，world，这是简单的打印字符串

除了使用交互模式执行程序，又可以使用.py，在命令行或者终端下 python hello.py，如果报错，请检查程序是否出错或者该文件是否在当前目录下

交互模式可以直接输出结果，但是使用.py文件却不会，想要输出结果，必须使用print()打印出来

一个好的开发工具往往可以达到事半功倍的效果，例如pycharm和Visual Studio Code，我使用的是Visual Studio Code

print()接受多个输出，使用“,”分隔开，也可以输出整数

当你想让用户输入一点东西的时候，python提供了一个input()，用法如下
name = input()
将输入的值存放到一个变量里

input()还提供了提示功能，显示一个字符串，例如：input("xxxxx")

我想知道这个变量的内容咋办，可以在交互模式下直接输入变量名，回车，就可以查看该变量的内容
又可以使用print()打印下来

那么什么是变量呢？

在计算机程序中，变量不仅可以为整数或浮点数，还可以是字符串

输入是Input，输出是Output，输入输出统称为Input/Output，或者简写为IO

以#开头的语句是注释，注释是给人看的，解释器会忽略掉注释，注释可以是任何内容

其他每一行都是一个语句，当语句以冒号:结尾时，缩进的语句视为代码块

注意：python区分大小写，搞错了大小写，程序可能会报错。缩进建议使用4个空格

python能直接处理的有：整型，浮点型，字符串，布尔值，空值，变量，常量

python可以处理整型任意大小的整数，也可以使用二进制代表整数、

浮点型就是小数，使用科学记数法表示时，一个浮点数的小数点位置是可变的，所以小数又叫浮点型，表示很大或很小的浮点数，必须用科学计数法表示

字符串是使用""和''括起来的任意文本，''或""本身只是一种表示方式，不是字符串的一部分

字符串内部包含'又包含"怎么办？使用转义字符来标识，转义字符可以转义很多字符，比如n表示换行，t表示制表符，字符本身也是可以转义的，所以\表示的字符就是\

为了简化，Python还允许用r''，用来表示''内部的字符串默认不转义

在交互式命令行内输入，在输入多行内容时，提示符由>>>变为...，提示可以接着上一行输入，注意...是提示符，不是代码的一部分

多行字符串'''xxx'''，可以在前面加上r使用

布尔值只有两种值，分别是True和False，在计算机中布尔值要么是True，要么是False，在python中可以直接使用True、False表示布尔值，注意大小写

and是与运算，只有所有值都为True时，and运算结果才是True

or运算是或运算，只要其中有一个为True，那么or运算结果就是True

not运算是非运算，它是一个单目运算符，专门把True变成False，False变成True

空值，使用None表示空值，这个空值不是0，因为0是有意义的，代表什么都没有

在计算机程序中，变量不仅可以是数字，还可以是任意数据类型。

变量在程序中用一个变量名表示，变量名必须是大小写英文、数字和_的组合，且不能用数字开头，

变量名 = 任意数据类型，这个过程叫变量赋值，变量可以反复赋值

这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。

例如golang就是一个静态语言，不能跨数据类型来赋值

x = 1

x = x+1

这个是计算新的值，并且重新赋值给该变量

a = "hello"

这时Python解释器干了两件事情：

在内存中创建了一个'hello'的字符串；

在内存中创建了一个名为a的变量，并把它指向'hallo'。

可以把一个变量a赋值给另一个变量b，这个操作实际上是把变量b指向变量a所指向的数据

因为程序是从上往下执行的，一定要理清逻辑

常量就是不能变的变量，在python中使用大写表示一个常量，例如：
ABC =123

但这个常量还是可以被修改，因为Python没有任何机制可以保证该常量不会被改变，这个很奇怪

/除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数

当使用//除法时，两个整数的相除就可以是整数

想要做精确的除法，就必须使用/了

// 除，永远是整数

字符编码
python提供了字符转编码函数ord()和编码转字符函数chr()

Python可以使用编码来输出内容

Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节，可以转成bytes类型，方便传输和保存数据

在Python中bytes类型的数据用带b前缀的单引号或双引号表示：a = b"hallo word"

str和bytes类型内容显示没有差别，但是bytes的每个字符都只占用一个字节

在Python中，可以使用encode()方法来指定编码，例如：
'hallo'.encode('ascii')
b'hallo'

注意：纯英文的str可以使用ASCII编码为bytes，显示内容一样，但是包含了其他语言（比如中文）的str不能使用ASCII编码，Python会报错，含有其他语言的str请使用UTF-8编码

如果想把bytes转为str，可以使用decode()方法

注意：如果bytes中包含无法解码的字节，decode()方法会报错

如果bytes中只有一小部分无效的字节，可以传入errors='ignore'忽略错误的字节

计算str包含多少个字符，可以用len()函数

len()函数计算的是str的字符数，如果换成bytes，len()函数就计算字节数
为了让Python按UTF-8编码读取，一般都会在文件开头声明一下：
\# !/usr/bin/python3
\# -- coding: utf-8 --

第一行是告诉计算机系统这是一个Python可执行程序，注意Windows会忽视这个

第二行是告诉Python，请按照UTF-8编码读取源代码

注意：声明并不代表文件是UTF-8编码的，请确保文本编辑器使用UTF-8 without BOM编码

如果想一些内容可以根据变量变化，那么就用到格式化字符串

在Python中，采用的格式化方式和C语言是一致的，用%实现

'Hello, %s $%d ' % ('world')

常见的占位符有：

%d -- 整数
%f -- 浮点数
%s -- 字符串
%x -- 十六进制整数

注意：%s永远起作用，它会把任何数据类型转换为字符串！！！

如果字符串里面的%只是普通字符，那么使用%%来代表一个%（转义）

还有一个格式化字符串方法：format()

它会用传入的参数依次替换字符串内的占位符{0}、{1}........

例如：

'hello,{0}'.format('world')

Python 3的字符串使用Unicode，直接支持多语言

当str和bytes互相转换时，需要指定编码

列表（list），一种有序的集合，可以随时添加和删除其中的元素，使用[]来表示，例如：
abc = ['a','b','c']

可以使用索引来访问列表的每一个元素，索引是从0开始，例如：

abc[0]
'a'

注意：索引超出范围时，Python会抛出一个IndexError错误，记得最后一个元素的索引是len(classmates) - 1

如果想获取最后一个元素，不知道索引，又怕抛出错误，可以使用-1方法，例如：

abc[-1]
c

以此类推，可以获取倒数第2个、倒数第3个，-2，-3

因为这个列表是可变的有序的列表，所以可以在列表中追加元素到末尾，例如：

abc.append('d')
abc
['a','b','c','d']

当然也可以添加元素到指定的位置，例如：

abc.insert(1,'1')
abc
['a','1','b','c','d']

删除列表末尾元素，使用pop()方法，例如：

abc.pop()
'd'
abc
['a','1','b','c']

删除指定位置的元素，使用pop()，括号中填写索引位置

abc.pop(1)
'1'
abc
['a','b','c']

如果想把某个元素换成其他元素，可以直接赋值给对应的索引位置，例如：

abc[0]='abc'
abc
['abc','b','c']

列表里面的元素数据类型可以不同，例如：

a = ['hallo',666,False]

列表里面可以包含其他列表，反复嵌套，例如：

a = ['hallo','word',['123'.'abc']]

注意：这个a列表只有3个元素，其中的a[2]又是一个列表，可以拆开理解，例如：

123 = ['hallo','word']

abc = ['666',123,'abc']

想拿到'hallo',可以写成123[1]或者abc1

这个可以看成一个二维数组

注意：当一个列表中一个元素也没有，那么就是一个空的列表，长度为0，a=[] len(a) 0

还有一个有序列表叫元组（tuple），和list很相似，不过这个一但初始化就不能修改

没有添加元素和删除元素的方法，但是可以正常使用获取元素的方法，获取方法和list一样，就是不能赋值成另外的元素

那么这个列表不能改变，那么有什么意义？因为tuple不可变，所以程序更安全

当定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来

如果要定义一个空的tuple，可以写成()

正确定义只有一个元素的tuple，例如：

a = (1,)
a
1

为啥这样呢？因为括号()可以表示tuple，又可以表示数学公式中的小括号，防止Python按照小括号来计算，避免歧义

关于可以'改变'的tuple：实际上改变的是list，因为tuple里面可以有list列表，所以可以'改变'01


---




全局变量和局部变量


一般在函数内部定义的变量，为局部变量，在函数外部定义的变量为全局变量

因为作用域的问题，一般在函数内部不能真正修改到全局变量

可以使用关键字global来处理

    abc = None

    def xyz():

        global abc

        abc="hallo"

        return a

    print(abc)  // 观察这个变量和下面的变量的区别

    print(xyz())

    print(abc)  // 观察这个变量和上面的变量的区别



nonlocal 可以用于在函数或者其他作用域中使用上层（非全局）的变量，例如：

    def xyz():
        abc="hallo"
    
        def go():
            nonlocal abc
            return abc

        return go()

    print(xyz())

函数

调用函数 函数名()

python中内置了大量的函数，可以直接调用，例如：

int("666") // 666


因为函数本身可以理解为是一个引用类型，所以函数是可以赋值给变量，例如：

a = int
a("123") // 123

定义函数

def为定义一个函数的关键字，定义一个函数例如：
    def abc(i):
        print(i)

如果有一个函数定义在另一个文件里，可以在当前文件的当前目录下，使用from abstest import导入函数


空函数：
    def no():
        pass

pass语句用于什么都不干，可以用于占位


检查函数传入的参数

检查数据类型可以使用isinstance()

isinstance(1,int) // 返回布尔值，可以搭配if判断语句使用

如果想要手动设置异常，可以使用raise语句，例如：

    def no(i):
        if not isinstance(i,(int)):
            raise ValueError("no函数的传参必须为整数")

    no('1')

函数内部可以使用return来返回函数结果

使用return返回不了值，主要是没有把值取出来，例如

    def a(x):
        return x

    b = a(1)
    print(b) // 1

python的函数参数灵活度很高

def a(x,i=1)  // 默认i为1

必选参数在前，默认参数在后，当不需要默认参数可以直接覆盖掉，灵活性很高

当参数的个数不明确时，可以把参数当成list或tuple传进来，例如：

    def a(x):
        abc = 0
        for a in x:
            abc=abc+a
        return abc
    a = a([1,2,3])

    print(a) // 6

如果已经有一个list或tuple，想传进来：

    def xyz(*x):
        abc = 0
        for a in x:
            abc=abc+a
        return abc

    b = [1,3,5]
    a = xyz(*b)

    print(a)


关键字参数可以传入0或者任意个参数，而这些参数会变成一个tuple

def abc(i,**n):
    print('i:', i, 'n:', n)

abc("abc")


如果已经有一个tuple，想传入：
b = {1,3,5}
abc(**b)


如果想限制关键字参数，例如：

    def xyz(i,n,*,name,age):
        print(i,n,name,age)
    xyz(1,2,name="hallo",age=18)  // 1 2 hallo 18

*后面的参数被认为是命名关键字参数，限制只能使用该参数名

如果参数中已经有了可变参数，后面跟着的命名关键字参数就不需要*来分隔了

命名关键字参数必须要传入参数名，否则报错，如果命名关键字参数已经有默认值，可以不用传入



递归函数

递归函数就是这个函数反复调用它本身，例如：

从1加到100，1+2+3+...+100

    def xyz(n):
        if n == 1:
        return 1

        return n+xyz(n-1)

    a = xyz(100)

    print(a) // 5050


尾递归优化

递归调用次数过多，会导致堆栈溢出，溢出就是超出了最大上限的堆栈

而解决因为递归而导致的溢出的方法就是尾递归优化

但是Python解释器目前不支持尾递归优化，就算使用尾递归方式，也还是会导致堆栈溢出


    def abc(n):
        return xyz(n,1)

    def xyz(n,go):
        if n == 1:
            return go
    
        return xyz(n-1,n+go)

    a = abc(100)

    print(a)


切片，顾名思义，就是取部分值，例如：

a = [1,2,3,4,5]

a[0:3] // [1,2,3]

索引从0开始，到3为止

支持倒数切片

a[:-1] // [1,2,3,4]

如果开头为0，可以省略不写，倒数第一个为-1

a[::2] // [1, 3, 5] 每两个取一个

a[:] // 复制一个一样的list

tupel也可以使用切片，但是操作结果还是tuple

a = (0,1,2,3)

a[:3] // (0,1,2)

甚至连字符串都可以切片，结果还是字符串

a = "hallo"

a[:2] // hallo



迭代


读写文件

abc = open('1.txt','r')
go = abc.read(6) # 可以指定读取的数量，如果为空则读取全部
print(go)
abc.clsoe() # 这个用来关闭文件

r为只读，w为写入，a为追加，rb为以二进制方式只读，wb为以二进制方式写入，ab为以二进制方式追加

abc = open('1.txt','w')
abc.write("hallo word!!" * 6) # 写入6条数据
abc.close()


abc = open('1.txt','r')
data = abc.readline()
print("3:%s" % data) # 读取指定行数的数据，这里为读取3行数据
abc.close()


如果想多钱全部数据，并且返回的是一个列表，列表的每一个元素为都为每一行数据，可以：
    abc = open('1.txt', 'r') 
    data = data.readlines()                             
    print(type(data)) 
    for a in data: 
        print(a) 
    data.close()




---

if判断

    data = 10
    if data >=18:
        print("成年了!")
    elif data>=6:
        print("义务教育")
    else:
        print("未成年!!!")

---

输入输出

    data = int(input("请输入数据"))
    print(data)



---


类型转换（Python是弱类型语言，弱类型指不能对变量声明类型，只能声明数据的类型）

int()整数,str()字符串,float()浮点数,bool()布尔数，list()列表，tuple()元组，chr()字符，unichr()Unicode字符等等

例如：

    data1 = "123"
    data2 = 123
    print(isinstance(data1,str))
    print(isinstance(data2,int))
    data3 = int(data1)
    data4 = str(data2)
    print(isinstance(data3,int))
    print(isinstance(data4,str))



---

运算符

+加，-减，*乘，/除，//取整除，%取余，**指数

优先级和普通的算数规则一样，指数大于乘和除,取余,取整除大于加和减

字符串不能和整数相加，必须将字符串或者整数转换，字符串为拼接，整数为算数

赋值运算符

=，将右边赋值给左边的变量，可以给多个变量赋值，例如：a = b = c = 666或者a,b,c = 666,123,100

a+=1 ：a=a+1，a-=1：a=a-1，a*=1：：a=a*1，a/=1：：a=a/1,a//=1: ：a=a//1,a%=1：a=a%1 ,a**=1：a=a**1


==比较是否相等，!=比较不相等，>大于，<小于，>=大于等于，<=小于等于


and和运算符（只有当比较全部为true才为true，否则都为false），or或运算符（只有当比较有一个为true才为true，全部为false才为false），not取反运算符（只有当比较为true才为false，当比较为false才为true）


---



for循环和while循环

for循环可以遍历任何以序列排序的东东，比如列表和字符串

    for a in "hallo word":
        print(a)

或者

    fot a in range(1,100):
        print(a)

range支持3个参数，分别是起始，结束，以及步长

while循环,在某个条件下为true下才会执行里面的语句

    data = 0
    while data < 10:
        print(data)
        data+=1


---

获取字符串或者列表的长度，len(data)

查找某些内容是否在字符串的某些地方中存在（如果不存在，返回-1）,find(str,beg=0,end=len(strdata))

str为要查找的字符串，beg开始查找的位置，end结束查找的位置


---

错误处理

读取文件错误（当文件不存在时）
    try:
        abc = open('data.txt', 'r') 
        print(abc.read()) 
    except FileNotFoundError: 
        print('文件没有找到,请检查文件名称是否正确')

try...except语句可以处理程序运行过程中可能出现的异常
