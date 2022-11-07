---
title: "Linux环境下简单搭建Minecraft服务器（java版）"
categories: [ "技术" ]
tags: [ "server" ]
draft: false
slug: "33"
date: "2021-06-16 15:46:00"
---

服务器可以使用国内的，保证延迟低，服务器配置一定要高一点，不然很容易Killed

安装java

dnf install java-openjdk

检测java是否安装成功

java -version


新建一个目录

mkdir hallomc

cd hallomc

下载第三方mc服务器

wget -c https://papermc.io/api/v2/projects/paper/versions/1.16.5/builds/553/downloads/paper-1.16.5-553.jar

这是1.16.5的版本，服务器版本和客户端版本要一致

历史版本https://papermc.io/legacy

运行mc服务器

java -Xmx1024M -Xms512M -jar paper-1.16.5-553.jar

Xmx 代表服务器启动所占的最大运行内存，Xms代表服务器正常运行的最大内存

一般来说第一次运行都是运行不了，因为没有同意协议

进入mc目录下，nano eula.txt，把eula=false改成eula=true，然后再运行mc服务器


24小时运行mc服务端

一般来说退出ssh登录，就会终止运行mc服务端，可以通过简单建立个“虚拟终端”，来24小时运行

dnf install screen  # 安装screen

screen -S mcserver # 创建一个新“终端”，名称自定义

screen -R mcserver # 进入这个新“终端”

如果想退出，可以使用ctrl+a+d


可以使用screen -ls 命令来查看所有“终端”


关闭正版验证

在服务端目录，找到server.properties文件

修改这个文件，把onlinemode 改为 false 




---


MCSManager面板

wget -qO- https://gitee.com/mcsmanager/script/raw/master/setup.sh | bash