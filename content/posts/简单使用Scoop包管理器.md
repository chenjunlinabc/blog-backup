---
title: "简单使用Scoop包管理器"
categories: [ "技术" ]
tags: [ "scoop","windows" ]
draft: false
slug: "150"
date: "2022-05-20 22:10:00"
---

Scoop是windows平台下开源的命令行软件包管理器，类似于ubuntu的apt或者macOS的brew

scoop仓库里面全部都是通过审核的绿色软件包（前提是不要乱用来路不明的scoop源）

允许执行本地脚本权限

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser


修改Scoop安装目录（用户级）

$env:SCOOP='D:\Software\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')

修改Scoop安装目录（全局）

$env:SCOOP_GLOBAL='D:\Software\Scoop\Global'
[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'Machine')

目录介绍：
apps：通过scoop安装的软件存储的目录

buckets：管理软件的仓库目录（记录了哪些仓库有哪些软件）

chache：软件安装包目录

persit：存储用户数据，与软件分离

shims；软链接


安装scoop

iwr -useb get.scoop.sh | iex

或者

iex (new-object net.webclient).downloadstring('https://get.scoop.sh')

安装软件

scoop install sudo

查看环境存在的问题

scoop checkup

将一些scoop环境必须的软件安装一下

搜索软件

scoop search git

安装软件

scoop install git

查看软件信息

scoop info git

查看已经安装的软件

scoop list

卸载软件（-p删除配置）

scoop uninstall git -p

更新软件

scoop update git

更新全部已安装软件

scoop update *

导出已安装软件列表

scoop.cmd export > App_list.txt

添加官方的bucket（包含大量的软件）

scoop bucket add main 
scoop bucket add extras 

更新仓库

scoop updat

限制软件更新

scoop hold git


代理

scoop config proxy 127.0.0.1:7890

取消代理

scoop config proxy none


查看其他scoop命令

scoop help




