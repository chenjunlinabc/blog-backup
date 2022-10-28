---
title: "NextJS学习笔记"
categories: [ "学习" ]
tags: [ "React","NextJS" ]
draft: false
slug: "74"
date: "2021-08-07 23:26:00"
---

NextJS是一个用于生产环境的react框架，可以提供服务器端渲染等等功能


服务端渲染（ssr）：后端调用数据库获取数据后，将数据和页面元素进行组合成完整的DOM结构，再返回给浏览器，提供给用户浏览


SPA：

全称single page web application

单一页面，加载慢，百度目前不支持spa的SEO


NextJS：

服务端渲染，服务端和客户端数据同步，插件丰富，搭建轻量，灵活配置



手动配置：

yarn add react react-dom next

yarn init

修改package.json文件

在scripts下加添加

"dev" : "next",
"build": "next build",
"start": "next start"


创建个js文件

    function Hallo(){
        return(
            <div>hallo next.js</div>
        )
    }
    export default Hallo

yarn dev调试

如果浏览器输出正确则配置成功


通过create-next-app脚手架创建项目


npm install create-next-app -g 

或者

yarn add create-next-app -g 



创建create-next-app项目

npx create-next-app demo

或者

yarn create-next-app demo

跑create-next-app项目

yarn dev


访问http://localhost:3000/，正常显示网页则配置正常



---


编程式跳转

    import Link from "next/link"
    <Link href="/"><a>返回</a></Link>


Link不支持直接加兄弟标签，起码要有一个父级标签


利用Router实现跳转


    import Router from "next/router"
    <button onClick={()=>{Router.push("/")}}>返回</button>



路由跳转通过query传递参数和接收参数

传递参数

    <Link href="/hallo?name=hallo"><a>返回</a></Link>


接收参数

    import {withRouter} from "next/router"
    const Hallo = ({router}) =>{
        return(
            <div>
                <div>{router.query.name} word</div>
                <Link href="/"><a>返回</a><Link>
            </div>
        )
    }
    export default withRouter(Hallo)


也是可以用编程式跳转的方式

函数的方式

    function goData(){
        Router.push('/hallo?name=hallo')
    }


或者用对象的方式

    Router.push({
        pathname: "/hallo",
        query: {name: "hallo"}
    })


路由的钩子函数

routeChangeStart // 路由将发生变化的时候触发

routeChangeComplete // 路由发生变化完成

beforeHistoryChange // 在history触发前，nextjs路由变化默认是通过history进行的

正常情况下先触发routeChangeStart，然后beforeHistoryChange，再触发routeChangeComplete 

routeChangeError // 路由变化发生错误时

hashChangeStart // hash路由将发生变化的时候触发

hashChangeComplete // hash路由发生变化完成


hash路由（锚点）

    <div>
        <Link href="#hallo"><a>选hallo</a></Link>
    </div>

钩子触发调用自定义函数

    Router.push.on("routeChangeStart",(...url)=>{
        console.log("routeChangeStart触发",...url)
    })






axios从远端获取数据

    import axios from "axios"
    Hallo.getInitialProps = async() =>{
        const promise = new Promise((resolve)=>{
            axios("url").then(
                (datas)=>{
                    console.log(datas)
                    resolve(datas.data.data)
                }
            )
        })
        return await promise
    }
    export default withRouter(Hallo)



Style JSX，Next.js


    <style jsx>
        {`
            div{color: red;}
            .post{color: red;}
        `}
    </style>


使用jsx，nextjs会自动加入随机类名，防止css污染



自定义Head


    import Head from "next/head"
    ...
    ...
    <Head>
        <title>hallo word</title>
    </Head>



next打包

next build

运行

next start -p 8000

如果使用antd，打包失败，可以在page目录下建_app.js文件

    import App from 'next/app'
    import 'antd/dist/antd.css'
    export default App

然后再打包

---

页面

/pages/demo.js


    function Demo(){
        return(<h2>test</h2>)
    }
    export default Demo

Next会自动完成路由配置


自定义组件

/components/demo.js


    export default ({data})=><h2>{data}</h2>

导入组件

    import Demo from '../components/demo'
    ...
    <Demo>test</Demo>



---


搭配Axios获取远端数据（Next中提供了getInitialProps方法来获取）

/pages/demo.js

    import axios from 'axios'
    Demo.getInitialProps = async ()=>{
        const promise =new Promise((resolve)=>{
                axios('https://cjlio.com/feed/').then(
                    (res)=>{
                        console.log('远程数据结果：',res)
                        resolve(res.data.data)
                    }
                )
        })
        return await promise
    }


---

Next CSS(style jsx)


    <style jsx>
        {`
            h2{color: #000;}
        `}
    </style>


注意：使用style jsx了，Next会自动生成一个随机类名，来避免CSS的全局污染

---

LazyLoading 模块懒加载


    function MainData(){
        const Data= async ()=>{ 
            const axios = await import('axios')
            ...
        }            

当该函数触发异步加载该模块


懒加载组件

import dynamic from 'next/dynamic'
const Demo = dynamic(import('../components/Demo'))

只有当需要用到<Demo>组件才会被加载，利用的是next提供dynamic模块



---


自定义<Head>（next自身已经定义了head组件，只需要导入就是可以用了）


    import Head from 'next/head'
    const Header = ()=>{
        return (
            <>
                <Head>
                    <title> 小陈的辣鸡屋 </title>   
                </Head>
            </>
        )
    }
    export default Header


---

antd依赖

安装next css配置

NextJS使用@zeit/next-css将会警告，请卸载@zeit/next-css

Warning: Built-in CSS support is being disabled due to custom CSS configuration being detected.


antd 按需加载（babel-plugin-import）（当需要哪个组件才会加载到生产环境中，而且不是整个antd）

yarn add babel-plugin-import

根目录新建.babelrc配置

    {
        "presets":["next/babel"],
        "plugins":[     
            [
                "import",
                {
                    "libraryName":"antd",
                    "style":"css"
                }
            ]
        ]
    }



---



Turbopack是基于Rust开发的打包器，使用Rust的SWC编译器，Turbopack将作为Nextjs的开发服务端，提供强大的HMR支持，目前还在alpha测试中，可以通过yarn dev --turbo运行next13项目体验

Turbopack基于Rust开源的Turbo记忆化框架，Turbo可以缓存函数的结果（存储在内存中，只有当停止dev服务时清除缓存，但是Turbopack也提供了远程缓存来对于生产环境的使用（基于Git）），第二次运行时，不会执行函数，而是直接使用结果，除非发生改变