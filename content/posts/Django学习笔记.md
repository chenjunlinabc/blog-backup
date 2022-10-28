---
title: "Django学习笔记"
categories: [ "默认" ]
tags: [ "Python","django" ]
draft: false
slug: "118"
date: "2021-10-22 18:50:00"
---

django是一个基于Python编写的Web框架

Django采用了MVT的设计模式（模型，视图，模板）（mvt设计模式是基于mvc设计模式的）


安装django

pip install django

检查是否安装成功

django-admin


django常用命令

startproject #创建django项目

startapp #创建django应用

check #检查校验项目完整性

runserver #本地运行django项目

shell # 进入django项目的shell环境（Python）

test # 执行django实例测试

makemigrations # 创建模型变更的迁移文件

migrate # 执行迁移文件

dumpdata # 数据库数据导出

loaddata # 文件数据导入数据库


---



创建第一个项目

django-admin startproject django_demo

settings.py是django项目的配置文件，urls.py是django项目的路由文件，wsgi.py是django作为wsgi应用的配置文件（wsgi，全成web server gateway interface，这个文件用来部署应用服务器），manage.py是django项目的管理文件，__init__.py是Django项目的包初始化文件


wsgi：Web服务器网关接口（Python Web Server Gateway Interface，缩写为WSGI）是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口。（来自百度百科）


运行django项目

python .\manage.py runserver 0.0.0.0:8000

访问127.0.0.1:8000


---


django应用


django应用和django项目的区别：django项目是基于django的web应用，可以独自运行，django应用是一个可复用（重用）的Python软件包


django项目下可以包含一组配置和n个django应用


创建django应用

python .\manage.py startapp django_app


admin.py #定义admin模块管理的配置文件

apps.py # 声明应用的配置文件

tests.py # 应用测试的配置文化

models.py #定义应用模型配置文件

views.py #视图处理配置文件

urls.py # 管理应用路由的配置文件，因为应用也可以管理路由，不过需要自己创建


django应用用法：

修改settings.py，找到INSTALLED_APP，添加"django_app"（用于挂载该应用），找到TEMPLATES下的DIRS，添加os.path.join(BASE_DIR, "templates")，（用于加载模板文件），找到ALLOWED_HOSTS，修改为ALLOWED_HOSTS = ['*']，（让外界可访问到后台站点）




---


django视图&django路由


简单配置一个路由来获取请求的数据


views.py

    def hallo(request):
        name = request.GET.get("name")
        return HttpResponse('{}'.format(name))

项目的urls.py（在urlpatterns数组中添加，abc为路由）

    path("abc/",include("django_app.urls"))

不要忘了导入include

    from django.urls import path, include


在应用下新建一个urls.py

    from django.urls import path, include
    import django_app.views
    urlpatterns = [
        path("hallo",django_app.views.hallo)
    ]


上面例子中，访问/abc将触发一个应用级路由，该应用路由的返回值为该应用的views.py下的hallo函数


另外到settings.py中添加应用

INSTALLED_APPS数组下添加"django_app.apps.DjangoAppConfig"


应用名.apps.xxxConfig


xxxConfig可以到apps.py中找到

访问http://127.0.0.1:8000/abc/hallo?name=admin，可以看到输出了admin





---

模型层（位于视图层和数据库之间的组件）

模型层主要目的：Python对象和数据库表之间的转换，屏蔽不同的数据库的差异，提供更多的数据库工具（备份等等）


模型层字段定义

数字类型：IntegerField

文本类型：TextField

日期类型：DataTimeField

布尔类型：BooleanField

自增：AutoField

主键：primary_key属性



在settings.py中找到DATABASES，这是用来配置模型层和数据库的配置

另外需要到应用的models.py中定义模型

    class demotest(models.Model):
        demo_id = models.AutoField(primary_key=True)
        title = models.TextField()
        content = models.TextField()
        demo_data = models.DateTimeField(auto_now=True)
        def __str__(self):
            return self.title


然后生成迁移文件，执行迁移文件，执行迁移文件后就是可以同步到数据库中




---


django shell（交互式编程，方便测试，方便开发，继承django项目环境）

启动django shell

python manage.py shell

测试一下刚才创建的模型

    from django_app.models import Main
    main = Main()
    main.title = "test django"
    main.content = "hallo word"
    main.save()
    datas = Tast.objects.all()
    data = datas[0]
    print(data.title)


---


Admin模块（管理）

主要用途：认证用户，显示管理模型，检查输入等等

创建管理员用户

python manage.py createsuperuser

会要求输入用户名和邮箱，以及密码


注册模型

admin.py

    from .models import datas
    admin.site.register(datas)



---


返回数据库中的数据

views.py

    from django_app.models import datas
    def data_content(request):
        data = datas.objects.all()[0]
        title = data.title
        content = data.content
        demo_id = data.demo_id
        demo_data = data.demo_data
        return_str = "title: %s, demo_id: %s, demo_data : %s, content: %s (title,demo_id,demo_data,content)"
        return HttpResponse(return_str)

应用路由配置

    path("data",django_app.views.data)

项目路由配置（如果已经配置转发到应用的路由，就不用再配置了）
    


---





模板系统

除了模板外，也可以直接上静态文件（html，css，JavaScript等等）

在应用目录下新建一个目录，templates，在该目录下新建一个html


当然使用模板系统更好更分别，毕竟逻辑和设计要分开

模板基础语法

变量：<html><body>{{ data }}</html></body>
for循环：{% for data in datas %},{% endfor %}
if判断：{% if %},{ % else % },{% endif %}


启用模板系统 views.py

    def index_page(request):
        all_data = datas.objects.all()
        return render(request, "templates/index.html",{
            "datas": all_data
        })
        pass


应用路由配置

    path("index",django_app.views.ndex_page)



页面跳转
    
