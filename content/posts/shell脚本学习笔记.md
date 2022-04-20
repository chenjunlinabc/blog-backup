---
title: "shell脚本学习笔记"
categories: [ "默认" ]
tags: [ "shell" ]
draft: false
slug: "120"
date: "2021-11-23 00:01:00"
---

shell中文意思为壳，shell可以接受使用者的指令，来调用服务，shell的种类很多，而比较常见是Bash，头部声明#!/bin/bash，表示bash是解释脚本的程序

例如：

    #!/bin/bash
    echo "hallo word"


执行该脚本（注意：需要有可执行权限，sudo chmod +x hallo.sh）

bash hallo.sh

也可以直接执行

./hallo.sh



变量（首字符必须是字母，而且不能有bash的关键字，大小写敏感）

data = "hallo"
echo $data
data = "word"
echo ${data}


注意：如果使用单引号，单引号里面有变量的话，是无法生效的，而且还不能出现单引号，而且双引号可以使用变量和转义字符，美元符号（$）只有在使用变量才需要，在定义，更新，删除变量都不需要

例如：

name="chenjunlin"
data="hallo, ${name} !"
echo $data


获取字符串的长度

data="hallo word"
echo ${#data} 

获取指定位置的字符
data="hallo word"
echo ${#data:5:8} 

删除变量（不能删除只读变量）

unset data
echo ${data} 


在shell脚本定义的变量只能在当前脚本交互中使用，可以通过export传递变量到子shell中，可以通过env或者export指令来获取当前shell的环境变量



参数

echo "hallo word";
echo "要执行的shell脚本：$0";
echo "参数为：$1";


./hallo.sh abc


获取参数的个数：$#

以单一字符串的方式输出全部参数（要被双引号包裹）：$*

以独立字符串的方式输出全部参数（要被双引号包裹）：$@

获取上个命令的状态（是否执行成功，0为成功，非0为失败）：$?

获取当前脚本shell进程的ID：$$

获取后台运行的最后一个进程的ID：$!


配合函数使用（参数也可以通过函数来传递）

    function abc(){
        echo "参数1: $1"
        echo "被执行的脚本为: $0"
    }
    abc hallo

./hallo.sh

参数1: hallo 
被执行的脚本为: hallo.sh


数组（不需要逗号分开，使用的是空格）

data = (1 2 3 4 5 "hallo word")
data[6] = "shell 666"
echo "${data[6]}"



获取数组的长度

${#data[*]}或者${#data[@]}"


删除数组或者元素（如果不带索引则删除全部元素）

unset data[5]

数组切片

echo "${data[@]:2:4}"

数组元素更新

echo "${data[@]/2/100}"


数学运算（bash并不支持运算，需要利用第三方工具）

abc = 123
xyz =666
let data = ${abc} + ${xyz}
echo $data

或者

data = \`expr $abc + $xyz\`
echo $data

data = $[$abc + $xyz]
echo $data

data = $(($abc + $xyz))
echo $data


注意：shell的关系运算符不支持字符串，只支持数字

判断是否相等：-eq
判断是否不相等：-ne
判断是否大于：-gt
判断是否小于：-lt
判断是否相等或者大于：-ge
判断是否相等或者小于：-le

例如：

    abc = 123
    xyz =666
    if [ $abc -eq $xyz ]
    then
        echo"相等"
    else
        echo"不相等"
    fi
    if [ $abc -gt $xyz ]
    then
        echo"大于"
    else
        echo"不大于"



布尔运算符

&&和||

或：-o

与：-a

非：!


例如：

    abc = 123
    xyz = 666
    if [ $abc -eq $xyz -o $abc -lt $xyz]
    then
        echo"相等或者小于"
    else
        echo"不相等或者小于"
    fi


字符串运算符

    abc = "hallo"
    xyz = "hi"
    if [$abc = $xyz];then
        echo"相等"
    else
        echo"不相等"


判断字符串长度是否为0：-z（为0返回true）
判断字符串长度是否不为0：-n（不为0返回true）
判断字符串是否不为空：$（不为空返回true）


例如：

    a = "hallo word"
    b = "abchhh"
    if [ -z $a];then
        echo "$a :字符串长度为0"
    else
        echo "$a :字符串长度不为0"
    fi
    if [ ${b}];then
        echo "$b :字符串不为空"
    else
        echo "$b :字符串为空"
    fi



文件运算符

检查文件是否为目录（是返回true）：-d
检查文件是否为块设备文件：-b
检查文件是否为字符设备文件：-c
检查文件是否为普通文件（不是目录，也不能设备文件）：-f
检查文件是否可读：-r
检查文件是否可写：-w
检查文件是否可执行：-x
检查文件大小是否大于0（大于返回true）：-s
检查文件（目录）是否存在：-e
检查文件是否设置了SGID位：-g
检查文件是否设置了SUID位：-u
检查文件是否是有名管道：-p
检查文件是否设置粘着位(Sticky Bit)：-k


例如：

    data = "/etc/hallo"
    if [-r $data]
    then
        echo "文件可读"
    else
        echo "文件不可读"
    fi
    if [-e $data]
    then
        echo "文件存在"
    else
        echo "文件不存在"
    fi





echo和printf

echo可以输出普通字符串，转义，变量

开启转义功能：echo -e "hallo!!! \n"

使用单引号可以原封不动的输出字符串

将执行结果指向到文件中

echo "hallo word" > data


printf比较高级点，可以定义字符串的对齐方式，宽度等等，而且printf默认不自动添加换行符（echo自动添加），可以手动\n

printf "%-8s %-5s %-6.3s\n" root data main 6.12345

上面这句脚本的%s表示输出字符串（类似的还有$c(整型输出),$d（字符输出）,$f（小数输出））

%-8s表示一个宽度为8个字符，如果有-表示左对齐，没有就是右对齐，全部字符都被输出到8个字符宽的字符中，如果不足8个字符，则自动以空格填充，超出的话，也会全部显示（不会忽略）

%-6.3s表示将其格式化为小数，.3表示保留3位小数



test命令用来判断条件是否成立，数值，字符，文件都可以判断

test命令使用的是关系运算符，例如：

    data1 = 300
    data2 = 200
    if test $[data1] -gt $[data2]
    then
        echo "data1大于data2"
    else
        echo "data1小于data2"
    fi



if判断（注意：如果else分支没有语句要执行，就不要写else了）

if
then
elif
else
fi

for循环

    for data in 1 2 3
    do
        echo "data : $data"
    done

while循环

    data = 1
    while(( $data <= 10))
    do
        echo $data
        let "data++"
    done

until循环

    data=1
    until [ ! $data -lt 10 ]
    do
       echo $data
       data=`expr $data + 1`
    done

case ... esac语句（替代switch语句）

    echo '请输入1到3之间的数字:'
    echo '输入的数字为:'
    read data
    case $data in
        1)  echo '1'
        ;;
        2)  echo '2'
        ;;
        3)  echo '3'
        ;;
        *)  echo '没有输入1到3之间的数字'
        ;;
    esac


跳出循环

break（终止执行后面的循环，一般搭配判断使用）

continue（只终止当前循环，不会终止执行后面的循环）



函数

    demo(){
        echo "hallo word"
        return $(($1+2))
    }
    demo 100 300
    echo " $? "








