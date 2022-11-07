---
title: "简单使用Vite-前端构建工具"
categories: [ "技术" ]
tags: [ "Vite" ]
draft: false
slug: "87"
date: "2021-08-18 14:20:00"
---

Vite是由 Vue.js 的作者尤雨溪开发完成的一款前端项目构建工具，使用原生ESM文件，支持热重载

Vite在法语中的意思为快速的

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

npm create vite

或者

yarn create vite

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


npm create vite

选择react，如果需要ts，也可以选择ts

npm install




---






Vite是一个构建工具的高阶封装（生产环境打包用Rollup来构建），开发环境无需要打包编译（利用现代浏览器的原生ESM来进行导入模块，模块加载的工作由浏览器完成，实现冷启动），支持动态模块热重载（HMR），开发环境的编译通过Esbuild来完成


Vite生产环境构建（也就是Rollup）是根据browserslist来做浏览器兼容性的，需要通过修改vite.config.ts下的build.target来指定构建目标，Vite最低支持es2015，也就是es6，需要es5以及更多浏览器可能无法正常运行或者通过@vitejs/plugin-legacy插件来获取支持（注意：使用该插件也不能让vue3支持ie11，只有vue2支持ie11）

安装@vitejs/plugin-legacy

    yarn add @vitejs/plugin-legacy -d

配置vite.config.ts

    import legacy from '@vitejs/plugin-legacy'
    ...
    plugins: [
        legacy([
            targets: ['defaults','no IE 11']
        ])
    ]
    ...
    build: {
        target: 'es2015'
    }



浏览器原生ESM导入

index.html
 
    <div id='app'></div>
    <script type='module' src='app.js'></script>

app.js

    import { createApp } from 'vue'
    import App form './App.vue'
    import './index.css'
    createApp(App).mount("#app")



浏览器解析index.html的时候，通过\<script type='module' src='app.js'>得到app.js文件，而且声明该文件使用的ESM模块化的机制，然后对app.js导入的模块进行实时编译，实现按需加载


因为其是利用了原生ESM模块化，因此Vite原生支持css，不需要额外配置，通过插入style元素来实现

main.js

    import './index.css'


使用less或者sass，postcss都很简单，Vite内置了配置，无需配置Vite

安装less

    yarn add less -d

安装sass

    yarn add sass -d

安装postcss

    yarn add postcss -D

postcss是通过插件完成功能的，因此还得根据需要来安装插件（注意：postcss.config.js配置文件导入插件，不需要使用require()导入，而是直接写入字符串）


使用typescript（vite默认支持typescript，但是只进行ts编译，不会进行校验）

App.vue

    import { test } './index'


让vite支持校验typescript

安装typescript

yarn add typescript -d



修改package.json命令

    "scripts": {
        "build": "tsc --noEmit && vite build"
    }


添加tsconfig.json配置

    {
        "compilerOptions": {
            "target": "esnext",
            "module": "esnext",
            "moduleResolution": "node",
            "strict": true,
            "jsx": "preserve",
            "sourceMap": true,
            "resolveJsonModule": true,
            "esModuleInterop": true,
            "lib": ["esnext","dom"],
            "isolatedModules": true,
        },
        "include": [
            "src/**/*.ts",
            "src/**/*.d.ts",
            "src/**/*.tsx",
            "src/**/*.vue",
        ],
    }


注意：isolatedModules属性为true时，ts文件中如果不存在import和export时，ts会认定该模块不是ESM模块，设置的原因是Babel在转义阶段，会将ts的类型声明删除，这个时候如果export类型出现，Babel是不知道有这个类型存在的（因为js没有类型），导致前端报错，开启这个功能可以在开发阶段就提示错误（）

校验.vue文件（vue-tsc）

安装vue-tsc

    yarn add vue-tsc -d


修改package.json命令


    "scripts": {
        "build": "vue-tsc --noEmit && tsc --noEmit && vite build"
    }


---


vite集成eslint和pritter

安装依赖

npm install eslint-config-standard eslint-plugin-import eslint-plugin-promise eslint-plugin-node -d

.eslintrc.js，使用standard规则

    module.exports = {
        extends: 'standard',
    }

.pritterrc（搭配vsc插件pritter使用，prettier用来格式化）

    {
        'semi': false,
        'singleQuote': true
    }

vsc打开设置，搜索format on save，构选，再搜索formatter，选择prettier插件，保存时将会使用prettier进行代码格式化（让代码符合eslint规则）



---


vite的HMR热更新API

import.meta可以返回当前模块的信息，需要在模块内部使用，由ECMAScript实现这个功能

    <script type='module'>
        console.log(import.meta)
    </script>

可以看到其返回的是一个对象，该对象有个url属性，这个url属性指向了模块的url，这个对象可以进行扩展

vite通过扩展该对象，import.meta.hot手动暴露HMR（获取新的模块），例如：

    if(import.meta.hot){
        import.meta.hot.accept((newMod) =>{
            newMod.Dom()
        })
    }

原理是服务端发现模块更新了，发送一个事件给客户端，客户端知道模块更新了，就通过发送请求获取最新的文件，将老的模块替换成新的，就实现热更新功能了



vite批量导入

    const mods = import.meta.glob('./src/*')
    console.log(mods)

可以看到输出了一个键值对，键为文件名，值为该文件的import


    Object.entries(mods).forEach([a,b]) =>{
        b().then(mod => {
            console.log(mod.default)
        })
    }




预编译（运行vite项目，会在node_modules下创建一个.vite文件夹，这个文件夹是vite的依赖，直接读取这个依赖就可以了）

预编译的另一个作用是可以将多个依赖文件整合成一个文件，可以通过optimizeDeps，exclude禁止预编译某个依赖来查看

















