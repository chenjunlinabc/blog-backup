---
title: "简单使用Hugo搭建网站"
categories: [ "技术" ]
tags: [ "golang","Hugo" ]
draft: false
slug: "137"
date: "2022-02-22 05:19:00"
---

Hugo是基于Go语言开发的静态网站生成器，特点就是快



安装

二进制文件安装（由官方编译完成的二进制文件来安装，推荐使用，用源码容易出现问题）

https://github.com/gohugoio/hugo/releases


源码安装

git clone https://github.com/gohugoio/hugo.git

cd hugo

go install

检查是否安装完成 hugo -v，如果需要支持SASS/SCSS，请添加--tags extended参数，不过在这之前需要CGO的依赖（或者使用hugo_extended版本）

如果没安装CGO，请先安装CGO，这里使用的是mingw64，CGO_ENABLED环境变量为1


生成站点

 hugo new site ./www


创建文章（默认自动生成md文件到content文件夹中，可选择目录）

hugo new post/hallo.md

如果没有显示文章的话，请将文章的draft字段改为false，因为这个是草稿，草稿是不会显示在页面上的


安装主题

git clone https://github.com/miiiku/hugo-theme-kagome.git ./themes/kagome


修改config.toml文件



baseURL = 'https://blog.xiaochenabc123.test.com'
languageCode = 'zh-CN'
title = '小陈的博客'
theme = "kagome"


启动Hugo服务器

hugo server

访问http://localhost:1313


如果报错，you need the extended version to build SCSS/SASS的话，请使用extended版本


部署到github pages

hugo

如果该命令执行成功，会将静态页面生成到public文件夹中，只需要push该文件夹到github上就好了



