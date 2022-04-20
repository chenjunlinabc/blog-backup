---
title: "Node.js学习笔记"
categories: [ "默认" ]
tags: [ "node.js" ]
draft: false
slug: "108"
date: "2021-09-20 10:50:00"
---

nodejs是基于Google的V8引擎，使JavaScript不再只能运行与浏览器中了

npm是跟随nodejs的包管理工具，可以用来更新包，安装包，编写包等等功能

nodejs也提供了完整的http服务功能（http模块是用c++写的，性能可靠）


    const http = require("http")
    http.createServer(function (_request, response) {
        response.writeHead(200, {'Content-Type': 'text/plain'})
        response.end("hallo nodejs")
    }).listen(8888)
    console.log('Server running at http://127.0.0.1:8888/')


如果在http://127.0.0.1:8888/看到了hallo nodejs，那么就说明运行成功了


nodejs的模块分为全局模块，系统模块，自定义模块


全局模块（不需要引入，可以直接使用）

例如process.env和process.argv

获取系统环境变量

    console.log(process.env)



node自定义参数（process.argv）

    let num1 = parseInt(process.argv[2])
    let num2 = parseInt(process.argv[3])
    console.log(num1+num2)


node hallo.js 6 3

可以看到输出了9，说明参数被传递进去了



系统模块（需要引用，不需要安装，nodejs已经封装好的预制的系统模块）

获取目录

    const path = require("path")
    console.log(path.dirname("/hallo/index/main.js"))

获取文件名

    console.log(path.basename("/hallo/index/main.js"))


获取文件扩展名

    console.log(path.extname("/hallo/index/main.js"))

功能扩展

    console.log(path.resolve("/hallo/index/main.js","../","index.js"))

获取文件的绝对路径

    console.log(path.resolve(__dirname,"index.js"))


文件读写模块（fs）

读

    const fs = require("fs")
    fs.readFile("data.txt",(err,data)=>{
        if(err){
            console.log(err)
        }else{
            console.log(data.toString())
        }
    })


    
写

    fs.writeFile("data.txt","hallo nodejs",(err,data)=>{
        if(err){
            throw err
        }
    })


追加（在不更改原来文件内容的前提下，在后面追加内容）

    fs.appendFile("data.txt","hallo xxx" , (err)  => {
        if (err){
            return err.message
        }else{
            console.log("成功")
        }
    })

因为上面都是异步操作的，也是有同步操作的，例如writeFileSync，不过同步就不能用回调函数了，因为异步防堵塞用的就是回调函数




自定义模块

require（引入模块）

如果没有路径，就去node_modules目录查找，如果有路径，就去当前路径查找被暴露的模块


exports（暴露模块）

module（暴露扩展，可以使用函数或者class）

    module.exports = function(){
        console.log("hallo word")
    }




http模块


    const http = require("http")
    const fs = require("fs")
    http.createServer((request,response)=>{
        fs.readFile(`./${request.url}`,(err,data)=>{
            if(err){
                response.writeHead(404)
                response.end("404")
            }else{
                response.writeHead(200)
                response.end(data)
            }
         })
        
        response.end()
    }).listen(8888)



---



require模块加载机制：计算模块路径，被加载过的模块会被缓存起来，当第二次引用时直接从缓存中获取，加载模块，输出模块的exports属性

模块查找：优先从缓存中加载，如果没有再到核心模块（例如：fs和http）找，路径化的文件模块，第三方模块（当前项目的根目录的node_modules/example)，如果还是没有就是去上一级找node_modules/example，到磁盘根目录还是没有就返回错误


注意：如果package.json不存在或者main指定的入口模块也不存在，那么node就会自动查找该目录下index.js


导出模块有俩种方法，分别是exports.hallo=hallo和module.exports={}

像exports.hallo=hallo就是利用exports添加属性，但是不能对exports直接进行赋值

注意：exports !== module.exports


多线程

node中一个cluster模块，该模块被用于集群负载均衡，而cluster的创建进程的方法是利用了child_process的fork来新建子进程，子进程再操作文件



