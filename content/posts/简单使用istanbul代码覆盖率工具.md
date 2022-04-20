---
title: "简单使用istanbul代码覆盖率工具"
categories: [ "默认" ]
tags: [ "istanbul" ]
draft: false
slug: "113"
date: "2021-09-30 12:22:00"
---

代码覆盖率：是否所有代码都被执行或者调用

每一行，每个函数，每个语句块，每个if分支是否都被执行或者被调用

istanbul是JavaScript的覆盖率工具（类似工具还有NYC）（可搭配mocha使用）

安装

npm install -g istanbul

测试覆盖率

istanbul cover demo.js


检查程序覆盖率是否达到某个值

istanbul check-coverage --statement 60  --branch -5 --function 100



---


在执行检查测试后，会在目标文件的当前目录下生成个coverage文件夹

在coverage/lcov-report/index.html，可以查看网页版结果