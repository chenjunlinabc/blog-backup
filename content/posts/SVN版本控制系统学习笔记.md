---
title: "SVN版本控制系统学习笔记"
categories: [ "默认" ]
tags: [ "SVN" ]
draft: false
slug: "84"
date: "2021-08-11 15:00:00"
---

SVN全名Subversion，即版本控制系统

和Git不同，Git是分布式，而SVN是集中式


在commit没有冲突的时候，会合并

而存在冲突那么就需要先手动处理冲突再commit


源代码库：源代码统一存放的地方

获取：到源代码库获取一份源代码

提交：已经修改了源代码，想提交到源代码库

更新：更新同步和源代码库一样的源代码




安装SVN


dnf install subversion


创建版本库

cd /svn/demo

svnadmin create /svn/demo

其中conf/svnserve.conf文件是svn服务配置文件


anon-access = read
auth-access = write
password-db = passwd
authz-db = authz
realm = /svn/demo


passwd文件为账号密码文件

格式为：账号=密码，例如：abc = root 

authz为权限控制文件

格式为：账号=rw（r读，w写），例如：abc=rw


启动版本库

svnserve -d -r /svn/demo --listen-port 3690

--listen-port是指定svn监听端口，默认是3690，-r是指定版本库

停止svn服务

killall svnserve

检出：从版本库中创建副本，在其修改，再提交到版本库中


例如：

svn checkout svn:xxx/demo --username=root

因为root有读写权限，因此将会在本地获取到demo的副本

 

更新：更新副本，将其同步到版本库最新版本，如果不是当前最新版本，当前本地的副本将无效

svn update

默认是更新到最新版本，也可以指定更新到哪个版本

svn update -r 2 demo



提交

svn commit -m "hello svn"


---


查看副本状态

svn status

如果出现?，则表示还没有加入版本控制中，出现A，表示已经加入版本控制中


添加文件到版本控制中

svn add demo


add只是将其添加到版本控制中，还没有提交到版本库中

提交到版本库中

svn commit -m "hello svn"



版本回退（当想放弃当前文件的修改，想恢复到之前的文件）

svn revert -R demo

将demo目录回退到修改之前的状态，不加-R则是文件回退

当然也可以回退到指定版本

svn revert -R  2:1 demo

从版本2回退到版本1



查看历史信息

查看指定版本之间的信息（版本作者，日期，路径）

svn log -r 2:1

查看文件的版本修改信息

svn log /demo/index.html

获取目录和显示指定条数的

svn log -l 2 -v


查看历史修改情况（比如本地修改，副本和版本库）

svn diff

默认会比较副本文件和缓存在.svn中的原始的改变


比较副本和指定版本

svn diff -r 2 index.html


版本和版本的比较

svn diff -r 1:2 index.html


查看过去版本的内容

svn cat -r 2 index.html


查看远程库的文件列表（不下载）

svn list svn:xxx/demo





---




分支


创建分支



切换分支



合并分支


---


标签





















