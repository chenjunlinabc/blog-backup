---
title: "node.js包管理工具npm的简单使用"
categories: [ "默认" ]
tags: [ "node.js","npm" ]
draft: false
slug: "31"
date: "2021-06-16 15:05:00"
---

npm是Node.js的默认包管理工具

安装npm：安装node.js(一般来说安装nodejs都会安装npm的)

npm -v 查看npm版本号
node -v 查看node版本号
npm install nmp@latest -g 更新最新nmp，-g全局，没有加-g就是本地安装，或者在@后面加版本号来更新到指定版本的npm
npm init -y 初始化
npm i 要安装的依赖 先检查有没有这个东西，有的话就下载下来
npm uninstall 要删除的依赖的名称
npm i 要安装的依赖@版本号 安装指定版本的依赖
npm update 依赖名称 安装最新的依赖或者更新npm
npm init -y 使用默认的参数，去掉-y就是手动配置
npm run 对象名 执行脚本,引用package.json中的scripts对象，在对象中添加脚本
npm adduser 注册npm账号
npm publish 发布npm包
npm install 一键安装package.json文件里的所有依赖
npm install --dependencies 只安装package.json里的dependencies的文件
npm install --devDependencies 只安装package.json里的devDependencies文件
会自动将package.json中的模块安装到node-modules文件夹

升级插件 npm-check-updates使用
npm install -g npm-check-updates 安装npm-check-updates插件
ncu 查看package.json中依赖的最新版本
ncu -u 更新依赖到最新版本
ncu -a 更新全部依赖到最新版本
npm update

该升级插件和npm自带的差别在于npm自带的只能通过package.json文件中所标注的版本号更新，要手动更改，而该升级插件会自动更新package.json文件中所标注的版本号

安装淘宝 NPM 镜像
npm install -g cnpm --registry=https://registry.npm.taobao.org

全局设置淘宝 NPM 镜像
npm config set registry https://registry.npm.taobao.org -g

使用方法
cnpm install 包名

package.json文件是用来定义包的属性
name 是包名
version 是包的版本号
description 是包的描述
homepage 是包的官网的url
author 是包的作者名称
contributors 是包的其他贡献者名称
dependencies 是依赖包的列表，有依赖的名称和依赖的版本号，npm会自动将依赖安装到node_module目录下
repository 包代码存放的地方的类型，git或者svn
main 指定了程序的主入口文件
keywords 关键字



---

启动项目

npm run dev

npm run serve



---

用npm安装的包都在package.json中声明了，在另一个环境下，直接npm install，就可以一键安装该项目的全部包，不用一个个安装了

npm install执行流程，找到npmrc文件，得到包从哪里下，下到哪里的数据，然后再判断是否存在package-lock.json文件，存在则和package.json判断版本声明是否一致（从缓存，网络资源加载依赖），一致则按照package-lock.json处理，不一致则按照package.json处理，并且更新package-lock.json

如果没有package-lock.json文件则检查是否存在缓存，如果有缓存则解压到node_modules中，并且生成package-lock.json文件，如果没有缓存则从npm远程仓库获取包，检查包的完整性，然后添加到缓存，构建依赖树，并且解压到node_modules中，然后再生成package-lock.json文件

npmrc文件优先级：项目的npmrc文件>用户npmrc文件>全局npmrc文件>npm内置的npmrc文件

npm安装会先根据npmrc文件选择npm远程仓库

项目npmrc文件在当前项目的目录

用户npmrc文件可通过执行npm config get userconfig查看目录

全局npmrc文件（公共npmrc）可通过npm config get prefix查看目录

例如在项目根目录下创建.npmrc文件，并且设置registry=https://registry.npm.taobao.org

npmrc文件的配置是通过键值对的形式的，registry为key，https://registry.npm.taobao.org为value

设置用户npmrc文件

可以直接修改用户npmrc文件或者执行npm config set registry https://registry.npm.taobao.org

设置全局npmrc文件

可以直接修改全局npmrc文件或者执行config set registry https://registry.npm.taobao.org -g

快速修改用户npmrc文件，npm config edit

快速修改全局npmrc文件，npm config edit -g

