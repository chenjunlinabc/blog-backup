---
title: "wsl2-windows子系统简单配置"
categories: [ "技术" ]
tags: [ "wsl","windows" ]
draft: false
slug: "35"
date: "2021-06-16 17:43:00"
---

wsl全称Windows Subsystem for Linux


打开启动或关闭windows功能，选择虚拟机平台，安装完毕功能，重启


打开Windows Power Shell，输入

启用 wsl

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

启用虚拟机功能（请务必确保已经开启了虚拟机平台功能）

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart


下载并且安装Linux内核更新包
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi


wsl.exe --install或者wsl -l -v检查是否安装正常

切换wsl2为默认

wsl --set-default-version 2

Linux子系统切换为wsl

wsl --set-version Ubuntu 2  // Ubuntu为子系统名，2为wsl版本，可以输入wsl -l -v查看




报错 Installing, this may take a few minutes...

解决方法：打开启动或关闭windows功能，选择Linux的Windows子系统，就好了


开启wsl2不需要开启预览版本！！！


推荐个Visual Studio Code插件

Remote - WSL

可以免密登录Linux子系统，操作Linux子系统里面的数据



重启wsl

win+r+services.msc

找到Lxssmanager服务，重新启动



---




安装运行Linux系统所需要的功能（默认情况下安装ubuntu）

wsl --install

查看可用的Linux发行版

wsl --list --online

或者

wsl -l -o

安装指定发行版

wsl --install -d Ubuntu

查看本地安装发行版的版本以及状态（Running为正在运行中，为停止运行中）

wsl -l -v

停止运行（启动也很简单，重新执行Ubuntu2004，进入系统，就会启动）

wsl --shutdown

修改默认版本

wsl -s Ubuntu-20.04

卸载本地安装的发行版（全部数据都将清除）

wsl --unregister Ubuntu

更新wsl内核

wsl --update

回滚到wsl内核上一个版本

wsl --update rollback

wsl发行版会将windows认为是挂载，mnt目录就是windows的目录

导出本地发行版

wsl --export Ubuntu-20.04 d:\ubuntu20.04.tar

导入（在文件夹中看到vhdx文件，表示导入成功，导入的时候请确保本地没有同名的发行版，如果有需要卸载后导入）

wsl --import Ubuntu-20.04 d:\wsl\ubuntu d:\ubuntu20.04.tar

修改默认登陆用户

ubuntu2004 config --default-user root