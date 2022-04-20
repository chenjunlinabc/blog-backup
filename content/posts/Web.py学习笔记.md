---
title: "Web.py学习笔记"
categories: [ "默认" ]
tags: [ "Python","web.py" ]
draft: false
slug: "117"
date: "2021-10-18 12:00:00"
---

web.py是一个轻量级Python web框架，是由已故著名计算机黑客Aaron Swartz设计开发（如果你看过互联网之子这个电影，你应该对这位大佬很熟悉）


安装web.py

pip install  web.py


导入模块

import web




第一个例子

    import web
    urls = (
        "/(.*)","hallo"
    )
    app = web.application(urls,globals())
    class hallo:
        def GET(self,name):
            return "<h1>hallo web.py</h1>"
    if __name__=="__main__":
        app.run()


可以看到页面内容是return返回的，也可以open读取html文件，来返回回去，都是可以的