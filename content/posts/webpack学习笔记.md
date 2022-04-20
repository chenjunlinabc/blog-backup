---
title: "webpack学习笔记"
categories: [ "学习" ]
tags: [ "webpack" ]
draft: false
slug: "47"
date: "2021-06-24 10:11:00"
---

webpack作为模块加载和打包神器

安装node就有npm了，用npm安装webpack

npm i webpack -g 安装全局的webpack

到项目目录下，npm init -y 初始化模块

npm i webpack -D 安装项目局部的webpack，-D是--save-dev的意思

webpack 入口文件名 最后输出到哪个文件的文件名，例如：

webpack A.js B.js

那么就会编译A.js，输出到B.js

webpack只能处理JavaScript，处理其他类型的文件，需要安装几个包

npm install css-loader style-loader

假设有个一个abc.css文件，里面定好了样式

那么修改A.js

require("!style-loader!css-loader!./abc.css");
document.write(require("./C.js"));

再打包一次

webpack A.js B.js

会出现一个C.js

配置webpack.config.js

    module.exports = {
        entry: '入口文件的路径',
        output: {
            path:__dirname,
            filename: '最后输出到哪个文件的文件名'
        },
        module: {
            rules: [{
                test: '处理什么格式的文件',
                use:[
                  {
                    loader: '依赖包'
                  }
                ]
              }
            ]
        }
    };



---



模块化

import $ from "jquery" // 导入jQuery，只需要src当前js文件就可以导入jQuery依赖


loader加载器打包非js模块，比如：

less-loader：打包.less相关文件

sass-loader：打包.scss相关文件

url-loader：打包处理css中与url路径相关的文件


webpack打包处理过程：

先判断是否为js模块，不是就检查是否配置了对应的loader，配置了就是调用loader处理，没有配置就是报错

如果是js模块就判断是否包含了高级js语法，没有包含就直接调用webpack处理，包含了就检查是否配置了babel，配置了就是调用loader处理，没有配置就报错



加载器的使用

npm i style-loader css-loader -D // 安装处理css文件的loader

在webpack.config.js的module -> rules数组中，添加loader规则，例如：

    module: {
        rules: [
            {
                test: /\.css$/, use: ["style-loader","css-loader"]
            }
        ]
    }
   

注意：

test表示匹配的文件类型，use表示要调用的loader

use数组中指定的loader顺序是固定的，调用顺序是从后往前调用的


---

less loader

npm i less-loader less -D

test: /\.less$/, use: ["style-loader","css-loader","less-loader"]


先把less解析为css文件，然后再解析css文件


scss loader

npm i sass-loader node-sass -D


test: /\.scss$/, use: ["style-loader","css-loader","sass-loader"]


---

打包样式表的图片和字体

npm i url-loader file-loader -D

test: /\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/, use: "url-loader?limit=1024"



limit是用来指定图片的大小的，单位是字节（b），只有小于limit大小的图片，才被转换为base64图片


---

打包js高级语法

安装babel转换器相关的包

npm i babel-loader @babel/core @babel/runtime -D

安装babel语法插件相关的包

npm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-properties -D

配置babel.config.js并初始化

    module.exports = {
        presets: ["@babel/preset-env"],
        plugins: ["@babel/plugin-transform-runtime","@babel/plugin-proposal-properties"]
    }


配置webpack.config.js

test: /\.js$/, use: "babel-loader", exclude: /node_modules/


excludes是排除项，表示babel-loader不需要处理node_modules中的js文件


---

配置postCSS，自动添加css兼容性

npm i postcss-loader autoprefixer -D

配置postcss.config.js，并且初始化

    const autoprefixer = require("autoprefixer") // 导入插件

    module.exports = {
        plugins: [autoprefixer] // 加载插件
    }

配置webpack.config.js

test: /\.css$/, use: ["style-loader","css-loader","postcss-loader"]


---


导入vue单文件组件

import App from "./App.vue"

安装处理vue单文件的loader

npm i vue-loader vue-template-compile -D


添加loader配置文件

处理vue文件的loader需要VueLoaderPlugin插件

    const VueLoaderPlugin = require("vue-loader/lib/plugin");


    test: /\.vue$/, loader: "vue-loader"

    plugins: [
        new VueLoaderPlugin()
    ]

---

webpack dev-Server和proxy代理

---

loaders及plugins的处理


---

resolve/sourceMap

---

webpack性能优化（TreeShaking，热更新）



---


具体请看webpack官方文档




