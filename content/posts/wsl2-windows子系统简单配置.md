---
title: "wsl2-windows子系统简单配置"
categories: [ "默认" ]
tags: [ "wsl" ]
draft: false
slug: "35"
date: "2021-06-16 17:43:00"
---

wsl全称Windows Subsystem for Linux


打开启动或关闭windows功能，选择虚拟机平台，安装完毕功能，重启


打开Windows Power Shell，输入

// 启用 wsl

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

// 启用虚拟机功能（请务必确保已经开启了虚拟机平台功能）

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