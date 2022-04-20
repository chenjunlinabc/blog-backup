---
title: "简单使用Jest-JavaScript测试工具"
categories: [ "默认" ]
tags: [ "Jest" ]
draft: false
slug: "95"
date: "2021-09-06 23:00:00"
---

Jest是Facebook开源的一套JavaScript测试框架


安装

在项目中安装

yarn add --dev jest或者npm install --save-dev jest

全局安装

yarn global add jest



---



hello.js

    module.exports = function(){
        return "hello world"
    }



hello.test.js

    const hello = require('../hello')
    it('should ', () => {
        expect(hello()).toBe('hello world')
    })



package.json


    {
        "scripts": {
            "test": "jest"
        }
    }

执行测试

yarn test或者npm run test



---


exspect() //运行结果

toBe() //期待的结果

not.toBe() //判断不等

toBeNull() //判断是否为NULL

toBeUndefined() //判断是否为undefined

toBeDefined() //判断是否为undefined取反

toBeTruthy() //判断结果为true

toBeFalsy() //判断结果为false

toBeGreaterThan(5) //判断结果是否大于5

toBeLessThan(5) //判断结果是否小于5

toBeGreaterThanOrEqual(6) //判断结果是否大于等于6

toBeLessThanOrEqual(6) //判断结果是否小于等于6

toBeCloseTo(3.14) //判断结果是否相等于3.14浮点数

toMatch() //判断结果正则表达式

toContain() //判断是否包含


---

Jest默认使用require引用（CommonJS），而使用import的话会报错，因此需要babel工具来将其转换为CommonJS

当然需要安装babel

yarn add @babel/core@7.4.5 @babel/preset-env@7.4.5  --dev


新建.babelrc文件（babel转换配置文件）


    {
        "presets":[
            [
                "@babel/preset-env",{
                    "targets":{
                        "node":"current"
                    }
                }
            ]
        ]
    }


