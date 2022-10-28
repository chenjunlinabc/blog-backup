---
title: "gulp学习笔记"
categories: [ "学习" ]
tags: [ "Gulp" ]
draft: false
slug: "73"
date: "2021-08-06 20:52:00"
---

gulp.js是一个基于流(stream)的自动化构建工具，是开源的JavaScript自动化工具，基于node.js和npm的构建工具，可以处理压缩代码，合并代码，压缩图片，编译less等等任务


全局安装gulp

npm install --global gulp-cli

或者

yarn add gulp-cli -g


初始化

npm init


作为开发依赖进行安装gulp

npm install --save-dev gulp

或者

yarn add gulp-cli -d


安装依赖

yarn add --save-dev gulp-sass gulp-autoprefixer browser-sync gulp-notify



创建gulpfile.js

执行gulp

gulp


修改gulpfile.js

导入gulp（这里用个变量接收）

let gulp = require("gulp");



常用方法

gulp.task() // 定义任务

gulp.src() // 指向需要执行任务的文件

gulp.dest() // 执行完任务的文件最后在哪里

gulp.watch() // 监测文件是否发生变化




gulp.task()其中有两个参数，分别是任务名称和一个回调函数


例如：

    let gulp = require("gulp");
    gulp.task("test", function() {
        return console.log("hallo gulp");
    });

执行test任务（在终端，应用根目录下）

gulp test


默认任务

    gulp.task("default", function(){
        return console.log("hallo gulp");
    })


拷贝任务

    gulp.task("copyData", function(){
        gulp.src("src/*").pipe(gulp.dest("app"));
    })

上面是将src文件夹下全部文件拷贝到app文件下，哪怕没有这个文件夹，gulp也会自动新建文件夹并且拷贝过去


图片压缩任务

安装依赖

npm install --save-dev gulp-imagemin

修改gulpfile.js

    let imagemin = require("gulp-imagemin");
    gulp.task("imageMin", function(){
        gulp.src("src/*.{png,jpg,gif,ico}").pipe(imagemin(
            {
                optimizationLevel: 5,
                progressive: true, 
                interlaced: true, 
                multipass: true,
                svgoPlugins:[{removeViewBox: true}],
            }
        )).pipe(gulp.dest("src/images"));
    })

imagemin方法参数：

optimizationLevel：压缩等级（0-7），默认值为3

progressive：是否无损压缩jpg图片，默认值为false

interlaced：隔行扫描gif渲染，默认值为false

multipass：多次优化svg，直到优化完毕，默认值为false

svgoPlugins: 移除svg的viewbox属性




---



PostCSS是一个用JavaScript转换css的工具

一般来说PostCSS不单独使用，会搭配构建工具（例如：Webpack，Gulp）使用，因此这里使用的是Gulp


安装PostCSS依赖和gulp依赖

 yarn add gulp gulp-postcss postcss-cssnext postcss-short



修改gulpfile.js


    const gulp = require("gulp");
    const postcss = require("gulp-postcss");
    const cssnext = require("postcss-cssnext");
    const shortcss = require("postcss-short");
    gulp.task("postcss", function() {
        const processors = [
            shortcss,
            cssnext
        ];
        return gulp.src("./1.css")
                .pipe(postcss(processors))
                .pipe(gulp.dest("./css"));
    });


gulp postcss

执行完毕后自动添加css兼容前缀了，还支持下一代css的语法