---
title: "简单使用Pm2---node进程管理工具"
date: 2022-04-19T20:39:27+08:00
---











安装pm2

npm install pm2 -g



执行app.js文件并且监听app.js的变化，-i为进程数，max表示当前cpu可启动的最大进程

pm2 start app.js --watch -i max --ignore-watch="node_modules" --name demo



--ignore-watch="node_modules"为忽略监听指定的目录或者文件，这里忽略的是node_modules文件夹

--name为进程名字





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



查看全部进程的日志

pm2 logs



清空所有日志文件

pm2 flush

