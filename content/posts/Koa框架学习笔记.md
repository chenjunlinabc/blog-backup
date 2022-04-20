---
title: "Koa框架学习笔记"
categories: [ "默认" ]
tags: [ "koa" ]
draft: false
slug: "100"
date: "2021-09-12 18:30:00"
---

koa是web应用框架，是Express原班人马打造的，致力于成为一个更小、更富有表现力、更健壮的 Web 框架

个人推荐：个人开发推荐用koa，团队开发推荐用egg

egg是在koa的基础上进行封装，并且提供了一些，并且添加了约束，更利于工程化的开发

安装koa

npm install koa


新建一个app.js


    const Koa = require("koa")
    const app = new Koa()
    app.use(async (ctx, next) => {
        await next()
        ctx.response.type = 'text/html'
        ctx.response.body = '<h1>hallo, koa!</h1>'
    })
    app.listen(3000)


node app.js



get请求参数的接收


    const Koa = require('koa')
    const app = new Koa()
    app.use(async(ctx)=>{
        const url =ctx.url
        const request =ctx.request
        const reqQuery = request.query
        const reqQuerystring = request.querystring
        ctx.body={
            url,
            reqQuery,
            reqQuerystring
        }
    })
    app.listen(3000,()=>{
        console.log('port 3000')
    })

http://127.0.0.1:3000/admin?name=admin&pass=123

通过ctx.request来获取得到get请求参数（query）


还可以通过ctx上下文获取


    app.use(async(ctx)=>{
        const url =ctx.url
        const ctxQuery = ctx.query
        const ctxQuerystring = ctx.querystring
        ctx.body={
            url,
            ctxQuery,
            ctxQuerystring
        }
    })



Post请求接收





Koa默认返回类型是text/plain，判断返回类型ctx.request.accepts()

一般来说，返回给用户的页面都是写成文件的，因此可以通过nodejs的fs来读取本地的文件



    const fs = require("fs")
    app.use(async (ctx, next) => {
        await next()
        ctx.response.type = 'text/html'
        ctx.response.body = 'fs.createReadStream('./test.html')'
    })



路由


判断当前路由地址

ctx.request.path


路由模块

const route = require('koa-route')

app.ues(route.get("/", main))



静态资源管理

const path = require('path')
const serve = require('koa-static')
const main = serve(path.join(__dirname))
app.use(main)




重定向

ctx.response.redirect()可以实现302重定向，例如：

    const redirect = ctx => {
        ctx.response.redirect('/')
        ctx.response.body = '<a href="/">index</a>'
    }
    app.use(route.get('/auth, redirect));




中间件

中间件实质上就是在请求和响应之间，实现某个中间功能，而app.use()就是用来加载中间件的

这个中间件默认支持两个参数（ctx和next）

多个中间件，按照先进后出，next可以传递，await next就是异步中间件


合成中间件

const compose = require('koa-compose')
const c = compose([a, b])
app.use(c)



抛出错误

    const main = ctx =>{
        ctx.response.status = 404
        ctx.response.body = '404找不到资源';
    }


注意：发生错误，koa会触发一个error事件，因此也可以监听这个事件，来抛出错误



读写cookies

ctx.cookies.get()

