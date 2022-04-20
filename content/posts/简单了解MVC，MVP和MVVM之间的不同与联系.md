---
title: "简单了解MVC，MVP和MVVM之间的不同与联系"
categories: [ "技术" ]
tags: [  ]
draft: false
slug: "56"
date: "2021-07-09 22:50:00"
---

MVC（Model-View-Controller）分别为View（视图，用户界面），Model（模型，数据保存），Controller（控制器，逻辑）

视图层发指令（Dom事件）给控制器，控制器完成逻辑处理，请求模型改变状态，模型将最新的数据发送给视图，得到反馈，各个之间通信是单向的


也可以Controller接受指令，要求模型改变状态，模型将最新的数据发送给视图



MVP（Model-View-Presenter）实质是就是将Controller（控制器，逻辑）改为Presenter（从模型中获取数据，并且提供数据给视图）

各个通信之间是双向的，视图和模型并不联系，通过Presenter进行传递，全部逻辑都在Presenter进行处理




MVVM（Model-View-ViewModel）实质上就是MVC的改进版，和MVP模式基本一致，不过MVVM采用了双向绑定，视图变化，自动反映在ViewModel中

在前端中Model是用json表示，将Model和View关联起来的是ViewModel，Mode数据可以显示到View中，也可以将View修改回Mode






















