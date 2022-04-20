---
title: "简单使用Express-Web应用框架"
categories: [ "默认" ]
tags: [ "Express" ]
draft: false
slug: "138"
date: "2022-02-28 18:04:00"
---

Express是基于nodejs的web应用框架（同时也是node的第三库），同时也是很多web应用框架的底层库


安装

npm install express --save

或者安装express-generator脚手架

npm install -g express-generator

脚手架：

初始化项目（demo是项目名）

express demo

安装依赖

npm install

运行

npm start

如果不使用脚手架（main.js）

    const express = require("express")
    const app = express()
    app.get('/',function(req,res){ 
        res.end("hallo world!")
    })
    app.listen(3000)

运行

node main.js

或者（监视nodejs应用中的任何更改并自动重启服务）

nodemon main.js

访问localhost:3000


静态文件管理（必须通过/src才能访问src文件夹的静态文件）

app.use('/src',express.static('src'))


解决跨域问题（依赖于cors模块）

app.use(require('cors')())


Express连接MongoDB（mongoose）

npm install mongoose

    const mongoose = require('mongoose')
    mongoose.connect('mongodb://localhost:27014/test',{useNewUrlParser: true})
    const testdb = mongoose.model('testdb',new mongoose.Schema({
        _id: Number,
        title: String
    }))
    /*testdb.inserMany([
        {_id: 1, title: "abc"},
        {_id: 2, title: "xyz"}
        {_id: 3, title: "abcxyz"}
    ])*/
    app.get('/test',async function(req,res){
        res.send(await testdb.find())
    })


查询MongoDB数据

    app.get('/test',async function(req,res){
        // const data = await testdb.find().skip(1).limit(2)
        /* const data = await testdb.find().where({
           title: 'abc'
        */
        const data = await testdb.find().sort({
            _id: -1
        })
        res.send(data)
    })

skip(1)跳过多少条，limit(2)显示多少条，where()可指定查询某个字段为某个值的数据，sort()表示排序的顺序（1为正序，-1为倒序）

    app.get('/test/:id',async function(req,res){
        const data = await testdb.findById(req.params.id)
        res.send(data)
    })

访问localhost:3000/test/2

注意：req和res的区别，req是客户端请求，res是服务端响应


插入数据到MongoDB中

    app.use(express.json()) // 通过express.json()中间件，解析body中的json数据
    app.post('/admin' async function(req,res){
        const data = req.body // 获取前端post请求的body数据
        const dataMain = await testdb.create(data) // body数据create到MongoDB数据库中
        res.send(dataMain)
    })


修改MongoDB数据

    app.put('/admin/:id' async function(req,res){
        const data = await testdb.findById(req.params.id) // 寻找目标id
        data.title = req.body.title // 将请求body的title赋值到数据库中的目标id的title
        await data.save() // 保存到数据库中
        res.send(data)
    })


删除MongoDB数据

    app.delete('/admin/:id' async function(req,res){
        const data = await testdb.findById(req.params.id) // 寻找目标id
        await data.remove() // 从数据库中删除该数据
        res.send({
            code: 200
        })
    })





















