---
title: "简单使用ESbuild打包工具"
categories: [ "技术" ]
tags: [ "JavaScript","golang","esbuild" ]
draft: false
slug: "132"
date: "2022-02-07 13:37:00"
---

ESbuild打包器基于Golang开发，优点在于可多线程打包，直接编译成机器码，ESbuild提供的api可在JavaScript和golang使用，连Vite在很多场景都依赖了ESbuild打包，支持TypeScript和jsx


安装

npm install esbuild

或者

yarn add esbuild



打包

.\node_modules\.bin\esbuild app.jsx --outfile=build/index.js --bundle


或者package.json

"build": "esbuild app.jsx --outfile=build/index.js --bundle"

npm run build


例子（app.jsx）

    import React from 'react'
    import ReactDOM from 'react-dom'
    const App = () => {
        return (
            <div>
                <h1>Hallo, Esbuild!</h1>
            </div>
        )
    }
    ReactDOM.render(
        <App />,
        document.getElementById("app")
    )

index.html

    <div id="app"></div>
    <script src="./build/index.js"></script>

我本地打包只花64ms就打包好了
