---
title: "beego应用框架的简单使用"
categories: [ "技术" ]
tags: [ "golang","beego" ]
draft: false
slug: "134"
date: "2022-02-12 16:50:00"
---

beego是一个基于go语言开发的http框架，beego可用于开发web，api，后端服务等等应用，beego架构为mvc模型，支持RESTful api规范设计，支持热更新


安装

go get github.com/beego/beego

go get github.com/beego/bee

检查是否安装完成

bee version


beego项目可使用bee指令来创建和管理

创建第一个web应用

bee new hallo

beego是基于mvc模型的，因此其构建出来的项目文件也是标准mvc模型文件结构，其中main.go是入口文件

执行go mod tidy，生成go.sum


启动项目（bee run指令会自动编译部署）

bee run

访问http://127.0.0.1:8080/

其他常用beeg指令

创建api应用

bee api apitest


打包应用命令（将项目打包压缩）

bee pack

自动生成代码

bee generate



---

controller控制器


简单接收一下get请求的参数

controllers/default.go，在func (c *MainController) Get() 函数中修改

    name := c.GetString("name")
    c.Data["Website"] = name


访问http://127.0.0.1:8080/?name=hallo，views\index.tpl的模板中的{{.Website}}被设置为hallo

在controllers/default.go看到，其定义了一个MainController结构体，该结构体继承了beego.Controller的全部方法（其中方法包括Get，Post等等方法）


Model模型

在bee new中并没有Model实例演示，但是bee api有，而且controller控制器可完成一些简单逻辑，只有当逻辑需要复用时才抽象成Model模型



View视图

在controllers/default.go看到c.TplName = "index.tpl"，这个语句就是设置模板文件，该模板支持tpl和html文件，beego使用了golang默认的模板引擎


在views/index.tpl中可以看到这个


    <div class="author">
        Official website:
        <a href="http://{{.Website}}">{{.Website}}</a> /
        Contact me:
        <a class="email" href="mailto:{{.Email}}">{{.Email}}</a>
    </div>

在controller中，定义了数据，并且该数据赋值给Data，在模板文件中通过{{.Email}}来访问到数据


---


数据库交互


bee generate scaffold test -fields="id:int64,name:string" -driver=mysql -conn="root:root@tcp(127.0.0.1:3306)/test"


上面这个命令会自动生成代码，其中-fields参数为字段，-driver为引擎，-conn为数据库登录信息


如果没有打错指令，会提示是否创建mvc模型的文件，根据提示选择是否要创建


在models下的test.go文件，可以看到已经添加了管理数据库的函数


---



静态文件的引用

beego默认使用static目录作为静态文件的目录，在main.go文件中beego.Run()之前配置StaticDir["/static"] = "static"


如果有多个静态文件目录，可使用beego.SetStaticPath("/test", "test")












