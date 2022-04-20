---
title: "Yarn包管理工具的简单使用"
categories: [ "默认" ]
tags: [ "Yarn" ]
draft: false
slug: "38"
date: "2021-06-16 22:55:00"
---

Yarn 是一款新的 JavaScript 包管理工具，和npm对比就是速度快，保持一致性，安全

速度快是因为yarn是并行执行任务，而不是npm那样排队列执行package，而且yarn还可以提供缓存，如果安装过一次package，使用yarn再次安装就会从缓存中获取，而不用再下载一次

保持一致性：yarn提供了一个lockfile文件来记录要安装的package的版本号，锁定其版本不会出现错误，会生成yarn.lock文件来记录其package的版本号，就连依赖包的版本号都会被记录

安全：yarn会在每个package被执行时校验其完整性

实质上yarn本身还是从npm中获取的CLI客户端，还是一样可以获取和发布包

windows不允许禁止运行脚本解决方法，管理员打开powershell

set-ExecutionPolicy RemoteSigned


安装yarn（全局）

npm install yarn -g

查看全部yarn命令

yarn help


检查是否安装成功以及查看版本号

yarn --version


同样也是可以选择升级到yarn2

yarn版本在v1.22之上
yarn set version berry

yarn版本在v1.22之下
yarn policies set-version berry

初始化

yarn init 


安装一个包

yarn install

yarn




添加一个包到依赖中

yarn add 包名@版本号

如果没有写明版本号，默认安装的是最新的，支持一次性填加多个包，多个包用空格分开

添加一个包到不同的依赖类别中

开发环境
yarn add 包名 --dev

生产环境
yarn add 包名 --peer


全局依赖环境
yarn global add 包名


更新包到指定版本

yarn upgrade 包名@版本号


更新包到最新版本

yarn upgrade --latest 包名


删除包

yarn remove 包名

支持一次性删除多个包

管理依赖缓存

缓存目录
yarn cache dir


已缓存的依赖信息
yran cache list

强制清除本地缓存的依赖
yarn cache clean

管理依赖配置信息

输出yarn和npm路径信息
yarn config list

切换镜像源
yarn config set registry https://registry.npm.taobao.org -g

查看某个依赖的具体信息
yarn info vue

搜索项目的某个依赖信息

yarn why 包名

yarn run 自定义命令

自定义命令在package.json中定义


因为其会生成一个yarn.lock文件，只要保存这个文件，可以在其根目录下执行yarn就会直接下载yarn.lock文件所记录的包

更新yarn本体

最新的发布版本
yarn set version latest

从master分支获取
yarn set version from sources

从其他分支获取
yarn set version from sources --branch 1211


发布包

先去注册npm账号，https://www.npmjs.com/signup

第一次发布需要npm adduser一下，需要提供用户名，密码，邮箱，要求源必须是npm官方源

然后yarn poblish



---

启动项目，在项目根目录下执行（反正执行速度比npm快）

yarn run dev




