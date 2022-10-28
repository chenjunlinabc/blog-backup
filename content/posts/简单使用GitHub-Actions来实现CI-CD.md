---
title: "简单使用GitHub Actions来实现CI/CD"
categories: [ "技术" ]
tags: [ "Git","Github" ]
draft: false
slug: "131"
date: "2022-02-01 18:00:00"
---

CI：持续集成 (Continuous Integration)
CD：持续交付 (Continuous Delivery)
CD：持续部署 (Continuous Deployment)


GitHub Actions是GitHub提供的持续集成服务

GitHub Actions官方文档：https://docs.github.com/en/actions

workflow：工作流程，指一次持续集成的流程，由一个job或者多个job组成
Events：事件，触发流程的钩子（在github中事件为检测仓库特定活动的钩子，例如pull，当事件被触发则自动执行工作流程）
Job：任务，任务是工作流程的主体
Steps：步骤，每个Job可以包含一个或多个Step
Actions: 行为，每个Step包含一个或多个Action
Runners: 执行环境，工作流程运行时的服务端，每一个执行环境可以运行一个任务


workflow工作流程通过编写workflow文件来描述，workflow文件要使用YAML语言编写，github支持多个workflow（当github发现.github/workflows/目录下有.yml文件时就会执行该文件）

在仓库的.github/workflows/目录下创建test.yml，其中要配置字段

name：workflow名称，如果省略默认为当前workflow的文件名

on：指定触发workflow的条件，一般为事件触发（比如说push）

jobs：每一项任务都需要定义个job_id，job中的name为该任务的描述，needs为指定当前任务的运行顺序（依赖关系），runs-on为指定运行时需要的虚拟机环境（这个字段必须填）

目前github支持的虚拟机操作系统有ubuntu，windows，macOS，而且github提供的虚拟机是免费使用的

这里举个例子（github官方的）

    name: GitHub Actions Demo
    on: [push]
    jobs:
      Explore-GitHub-Actions:
        runs-on: ubuntu-latest
        steps:
          - run: echo " The job was automatically triggered by a ${{ github.event_name }} event."
          - run: echo " This job is now running on a ${{ runner.os }} server hosted by GitHub!"
          - run: echo " The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
          - name: Check out repository code
            uses: actions/checkout@v2
          - run: echo " The ${{ github.repository }} repository has been cloned to the runner."
          - run: echo " The workflow is now ready to test your code on the runner."
          - name: List files in the repository
            run: |
              ls ${{ github.workspace }}
          - run: echo " This job's status is ${{ job.status }}."