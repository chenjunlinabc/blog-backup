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
    }

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

变量通过let关键字来创建


    fn main() {
        let mut abc = "hallo word";
        println!(" abc: {}",abc);
    }

注意：rust变量默认是不能变得，需要通过mut关键字来让变量可变


常量通过const关键字来创建

    fn main() {
        const ABC = "hallo word";
        println!(" ABC: {}",ABC);
    }


