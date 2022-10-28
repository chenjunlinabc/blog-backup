---
title: "NodeJS常用工具的简单使用"
categories: [ "默认" ]
tags: [ "nodejs","nvm","nrm","pm2","n","pnpm" ]
draft: false
slug: "105"
date: "2021-09-16 21:55:00"
---

npm在这里https://cjlio.com/archives/31.html

Yarn在这里https://cjlio.com/archives/38.html



---



nvm全名node.js version management

用来nodejs多版本管理，可以切换和安装不同版本的nodejs



安装nvm之前记得把安装过的nodejs都卸载了


安装完成后，安装目录下会生成一个settings.txt文件

配置一下（如果要使用淘宝npm源的话）

root: D:\\Software\\nvm
path: D:\\Software\\nodejs
arch: 64
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/



环境变量

NVM_HOME设置为nvm安装目录

NVM_SYMLINK设置为nodejs安装目录

Path：添加%NVM_HOME%和%NVM_SYMLINK%




注意：安装路径不能出现中文或者空格，否则报错，请使用管理员权限运行use命令，否则可能导致exit status 1:xxx




安装指定版本nodejs

nvm install 14.17.3

查看可以安装的nodejs版本

nvm ls available


查看当前全部已经安装的nodejs版本

nvm ls

切换nodejs版本

nvm use 14.17.3

卸载nodejs

nvm uninstall 14.17.3


查看系统位数和node位数

nvm arch



---

另一个nodejs多版本管理工具n

安装n

npm install -g n


安装最新nodejs版本

n latest

安装稳定版本nodejs

n stable

安装lts版本

n lts


安装指定版本的nodejs（如果已安装了这个版本，那么就会选择这个版本，这个命令可以用来安装和选择）

n 14.17.3

卸载指定版本

n rm 14.17.3


指定版本执行js

n use 14.17.3 app.js


切换版本（n回车后，通过上下键来选择版本）

n


查看网络上可以安装的nodejs版本

n ls-remote --all


删除除当前版本外的全部版本

n prune



---




nrm镜像源管理工具

安装nrm

npm install -g nrm

查看全部源

nrm ls

带*的是当前源

切换源

nrm use taobao

添加自定义源（腾讯npm）

nrm add tencentnpm https://mirrors.cloud.tencent.com/npm/

删除源

nrm del taobao

测试源速度

nrm test npm



---


PM2是node进程管理工具，可以用来监控性能，进程守护，负载均衡等等

安装pm2

npm install -g pm2

或者

yarn add global pm2


启动应用（--watch是当文件发生变化自动重启，-i 3是3个app.js的应用实例，-name是命名应用）

pm2 start app.js --watch -i 3 -name="main"


执行app.js文件并且监听app.js的变化，-i为进程数，max表示当前cpu可启动的最大进程

pm2 start app.js –watch -i max –ignore-watch=“node_modules” –name demo

–ignore-watch=“node_modules"为忽略监听指定的目录或者文件，这里忽略的是node_modules文件夹

–name为进程名字

pm2执行npm run dev

pm2 start npm –watch – run dev

pm2执行npm run start

pm2 start npm –name demo – run start



停止应用

pm2 stop app.js


重启应用

pm2 reload app.js


删除应用

pm2 delete app.js

也可以用all来处理全部应用



pm2不只是只能启动nodejs，Python或者shell都可以

查看PM2中的进程信息

pm2 list

查看指定进程id的信息

pm2 show 0

重启指定进程id的进程

pm2 restart 0

重启全部进程

pm2 restart all

停止指定进程id的进程

pm2 stop 0

删除指定进程id的进程

pm2 delete 0



显示全部应用日志

pm2 logs

查看指定应用的日志

pm2 logs main

清空所有日志文件

pm2 flush



更新pm2

npm install pm2@latest -g

pm2内存更新

pm2 update

查看pm2运行中的全部应用

pm2 list

查看各个应用内存占用和CPU情况

pm2 monit

查看指定应用的全部信息

pm2 show main






---



pnpm

pnpm是Node.js包管理工具，解决了npm/yarn的一些BUG

特点是：包安装速度极快，磁盘空间利用高效，支持monorepo

和yarn一样，也是通过缓存来保存安装过的包，使用pnpm-lock.yaml记录依赖包版本

pnpm是利用软链接（快捷方式）来实现依赖结构扁平化，从而解决了从缓存中拷贝文件的时间

更安全：没有在package.json声明，将不能使用依赖，而且不存在依赖提升（一个包依赖于另一个包，而且那个包不需要声明，就可以使用）


安装

npm install -g pnpm


安装包

pnpm install axios

升级

pnpm add -g pnpm

卸载

pnpm uninstall axios --filter package-a