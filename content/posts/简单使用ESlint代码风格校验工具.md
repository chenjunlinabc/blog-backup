---
title: "简单使用ESlint代码风格校验工具"
categories: [ "默认" ]
tags: [ "ESlint" ]
draft: false
slug: "111"
date: "2021-09-23 10:06:00"
---

ESlint是用来校验JavaScript代码风格格式的工具，目的是确保每个人的代码风格统一，按照统一的规范编写（规范化、标准化是前端工程化的特点）


安装ESlint

npm install eslint --save-dev

或者全局安装

npm install eslint --global

修改scripts属性（package.json）--fix参数是ESlint提供的自动修复基础错误功能（不能修复逻辑性错误），如果不要也可以

 "lint": "eslint src --fix",
"lint:create": "eslint --init"


创建.eslintrc

npm run lint:create


会显示显示要求，例如是否校验ES6语法，首行空白是Tab键还是Space等等


校验程序（根据上面的修改，会检查src目录下的所有.js文件）

npm run lint


.eslintrc文件是ESlint校验配置文件，这个配置文件可以自己设置（或者手写手动修改），也可以复制别人的


"off" or 0 ：关闭规则

"warn" or 1 ：将规则视为一个警告

"error" or 2 ：将规则视为一个错误



可以设置规范，只能使用单引号，tab缩进等等编写规范