删除registry（npmrc的其他配置也可以修改，npm config delete key）

npm config delete registry

查看npmrc详细配置

npm config list [-l]

查看npmrc具体参数配置（npmrc的其他配置也可以查看，npm get registry key）

npm get registry registry

查看全局环境包目录，npm root -g


npm优先安装依赖到当前项目目录的node_modules目录中，也就是说同一依赖可能在计算机上多次安装，当然也可以使用参数-g全局安装


npm缓存

查看npm缓存目录，npm config get cache

清除缓存，npm cache clean --force

下载包时会先下载到缓存中，使用pacote包解压到node_modules目录中

其中pacote包又依赖于npm-registry-fetch来下载包，并且根据IETF RFC 7234生成缓存

在下载包时，根据package-lock.json存储的integrity，version，name信息生成唯一的key

如果存在缓存资源，则寻找其tar包的hash，通过pacote包将其解压到node_modules目录中


npm init可生成一个初始化的package.json，可通过js脚本来自定义npm init

npm-init.js

    module.exports = {
        name: prompt('name?''halloword'),
        version: prompt('version?''0.0.1')
    }

npm config set init-module ~\.npm-init.js


npm link（方便本地开发包，以及调试测试，假设npmModulea目录是一个包项目目录）

先进入包项目目录（npmModulea目录），执行npm link，执行该命令会让npmModulea包被链接到全局node_modules目录中（软链接）

在包项目目录（npmModulea目录）执行npm link npmModulea，该命令可以让npmModulea包链接到当前项目的node_modules目录中


npx可以直接执行node_modules目录的.bin目录下的文件，还可以在执行命令时自动检查命令是否存在

而且npx在执行模块时，会优先安装模块的依赖，在执行之后会删除这些依赖，避免全局安装模块带了的问题


部署私有npm镜像，nexus（java环境），verdaccio（Python环境），cnpm（node环境）


cnpm私有npm镜像仓库：https://github.com/cnpm/cnpmjs.org


简化依赖树，npm dedupe，避免几个包依赖另一个同版本包时，存在多个依赖的情况（yarn会自动执行该命令，自动扁平化依赖树）


npm ci要求项目存在package-lock.json或者npm-shrinkwrap.json，而且npm ci比npm install安装更严格，其完全根据package-lock.json安装依赖（安装依赖时不会修改package-lock.json和package.json），当package-lock.json中的依赖于package.json不一致时，报错退出并且不会修改package-lock.json

注意：package-lock.json和npm-shrinkwrap.json同时存在项目目录时，package-lock.json会被忽略

npm ci安装依赖前，会先删除原来项目的node_modules文件夹，然后全新安装，而且是一次安装项目的全部依赖，不能安装单个依赖，所以其不需要去校验已下载依赖版本与控制版本的关系，也不用校验是否存在最新版本的库，下载的速度会更快


package-lock.json可以优化依赖的安装速度，package-lock.json已经保存了每个包的具体版本和下载链接，不需要再向远程仓库进行查询，获取到依赖后可直接进入文件完整性校验环节（integrity）

而且package-lock.json可以锁定其依赖结构，保证执行npm install都得到相同的node_modules

package-lock.json保存其生成的依赖树是一致的，而package.json只能得到自己安装的依赖，但是其依赖的子依赖并没有记录，子依赖可能会发生改变或者更新了版本，从而导致其依赖树不一致


package-lock.json中的xxxdependencies

dependencies表示项目依赖（会自动下载），devDependencies表示开发依赖（不会自动下载），peerDependencies表示同版本依赖（父依赖，宿主依赖），bundledDependencies表示捆绑依赖，optionalDependencies表示可选依赖


注意：peerDependencies指定的依赖会进行安装，在引用这个依赖时，会先引用宿主依赖，bundledDependencies中指定的依赖，需要先在dependencies和devDependencies声明过，否则会报错，optionalDependencies中指定的依赖哪怕安装失败了，也不会导致安装失败

peerDependencies其中的依赖就是好吧antd依赖于react和react-dom一样，是宿主依赖，表示你安装我时，最好安装这些依赖
