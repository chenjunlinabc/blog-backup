---
title: "简单使用Vite-前端构建工具"
categories: [ "默认" ]
tags: [ "Vite" ]
draft: false
slug: "87"
date: "2021-08-18 14:20:00"
---

Vite是由 Vue.js 的作者尤雨溪开发完成的一款前端项目构建工具，使用原生ESM文件，支持热重载

Vite在法语中的意思为快速的（尤大是真喜欢法语啊）

基于原生 import 的，使用浏览器来解析import，服务端按需编译返回，支持热更新模块

依赖于Rollup打包，虽然vite设计初衷是为了vue3.x的，但是也是支持其他框架（例如react）

对于TypeScrip的支持程度相当好，只需要在script元素加lang="ts"就可以使用ts了

至于less，sass/scss之类的css预处理器以及css原生支持也是很好，Vite支持css样式直接引入（import './app.css'）

sass/scss使用（前提已经安装了sass）

在style元素中加lang="scss"就可以使用sass了

json也是可以直接引入，例如：import data from './data.json'


另外对于JSX也是支持的，用.jsx表示jsx，例如：import App from './App.jsx'


vite有一套开发服务功能，基于原生es模块，ESM+HMR，而且还有一套项目构建指令，用rollup打包

打包：指的是使用工具来抓取和处理源码模块，并且合成可以在浏览器上运行的文件（浏览器本身并不提供模块管理的机制，多模块需要很多的script标签，很繁琐臃肿，而打包就很好的解决了这个问题）

常见的打包工具例如：webpack，rollup，parcel，gulp


注意：当冷启动服务的时候，必须要先抓取并且构建应用（当应用越来越大，模块越来越多的时候，会导致服务启动缓慢），才能提供服务，而且进行模块更新的话，也会导致重建应用缓慢


传统打包，是将多个模块打包成单一的文件，而esm打包，是根据http请求，来获取相应的route，再根据route来获取module（避免一开始就获取全部module）

vite将模块分为依赖和源码，依赖指的是开发时不会发生改变的，vite将使用esbuild预构建依赖，而且将以原生ESM方式让浏览器接管打包源码






---

构建Vite项目

npm init vite-app demo

或者

yarn create vite-app demo

然后初始化一下

npm install

或者

yarn 


启动服务器

vite

或者

npm run dev，yarn dev

本地预览

vite preview

打包

vite build



---


react


npm init vite-app --template react

npm install

