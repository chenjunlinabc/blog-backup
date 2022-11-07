---
title: "WebSocket学习笔记"
categories: [ "学习" ]
tags: [ "WebSocket" ]
draft: false
slug: "158"
date: "2022-11-05 14:00:00"
---

HTTP协议实质上就是半双工信道，无法同时发送数据和接收数据，而且HTTP连接必须是客户端发起，由服务器来进行处理响应

WebSocket是全双工信道，而且还支持服务端主动发送数据给客户端，是服务器推送技术（还是需要客户端发起连接）

WebSocket协议是应用层协议，而且是建立在TCP协议上，端口也是使用443和80，握手使用HTTP协议，浏览器不会限制WebSocket的同源


WebSocket客户端配置

WebSocket构造函数，用来创建WebSocket实例

const ws = new WebSocket('ws://127.0.0.1')

WebSocket.readyState实例具备4种状态，该属性是只读的，用来表示连接WebSocket服务端的状态，分别是：0（正在连接），1（连接完成并且可以通信），2（连接正在关闭），3（连接已经关闭或者连接失败）

WebSocket.onopen是指定连接成功后执行的回调函数

WebSocket.onerror是指定连接失败后执行的回调函数

WebSocket.onclose是指定连接关闭后执行的回调函数

WebSocket.onmessage是指定从服务器获取信息时执行的回调函数

可以指定WebSocket.binaryType来指定传输的数据类型，数据类型有2种，分别是blob和arraybuffer


客户端配置例如：

    const ws = new WebSocket('ws://localhost:8080')
    ws.onopen = () =>{
        console.log("连接中")
        ws.send('hallo word') // 向服务端发送数据
    }
    ws.onerror = () =>{
        console.log('连接失败')
    }
    ws.onmessage = (evt) =>{
        console.log('连接成功，正在获取数据')
        if(typeof evt.data === String){
            console.log('hallo'+evt.data)
        }else if(evt.data instanceof ArrayBuffer){
            let data = evt.data
            console.log('数据：'+data)
        }
        ws.close()  // 手动关闭连接
    }
    ws.onclose = () =>{
        console.log('连接已关闭')
    }


还有WebSocket.bufferedAmount属性，也是只读，用于返回WebSocket..send没有发送到服务端的数据的字节数，为0表示全部数据已传输完毕

WebSocket.url属性可以返回WebSocket实例的URL绝对路径，只读

WebSocket.protocol属性可以返回服务端选中的子协议名字，只读

WebSocket.extensions属性可以返回服务端已选择的扩展值，只读


---


WebSocket服务端实现（node的ws模块）

安装ws模块

npm install ws

server.js

    const WebSocket = require('ws') // 导入模块
    const WebSocketServer = WebSocket.Server
    const wsmain =new WebSocketServer({
        port: 8080
    })
    wsmain.on('connection',function(ws){  
        console.log('客户端已连接')
        ws.on('message',function(message){
            console.log(`客户端发送的数据:${message}`)
            ws.send('欢迎连接该ws服务端')
            ws.send(message)
        })
        
    })


node server.js


这样服务端可以接收到客户端发送的数据，服务端也可以主动给连接中的客户端发送数据


除了ws模块外，还有nodejs-websocket，Socket.IO，µWebSockets


nodejs-websocket搭建服务端

安装

npm install nodejs-websocket


server.js

    const ws = require("nodejs-websocket")
    const server = ws.createServer(function (ws) {
        ws.on("text", function (str) {
            console.log(str)
            ws.sendText('hallo word')
        })
        ws.on("close", function (code, reason) {
            console.log("客户端已经关闭连接")
        })
    }).listen(8080)

app.js


    const ws = new WebSocket('ws://localhost:8080')
    ws.onopen = () =>{
        console.log("连接中")
        ws.send('hallo word') // 向服务端发送数据
        const obj = {
            name: 'root',
            age: 20,
            pass: '123456',
            email: 'a@cjlio.com',
            key: 'hallo word'
        }
        const data = JSON.stringify(obj)
        ws.send(data) // 发送json数据
    }
    ws.onerror = () =>{
        console.log('连接失败')
    }
    ws.onmessage = (message) =>{
        console.log('连接成功，正在获取数据')
        const isJson = (str) => {
            try{
                if(typeof JSON.parse(str) == 'object'){
                    return true
                }
            }catch (e){

            }
            return false
        } 
        if(isJson(message.data)){
            console.log(JSON.parse(message.data)) 
        }else{
            console.log(message.data)
        }
    }
    ws.onclose = () =>{
        console.log('连接已关闭')
    }


Socket.IO用法

安装

npm install socket.io