---
title: "Deno js运行时环境学习笔记"
categories: [ "学习" ]
tags: [ "JavaScript","Deno" ]
draft: false
slug: "161"
date: "2022-11-11 07:00:00"
---

Deno是基于V8引擎，使用Rust构建的JavaScript & TypeScript 运行时环境，天生支持TypeScript，并且有安全模式（默认情况下无法获取网络，文件系统，环境变量等权限，当然也可以开放），Deno的作者是Nodejs之父Ryan Dahl，构建的原因是解决Nodejs的缺陷，例如模块的安全性（Node运行时的权限很高，缺乏模块的安全运行），Deno的模块化选择了ESMoule标准，而且具备浏览器的api，例如window全局变量，支持onload，onunload等事件函数，支持fetch，Web Workers等标准，异步操作返回采用Promise，支持await

Deno不使用node_modules与package.json的包管理机制，而是采用下载编译的机制，并且存在缓存，模块更新通过更新缓存来完成

Deno有个特殊的功能，就是可以从网络上导入模块

安装

Linux

curl -fsSL https://deno.land/install.sh | sh

或者

windows

iwr https://deno.land/install.ps1 -useb | iex


也可以通过scoop安装

scoop install deno

作为Rust构建的，当然也支持Cargo包管理器安装

cargo install deno --locked

或者通过单一的可执行文件来安装（我采用这个方式，再配置一下Path环境变量就是可以了，windows选择deno-x86_64-pc-windows-msvc）

https://github.com/denoland/deno/releases

检查是否安装完成

deno --version



第一个例子


    import DFetch from "https://deno.land/x/dfetch/mod.ts"
    DFetch.get("https://xiaochenabc123.test.com").then((response) => {
        console.log(response)
    })


运行

deno run .\index.ts

它会向你询问进行网络请求，是否允许，可通过--allow-net默认运行，例如：deno run --allow-net .\index.ts

第三方库通过https://deno.land/x查找



---

权限

--allow-env，允许访问环境变量，可指定环境变量列表，通过逗号分隔

--allow-hrtime，允许高分辨率时间测量

--allow-net，允许网络访问，可指定网络地址列表，通过逗号分隔

--allow-ffi，允许加载动态库，动态库不是安全运行的

--allow-read，允许读取，可指定读取文件列表或者目录，使用逗号分隔

--allow-run，允许运行子进程，这个子进程不是安全运行的，可指定子进程列表，通过逗号分隔

--allow-write，允许写入，可指定写入文件列表或者目录，使用逗号分隔

-A 或者 --allow-all 允许全部权限




---

VsCode插件deno


---

deno内置工具

安装模块

deno install --allow-net -n httpreq https://deno.land/x/dfetch/mod.ts

卸载模块

deno uninstall httpreq

格式化（支持js，ts，jsx，tsx，md，json等格式）

格式化当前目录和子目录全部支持文件
deno fmt

格式化指定文件
deno fmt 1.ts 2.ts

格式化指定支持文件夹
deno fmt dist/

检查是否已格式化
deno fmt --check

如果需要忽略格式化，只需要在文件头部设置注释// deno-fmt-ignore-file


打包
deno bundle https://deno.land/x/dfetch/mod.ts httpreq.ts

编译成执行文件（支持交叉编译，支持win64，macos64，macosarm，Linux64，通过--target属性指定）
deno compile --allow-net --target aarch64-apple-darwin index.ts 

支持的值有x86_64-unknown-linux-gnu, x86_64-pc-windows-msvc, x86_64-apple-darwin, aarch64-apple-darwin


检查依赖关系

deno info .\index.ts

如果需要查看依赖缓存位置的信息，可以直接执行deno info命令来查看

规范检查
deno lint .\index.ts

检查json
deno lint --json

忽略规范检查请在文件头部设置注释，// deno-lint-ignore-file

下载全部远程依赖项到本地文件夹中（会放到vendor文件下，其中import_map.json是依赖地址文件，deno.land文件夹是依赖项的存储文件夹，可以在deno.json下指定import_map.json或者deno run --no-remote --import-map=vendor/import_map.json index.ts来离线使用模块（也不使用模块））

deno vendor index.ts


任务（deno.json）

    {
        "tasks": {
            "run": "deno run --allow-net index.ts "
        }
    }

查看已定义任务
deno task

执行指定任务
deno task run

也可以指定任务的工作目录
deno task --cwd .\src run


---
