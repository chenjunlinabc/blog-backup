---
title: "Svelte学习笔记"
categories: [ "默认" ]
tags: [ "Sveltejs" ]
draft: false
slug: "80"
date: "2021-08-09 22:50:00"
---

Svelte的核心思想在于通过静态编译减少框架运行时的代码量

Svelte风格和vue相似，模板用{}来表示

Svelte的特点就是没有虚拟DOM（像Vue和React都是用虚拟DOM的）


初始化

npx degit sveltejs/template demo

yarn

yarn dev



修改src\App.svelte和src\main.js


---


    <script>
        let name = 'world'
    </script>
    <h1>Hello {name}!</h1>


导入组件

    import Hallo from './hallo.svelte'
    ...
    <Hallo/>


如果想将HTML渲染到组件中可以使用
    <script>
        let string = `<div>hallo word</div>`
    </script>
    <p>{@html string}</p>


事件响应

    function aClick() {
        count += 1
    }
    ...
    <button on:click={aClick}>{count}</button>