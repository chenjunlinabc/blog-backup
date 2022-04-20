---
title: "简单利用Electron框架开发桌面端应用"
categories: [ "默认" ]
tags: [ "Electron" ]
draft: false
slug: "91"
date: "2021-08-25 12:00:00"
---

Electron是由github开发的开源框架，允许开发者使用web技术构建跨平台的桌面应用


GUI由Chromium提供，底层通过Nodejs提供，Native API提供桌面端和跨平台的原生功能


Visual Studio Code和Atom都是使用Electron开发的，可以说技术很成熟


但是毕竟是基于Chromium的，打包出来的应用非常大，就算是个halloword，也要几百m


安装Electron

npm install electron --save-dev


全局安装

npm install -g electron




新建项目，并且在该目录下建立index.html文件和main.js


mian.js是Electron应用的配置文件


导入electron模块

var electron = require('electron')


创建Electron引用

var app = electron.app


创建窗口引用

var BrowserWindow = electron.BrowserWindow

声明主窗口

var mainWindow = null


设置参数

    app.on('ready',()=>{
        mainWindow = new BrowserWindow({
            // 设置窗口大小
            width:500,
            height:500,
            webPreferences:{
                nodeIntegration: true
                // 是否集成node
            }
        })   
        mainWindow.loadFile('index.html')  // 指定窗口加载那个页面
        mainWindow.on('closed',()=>{
            mainWindow = null
            // 监听销毁事件，事件触发关闭主窗口，设置为null
        })
    })


初始化

npm init --yes


启动项目

electron .


安装打包应用依赖

npm install electron-packager --save-dev


修改package.json文件

"packager": "electron-packager ./ halloword --platform=all --out=./App"


./是项目路径，hallword是项目名字，--platform=all是构建x86和x64构架，也可以指定win32或者win64


--out=./App是打包应用到哪个文件夹中

启动打包应用

npm run-script packager


加密app文件夹，避免程序暴露

安装asar

npm install asar -g

在项目中安装

npm install asar --save-dev

启动加密

asar pack ./app app.asar

