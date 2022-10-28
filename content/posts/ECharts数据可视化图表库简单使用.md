---
title: "ECharts数据可视化图表库简单使用"
categories: [ "技术" ]
tags: [ "ECharts" ]
draft: false
slug: "153"
date: "2022-06-18 00:12:00"
---

ECharts是基于JavaScript的数据可视化图表库

安装

    npm install echarts --save


第一个实例

    import * as echarts from 'echarts'
    let app = echarts.init(document.getElementById('app'), null, {
        width: 800,
        height: 500
    })
    let data = {
        title: {
            text: '用户管理'
        },
        tooltip:{},
        legend: {
            data: ['用户']
        },
        xAxis: {
            data: [
                'root','admin','user1','user2','user3'
            ]
        },
        yAxis: {},
        series: [
            {
                name: '用户权限',
                type: 'bar',
                data: [
                    10,8,5,1,3
                ]
            }
        ]
        
    }
    app.setOption(data)


注意：容器必须具备高度和宽度（这里的容器的id为app），要么html指定，要么在初始化时指定一个