---
title: "简单使用Corepack-包管理器的管理器"
categories: [ "技术" ]
tags: [ "node.js" ]
draft: false
slug: "149"
date: "2022-05-13 00:09:00"
---

Corepack（管理yarn和pnpm的包管理器的管理器）

corepack是nodejs官方内置的CLI，nodejs16.9.0版本及其以上版本默认携带corepack一起分发（不需要额外安装corepack，16.9.0版本之下的需要手动安装或者更新nodejs版本）

corepack的特点就是不需要安装yarn和pnpm（yarn和pnpm将通过corepack来进行管理安装以及使用），而且还可以限制项目使用特定的包管理器版本（避免一个项目用多个包管理器，而且包管理器的版本还不同的情况）

手动安装

    npm install -g corepack

如果全局已经安装了yarn或者pnpm的话，需要先卸载

    npm uninstall -g yarn pnpm

启用corepack

    corepack enable

限制包管理器版本

package.json

    "packageManager": "yarn@1.22.19"


表示该项目只能使用yarn包管理器的1.22.19版本，使用其他版本或者使用pnpm包管理器的话，会报错，例如Usage Error: This project is configured to use yarn

默认无法限制npm，需要通过corepack enable npm来手动限制，移除限制通过corepack disable npm来处理

修改packageManager的值后，yarn install，会自动安装指定版本

安装包管理器（会根据package.json下的packageManager来下载指定版本的包管理器）

    yarn install

指定一个新的包管理器

    corepack prepare pnpm@7.13.5 --activate

使用本地包管理器（会将本地包管理器存储到一个压缩包里，方便离线使用）

获取

    corepack prepare --all -o=D:/demo/test.tgz

启动压缩包内的包

    corepack hydrate D:/demo/test.tgz

或者获取完成后立刻使用

    corepack hydrate D:/demo/test.tgz --activate