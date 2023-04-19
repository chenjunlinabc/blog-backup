---
title: "简单使用ESbuild打包工具"
categories: [ "技术" ]
tags: [ "JavaScript","golang","esbuild" ]
draft: false
slug: "132"
date: "2022-02-07 13:37:00"
---

ESbuild打包器基于Golang开发，优点在于可多线程打包，直接编译成机器码，ESbuild提供的api可在JavaScript和golang使用，连Vite在很多场景都依赖了ESbuild打包（viet在开发环境下使用这个），支持TypeScript和jsx（tsx），css

ESbuild支持ES6模块，cjs模块，对ES6+语法支持性好，可以直接打包css文件，json文件，ts文件

注意：esbuild并不对ts文件进行类型检查工作


安装

npm install esbuild

或者

yarn add esbuild



打包

.\node_modules\.bin\esbuild app.jsx --outfile=build/index.js --bundle

或者

npx esbuild app.jsx --outfile=build/index.js --bundle


或者package.json

"build": "esbuild app.jsx --outfile=build/index.js --bundle"

npm run build

或者

yarn build


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


使用source map功能

npx esbuild app.jsx --outfile=build/index.js --bundle --sourcemap

使用代码压缩功能--minify

npx esbuild app.jsx --outfile=build/index.js --bundle --minify

指定打包目标环境

npx esbuild app.jsx --outfile=build/index.js --bundle --target=es2020,node12

注意：如果将ES6+语法编译成ES5语法是不被支持的，但是可以将ES5语法编译成ES6+的语法或者ES6语法升级为更先进的语法，也可以将ES6+语法降到ES6语法，就是不能降到ES5（估计这是为啥vite选择这个作为开发环境打包工具，而不是生产环境打包工具的原因吧，只有开发环境下，默认认为是现代浏览器（node）环境下工作的）

指定输出目标

npx esbuild app.jsx --outfile=build/index.js --bundle --target=es2020,node12 --platform=node

--platform有3个值，分别是browser，node，neutral

默认情况下使用browser值，会将打包的代码放到IIFE（立即执行函数）中，并且对package.json依赖做出修改，会使用对目标友好的插件或者包来代替原来的

设置为node时，打包格式是cjs格式（CommonJS），会将其他风格转换为cjs格式

设置为neutral时，打包格式为esm格式

指定不进行打包构建的模块

npx esbuild app.jsx --outfile=build/index.js --bundle --platform=node --external:react

esbuild.config.js，api调用文件

    const postCssPlugin = require('@deanc/esbuild-plugin-postcss')
    const autoprefixer = require('autoprefixer')
    const esbuildConig = () => require('esbuild').buildSync({
        entryPoints: ['src/app.jsx'],
        bundle: true,
        target: ['es2020','node12'],
        external: ['react'],
        loader: {'.jpg':'dataurl','.js':'jsx'},
        platform: 'node',
        outfile: 'build/index.js',
        plugins: [
            postCssPlugin({
                plugins: [autoprefixer]
            })
        ],
    })
    esbuildConig()

通过执行node ./esbuild.config.js，来调用ESbuild API快速打包
当然也是可以使用npm script来设置

    "scripts": {
        "build": "node ./esbuild.config.js"
    }

npm run build

或者

yarn build


添加图片支持（通过转换成base64的方法来引用）

npx esbuild app.jsx --outfile=build/index.js --bundle --loader:.jpg=dataurl

添加tsx支持（可以把tsx文件转换为普通的js文件）

npx esbuild app.tsx --outfile=build/index.js --bundle --loader:.js=jsx

esbuild社区插件库：https://github.com/esbuild/community-plugins