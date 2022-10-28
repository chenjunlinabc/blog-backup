---
title: "Gin框架学习笔记"
categories: [ "学习" ]
tags: [ "golang","Gin" ]
draft: false
slug: "135"
date: "2022-02-15 17:00:00"
---

Gin是一个基于go语言编写的web框架，因为Gin的路由库基于httprouter开发的，性能非常好，支持Restful api规范


安装

go get -u github.com/gin-gonic/gin


第一个demo

    package main
    import "github.com/gin-gonic/gin"
    import "net/http"
    func main() {
	g := gin.Default()
	g.GET("/", func(c *gin.Context) {
		c.String(http.StatusOK, "hallo word")
	})
	g.Run()
    }


go run main.go

g.Run()是将应用部署到本地服务器上，默认端口为8080，可设置端口，g.Run(":2333")


路由

    r.GET("/test/:name", func(c *gin.Context) {
        name := c.Param("name")
        c.String(http.StatusOK, name)
    })
    g.Run(":6666")

127.0.0.1:6666/test/xiaochen

可以看到Context的Param方法可以获取路由的参数


通过url传递参数

    r.GET("/test", func(c *gin.Context) {
        name := c.DefaultQuery("name", "test")
        c.String(http.StatusOK, fmt.Sprintf("hallo %s", name))
    })
    r.Run()

127.0.0.1:6666/test

如果没有传递参数将会输出DefaultQuery的默认参数test


传递参数后 127.0.0.1:6666/test?name=word

POST请求

index.html
    <input type="text" name="user" placeholder="name">
    <input type="password" name="pass" placeholder="pass">
    <input type="submit" value="提交">

main.go

    r.POST("/form", func(c *gin.Context) {
        types := c.DefaultPostForm("type", "post")
        user := c.PostForm("user")
        pass := c.PostForm("pass")
        c.String(http.StatusOK, fmt.Sprintf("user:%s,pass:%s,type:%s", name, pass, types))
    })
    r.Run()

