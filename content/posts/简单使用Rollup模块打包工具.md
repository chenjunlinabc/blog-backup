---
title: "简单使用Rollup模块打包工具"
categories: [ "默认" ]
tags: [ "Rollup" ]
draft: false
slug: "119"
date: "2021-11-10 16:22:00"
---

根据Rollup官方文档的介绍：Rollup 是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码，例如 library 或应用程序。

Rollup和webpack那种偏向于应用打包不同，Rollup更专注于类库打包，像vue，react都是通过rollup打包的

注意：webpack支持HMR（热模块更新），而Rollup并不支持，因此在应用打包的时候还是选择webpack比较好，但是如果只是打包类库之类的，并且还是基于ES6模块开发的，那么就可以考虑选择rollup了，因为rollup在Tree-shaking和ES6模块有着算法优势。因为Rollup对模块化是使用新标准，例如 CommonJS，而不是老旧解决方案

提示：webpack已经支持Tree-shaking，并且在babel-loader的情况下也支持es6 module的打包


安装rollup

npm install rollup -g


打包

rollup src/demo.js -f cjs -o dist/bundle.js

注意：-f是--format的缩写，表示生成代码的格式，例如amd，cjs，esm，umd

如果想监听文件是否改动，可以使用-w参数，当文件发送改动的时候，重新打包

配置文件（rollup.config.js）

    export default {
        input: ["./src/demo.js"],
        output: {
            file: "./dist/bundle.js",
            format: "cjs",
            name: "experience",
        },
    }

执行命令

rollup -c


---



Rollup常用插件

@rollup/plugin-babel

@rollup/plugin-json

@rollup/plugin-alias

@rollup/plugin-commonjs

@rollup/plugin-node-resolve

@rollup/plugin-typescript

@rollup/plugin-image

rollup-plugin-terser

rollup-plugin-postcss

rollup-plugin-serve

rollup-plugin-livereload

rollup-plugin-copy

rollup-plugin-visualizer