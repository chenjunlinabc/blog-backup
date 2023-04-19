---
title: "简单使用Rollup模块打包工具"
categories: [ "技术" ]
tags: [ "Rollup" ]
draft: false
slug: "119"
date: "2021-11-10 16:22:00"
---

根据Rollup官方文档的介绍：Rollup 是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码，例如 library 或应用程序。

Rollup和webpack那种偏向于应用打包不同，Rollup更专注于类库打包，像vue，react都是通过rollup打包的

注意：webpack支持HMR（热模块更新），而Rollup并不支持，因此在应用打包的时候还是选择webpack比较好，但是如果只是打包类库之类的，并且还是基于ES6模块开发的，那么就可以考虑选择rollup了，因为rollup在Tree-shaking和ES6模块有着算法优势。因为Rollup对模块化是使用新标准，例如 CommonJS，而不是老旧解决方案

提示：webpack已经支持Tree-shaking，并且在babel-loader的情况下也支持es6 module的打包

Rollup是ESM模块标准构建打包工具，源必须使用ESM模块标准，如果需要使用其他标准可通过插件完成

安装rollup

npm install rollup

查看帮助文档

rollup --help

打包使用

    rollup -i index.js

默认输出到终端

指定输出到哪个文件

     rollup -i index.js --file dist.js

还可以指定输出模块标准是哪个

     rollup -i index.js --file dist.js --format umd

     rollup -i index.js --file dist.js --format cjs

     rollup -i index.js --file dist.js --format iife


打包

rollup src/demo.js -f cjs -o dist/bundle.js

注意：-f是--format的缩写，表示生成代码的格式，例如amd，cjs，es，umd，iife

使用UMD格式需要指明一个name属性，用来挂载模块到全局环境中

rollup src/demo.js -f umd -o dist/bundle.js --name hallo

在global下声明一个名为hallo的对象，用来挂载全部的export模块

一次打包多个文件

rollup -i index.js -i main.js --dir dist --format cjs

如果想监听文件是否改动，可以使用-w参数（--watch），当文件发生改动的时候，重新打包



配置文件（rollup.config.js）

    export default {
        input: ["./src/demo.js"],
        output: {
            file: "./dist/bundle.js",
            format: "cjs",
            name: "experience",
        },
    }

如果配置文件想使用module.exports = {}的方式，需要将配置文件修改为.cjs文件

执行命令

rollup -c rollup.config.js

或者

rollup --config rollup.config.js


打包阶段传递环境变量

rollup.config.js获取设置的变量
console.log(process.env.DEV)

执行时设置环境变量
rollup -c rollup.config.js --environment DEV:1

这个可以通过环境变量的不同，来做不同的处理，而不需要写多套config配置了


Rollup的Tree-shaking功能，只对使用的模块进行打包，对于没有使用的模块是不进行打包的



---



Rollup常用插件（vite使用Rollup作为打包工具，因此vite也可以使用Rollup插件）

@rollup/plugin-json，打包json文件成js代码

安装

yarn add @rollup/plugin-json

使用

yarn rollup -c rollup.config.js --plugin json

如果不想在命令行设置要使用的插件，也可以在rollup.config.js下设置

    import json form rollup/plugin-json
    export default {
        input: ["./src/demo.js"],
        output: {
            file: "./dist/bundle.js",
            format: "cjs",
            name: "experience",
            plugins: [],
            banner: '/**hallo word**/',
        },
        externals: [
            'react'
        ]
        plugins:[
            json(),
        ]
    }

可以看到Rollup插件是函数，并且是顺序执行插件

externals可以指定不打包的模块

banner是指定打包完成文件头部的注释，会和rollup-plugin-terser插件冲突，代码压缩会删除全部注释

也可以通过设置npm script脚本来简化命令

    'scripts':{
        'build': 'rollup -c rollup.config.js'
    }

执行npm run build或者yarn build都可以执行


@rollup/plugin-babel，将es6文件转换为es5文件

@rollup/plugin-alias，设置模块别名，不需要写很长很长的路径，例如：

    plugins: [
        alias({
            entries: [
                { find: 'main', replacement: '../src/main' },
                { find: 'test', replacement: './test' }
            ]
        })
    ]

index.js

    import main from 'main'
    import test from 'test'


@rollup/plugin-commonjs，让其支持CommonJS模块标准打包

@rollup/plugin-vue，打包.vue文件，vue2和vue3使用的插件版本不同，vue3使用的是@rollup/plugin-vue6.0.0版本以上，而vue2使用@rollup/plugin-vue5.1.9版本，而且还需要搭配vue编译器使用，vue3使用@vue/compiler-sfc，vue2使用vue-template-compiler

@rollup/plugin-node-resolve，允许打包第三方模块，这个插件需要搭配@rollup/plugin-commonjs使用，因为这个插件底层实现是使用了CommonJS模块标准



@rollup/plugin-typescript，增加typescript支持，TypeScript版本要求3.7以及以上

@rollup/plugin-image，支持图片加载依赖，图片会被编码为base64格式（体积会增加）

rollup-plugin-terser，代码压缩插件，应该在打包完成后执行该插件，因此应该在output内部的plugins设置，老插件，这个插件没有设置default，import使用时需要{terser}

@rollup/plugin-postcss，让其支持使用postcss以及postcss插件

@rollup/plugin-serve，启动本地服务器

@rollup/plugin-livereload，文件发生变化，进行实时刷新

@rollup/plugin-eslint，打包时校验语法是否符合规范

@rollup/plugin-copy，打包时删除debugger语句和函数，例如console.log

@rollup/plugin-visualizer，加载WebAssembly模块