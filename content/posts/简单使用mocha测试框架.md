---
title: "简单使用mocha测试框架"
categories: [ "默认" ]
tags: [ "mocha" ]
draft: false
slug: "114"
date: "2021-10-03 12:55:00"
---

mocha是JavaScript测试框架

安装

npm install --global mocha


测试，例如：

demo.js

    function abc(a,b,c){
        return a+b+c
    }
    module.exports = abc

demo.test.js

    const demo = require('./demo.js')
    const expect = require('chai').expect
    describe('test', function() {
        it('错误', function() {
            expect(demo(1,3,7)).to.be.equal(11)
        })
    })


测试（允许测试多个，默认执行test子目录的测试文件，如果test子目录存在该文件，可以不用加参数）

mocha demo.test.js


其中expect(demo(1,3,7)).to.be.equal(11)是断言，当1+3+7的结果不是11的时候，抛出错误

因为mocha本身没有断言库，需要导入 const expect = require('chai').expect


查看内置的全部报告格式（默认是spec）

mocha --reporters

使用Dot格式显示

mocha --reporter dot


使用HTML报告

npm install --save-dev mochawesome


---

mocha其他参数

--watch：监听指定测试脚本，只要测试脚本发生改变就自动执行mocha

搜索测试实例（通过名称）

mocha --grep "test"

--invert ：只执行不符合条件的测试脚本，要搭配--grep使用



---


如果要测试ES6，需要转码

npm install babel-core babel-preset-es2015 --save-dev

.babelrc

    {
        "presets": [ "es2015" ]
    }

mocha --compilers js:babel-core/register


---


注意：mocha默认每个测试实例只能最多执行2000毫秒，如果在这个时间里没处理完毕将报错

需要指定超时时间（-t或--timeout参数），例如：

mocha -t 6000 demo.test.js

也可以设置-s或-slow参数来指定超过一定时间的部分高亮显示

mocha -t 6000 -s 3000 demo.test.js


---

生成指定格式的测试文件

mocha --recursive -R markdown > demo.md

mocha --recursive -R doc > demo.html


