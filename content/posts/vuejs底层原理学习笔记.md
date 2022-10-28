---
title: "vuejs底层原理学习笔记"
categories: [ "学习" ]
tags: [ "vuejs" ]
draft: false
slug: "147"
date: "2022-04-26 20:00:00"
---

组件的渲染，更新

组件的渲染：通过组件的模板创建vnode，渲染vnode，生成DOM

vue应用的初始化

    import { createApp } from 'vue'
    import App from './app'
    const app = createApp(App)
    app.mount('#app')

通过上面例子看到vue将app应用挂载到根组件上（一般是id为app的dom节点），通过createApp()函数，对外暴露vue，createApp()方法主要作用是创建app应用，以及重写mount方法，最后返回app应用

通过createApp()源码，可以看到createApp()接收一个参数，这个参数也就是app应用（根组件），createApp()创建app应用是通过ensureRenderer()的createApp()创建的

ensureRenderer()用于创建一个惰性渲染器对象（延时创建，这样的好处是当只使用响应式包时，不需要打包渲染器等渲染逻辑相关的代码）


