---
title: "Angular学习笔记"
categories: [ "默认" ]
tags: [ "Angular" ]
draft: false
slug: "101"
date: "2021-09-13 12:00:00"
---

Angular是三大前端框架之一（Angular在国内的热度低，但是在国外热度还是很高的，主要是因为Angular到Angular2的断崖式升级）

Angular和Vue的区别就是，Angular具备完整的MVVM框架功能（功能高度集成），提供一套完整的解决方案，而Vue是轻量级MVVM框架（渐进式，还需要vue-router之类的扩展功能），Angular和Vue并没有谁好谁坏之分，各有风格

注意：Angular是AngularJS的重写，AngularJS使用JavaScript编写完成，而Angular采用TypeScript编写完成


安装

npm install -g angular-cli


第一个Angular应用

    <div ng-app ng-init="name='default'">
        <p>name: <input type="text" ng-model="name"></p>
        <h1>hallo，{{name}}</h1>
        <p ng-bind="name"></p>
    </div>


ng-app属性将其声明为是一个Angular应用，ng-model将数据绑定到name中，ng-bind将其输出绑定，ng-init是初始化值



