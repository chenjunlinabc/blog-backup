---
title: "Nestjs学习笔记"
categories: [ "默认" ]
tags: [ "NestJS" ]
draft: false
slug: "139"
date: "2022-03-05 14:00:00"
---

NestJS是一个nodejs服务端应用开发框架，基于typescript开发，http服务框架默认为Express，也支持Fastify，支持面向对象，函数式以及函数响应式编程


安装

npm install -g @nestjs/cli

创建demo项目

nest new demo

选择使用包管理器（支持npm，yarn，pnpm）

创建完成后可以看到src目录，是典型的MVC架构

app.controller.ts（应用路由控制器）
app.controller.spec.ts（应用控制器单元测试）
app.module.ts（应用模块文件）
app.service.ts（应用服务文件）
main.ts（应用程序入口文件，实质上是async/await异步函数（bootstrap(）））

从main.ts入口文件可以看出，nest应用实例是基于NestFactory类（该类来源于@nestjs/core，nest核心程序）对外暴露的方法创建的



启动项目

npm run start

访问http://localhost:3000/，如果看到Hello World!表示启动成功


nestjs cli支持对mvc模块的生成

创建控制器
nest g controller 名称

创建服务
nest g service 名称

创建模块
nest g module 名称

创建异常过滤器
nest g filter 名称

创建拦截器
nest g interceptor 名称

创建中间件
nest g middleware 名称

创建管道 
nest g pipe 名称

创建守卫
nest g gu 名称


---


控制器（controller）

controller负责接收请求和处理响应