---
title: "简单搭建一个hexo博客"
categories: [ "默认" ]
tags: [ "hexo" ]
draft: false
slug: "26"
date: "2021-06-16 14:36:00"
---

hexo+github可以实现免费搭建一个博客网站，就是维护起来有点麻烦


apt install npm

npm install hexo-cli -g

hexo init blog

cd blog

npm install

hexo server

使用NexT

apt install git

git clone https://github.com/theme-next/hexo-theme-next.git

把hexo-theme-next文件夹放到hexo根目录的themes文件夹下

启动主题

打开hexo目录下的_config.yml文件，这个文件为站点配置文件

找到theme，修改值为hexo-theme-next

调试模式

hexo s --debug
查看是否输出有错误，方便修改错误信息

选择主题

打开主题的根目录的_config.yml文件，这个文件是主题配置文件

找到scheme，需要启动的在前面去掉#注释即可，不需要就加注释#

设置 语言

打开站点配置文件，修改language为需要的语言zh-CN

修改菜单

打开主题配置文件，找到menu，需要用到的菜单就去掉#，不需要就加#

home 主页
archives 归档页
categories 分类页
tags 标签页
about 关于页面
commonweal公益 404

修改对应的语言翻译

主题目录下languages/zh-CN.yml

设定菜单项的图标，可以使用的是Font Awesome 图标

设置 侧栏

打开主题配置文件

修改sidebar.position的值

left - 靠左放置
right - 靠右放置

设置 头像

打开主题配置文件，修改avatar值设置成头像的链接地址

可以是url，也可以站点内的地址

设置 作者昵称

打开站点配置文件，设置 author 为作者的昵称

站点描述

打开站点配置文件

设置 description 字段为你的站点描述

百度统计

打开主题配置文件

修改字段 baidu_analytics 字段，值设置成百度统计 id

添加RSS功能

npm install hexo-generator-feed –save

编辑主题配置文件，修改rss，rss: /atom.xml

标签 页面

hexo new page tags

type: "tags"

分类 页面

hexo new page categories

type: "categories"

设置代码高亮主题

打开主题配置文件

修改highlight_theme

可选的值有 normal，night， night blue， night bright， night eighties

侧边栏社交链接

social

格式是 显示文本: 链接地址

social_icons

格式是 匹配键: Font Awesome 图标名称

友情链接

打开主题配置文件

links

404页面

新建 404.html 页面，放到主题的 source 目录下

推荐使用腾讯公益404页面

站点建立时间

打开主题配置文件，since

来必力

打开主题配置文件，livere_uid: LiveRe UID

Google 分析

打开主题配置文件， 修改字段 google_analytics，
值设置成 Google 跟踪 ID。跟踪 ID 通常是以 UA- 开头。

腾讯分析

打开主题配置文件 里将 ID 放置 tencent_analytics

CNZZ 统计

在主题配置文件中添加cnzz_siteid的配置项，值为 CNZZ 里面添加统计的站点ID

百度分享

打开主题配置文件
添加/修改字段 baidushare，值为 true

搜索服务

如何设置 阅读全文

在文章中使用 <!-- more --> 手动进行截断，Hexo 提供的方式

打开主题配置文件，添加：
auto_excerpt:
enable: true
length: 150

部署到Github
git config --global user.name "chenjunlinabc"
git config --global user.email "a@cjlio.com"

ssh-keygen -t rsa -C "a@cjlio.com"

检查是否连接上github

ssh -T git@github.com

设置deploy参数

打开主题配置文件

deploy:
type: git
repo: git@github.com:chenjunlinabc/chenjunlinabc.github.io.git
branch: main

github的分支默认为main

安装一个插件
npm install hexo-deployer-git --save

hexo clean 清理缓存

hexo g 进行渲染

部署到git服务器，hexo d


---

关于每次hexo d后，GitHub Pages自定义域名都会失效的解决方法

在source目录下添加一个文件CNAME就好

该文件放域名