---
title: "Umi框架学习笔记"
categories: [ "默认" ]
tags: [ "Umi" ]
draft: false
slug: "110"
date: "2021-09-20 21:00:00"
---

Umi是一款前端应用框架

官方文档：https://umijs.org/zh-CN/docs

根据官方文档要求，node版本>=10.13

yarn create @umijs/umi-app

安装依赖

yarn

启动项目

yarn start

构建项目（默认生成到./dist）

yarn build


路由（src\\.umi\\core\\routes.ts）

    "routes": [
        {
            "path": "/",
            "component": require('@/pages/index').default,
            "exact": true
       },{
            "path": "/admin",
            "component": require('@/pages/admin').default,
            "exact": true
        }
    ]


组件文件放在src\\pages下

path是路径，component是组件路径，绝对和相对都可以用，也可以有require('@/pages/xxx')的方式

exact表示是否严格匹配，就是path和组件路径是否要完全对应，默认为开启，如果设置为false，表示模糊匹配


子组件

    "routes": [
      {
        "path": "/",
        "redirect": '/admin',
      },{
        "path": "/admin",
        "component": require('@/pages/admin').default,
        routes: [
            { path: '/admin/archives', redirect: '/' },
            { path: '/admin/category', component: 'category' },
        ]
      }
    ]


redirect是跳转路由，当访问/的时候，跳转到/admin


---


文件路由（根据目录和文件名来分析路由）

如果没有routes路由配置，那么就会触发该文件路由，通过分析src/pages目录


注意：用.或者_开头的文件，用d.ts结尾的文件，不是 .js，.jsx，.ts或tsx文件，文件没有jsx元素的等等都不会被注册为路由


动态路由

使用[]包裹的路由将认为动态路由，例如：

    "routes": [
        {
            "path": "/",
            "component": require('@/pages/index').default,
            "exact": true
       },{
            "path": "/:data/admin",
            "component": require('@/pages/[data]/admin').default,
            "exact": true
        }
    ]


像[ $]的就是可选的，不是必须的


当目录下存在_layout.tsx的时候将生成嵌套路由


约定src/pages/404.tsx是404页面，要返回react组件，会默认生成路由，{ component: '@/pages/404' }


页面跳转一样分为声明式和命令式

和在react一样，通过to属性指定路径，例如：

    import { Link } from 'umi'
    export default () => (
        <Link to="/admin">Go</Link>
    )


命令式


    import { history } from 'umi'
    function goPage() {
        history.push('/list')
    }


也是可以通过props.history.push()实现


umi约定如果存在src/pages/document.ejs文件，那么作为默认的模板

umi约定/mock文件夹下全部文件都为mock文件

支持数组和对象

例如：


    export default {
        'GET /api/name': { name: ["root", "admin"] },
        '/api/id': { id: 123 },
        'POST /api/users/create': (req, res) => {
            res.setHeader('Access-Control-Allow-Origin', '*')
            res.end('ok')
        }
    }


访问http://localhost:8000/api/name，可以获得{"name":["root","admin"]}，访问/api/id可以{"id":123}

使用mock: false禁用当前mock文件


.env文件为环境变量配置文件，例如

PORT=3000

那么就会以3000端口启动服务


---

###umi cli

umi build

umi dev

umi generate

umi plugin

umi help

umi version

umi webpack



---

umi约定src/global.css为全局样式，如果存在这个文件，那么会自动引入


css模块化

import styles from './styles.css'


umi内置less


---

引入图片

\<img src={require('./img.jpg')} />

也是可以用@来指向src目录
























