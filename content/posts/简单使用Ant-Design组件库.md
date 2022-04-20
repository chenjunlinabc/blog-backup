---
title: "简单使用Ant Design组件库"
categories: [ "默认" ]
tags: [ "React","Ant Design" ]
draft: false
slug: "82"
date: "2021-08-10 23:03:00"
---

Ant Design是蚂蚁金服技术沉淀出一套基于React的组件库和前端框架

官网：https://ant.design/index-cn

使用create-react-app初始项目

yarn create react-app antd-demo

运行

yarn start

安装antd组件库

yarn add antd


通过import { 组件名 } from "antd"方式导入antd组件


导入antd css样式

@import '~antd/dist/antd.css';



在typescript上使用


yarn create react-app antd-demo-ts --template typescript




Antd的样式使用了less作为开发语言






第一个实例demo



    import 'antd/dist/antd.css';
    import { DatePicker, Space } from 'antd';
    ReactDOM.render(
      <Space direction="vertical">
        <DatePicker/>
      </Space>,
      document.getElementById('root'),
    );