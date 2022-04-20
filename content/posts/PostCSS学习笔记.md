---
title: "PostCSS学习笔记"
categories: [ "默认" ]
tags: [ "PostCSS" ]
draft: false
slug: "129"
date: "2022-01-23 13:55:00"
---

PostCSS是一个用JS插件转换为css的插件工具（注意：PostCSS不是css预处理器，PostCSS本身是个平台，可以通过一些插件达到css预处理器的效果）

PostCSS is a tool for transforming CSS with JS plugins. These plugins can support variables and mixins, transpile future CSS syntax, inline images, and more.

插件查询：https://www.postcss.parts/

常用插件：https://github.com/postcss/postcss/blob/main/docs/plugins.md

安装

npm install postcss postcss-loader 

或者安装到项目中

npm install postcss postcss-loader --save-dev

PostCSS不单独使用，可搭配Gulp或者webpack使用（这里使用的是webpack）

webpack.config.js

    module.exports = {
        module: {
            rules: [
                {
                    test: /\.css$/,
                    use: ["style-loader", "css-loader", "postcss-loader"]
                }
            ]
        }
    };

postcss.config.js


    module.exports = {
        plugins: [插件1,插件2]
    };



Autopreﬁxer（自动添加浏览器前缀）

npm install autoprefixer --save-dev

    const autoprefixer = require("autoprefixer") // 导入插件
    module.exports = {
        plugins: [autoprefixer] // 加载插件
    }

执行打包命令npm run build 


postcss-preset-env（支持现代css语法）

npm install postcss-preset-env --save-dev

const postcssPresetEnv = require("postcss-preset-env")

css-modules（css模块化）

npm install postcss-modules --save-dev

const postcssModules = require("postcss-modules")

stylelint（css代码检查）

npm install stylelint stylelint-config-standard --save-dev

const StyleLintPlugin = require("stylelint-webpack-plugin")