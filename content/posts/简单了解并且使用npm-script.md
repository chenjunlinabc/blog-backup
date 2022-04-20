---
title: "简单了解并且使用npm script"
categories: [ "默认" ]
tags: [ "npm" ]
draft: false
slug: "109"
date: "2021-09-20 14:30:00"
---

npm script是package.json中可以定义的脚本命令，可以用来实现自动化构建，例如：

    "scripts": {
        "dev": "node hallo.js"
      }


npm run dev // 等于执行node hallo.js


查看当前项目的全部npm脚本

npm run


注意：当前项目的node_modules/bin下的全部依赖都可以直接访问


如果要执行多个脚本可以用&&（依次运行），&（并行运行）


npm script有pre和post两个钩子，这两个钩子可以分别来做准备工作和清理工作等等，例如：

    "scripts": {
        "predev": "echo hallo",
        "dev": "node hallo.js",
        "postdev": "echo yes"
    }

相对于npm run predev && npm run dev && npm run postdev


像install，uninstall，publish，test，start等等都有pre和post这两个钩子


查看正在运行的脚本

const NpmScript = process.env.npm_lifecycle_event
console.log(NpmScript)


可以缩写不用run，例如：npm dev


npm script可以使用npm内部变量，例如：

    {
        "name": "root", 
        "scripts": {
            "dev": "node hallo.js $npm_package_name"
        }
    }

获取npm内部变量name

console.log(process.env.npm_package_name)


脚本错误抛出

    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    }


传递参数

    {
        "scripts": {
            "dev": "node hallo.js"
        }
    }


const name = process.env.npm_config_name
console.log(name)


npm run dev --name=root

可以看到获取到了参数name的值