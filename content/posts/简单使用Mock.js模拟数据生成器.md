---

title: "简单使用Mock.js模拟数据生成器"
categories: [ "技术" ]
tags: [ "Mock.js" ]
draft: false
slug: "160"
date: "2022-11-09 12:00:00"
---


Mock.js是模拟数据生成器，不需要后端来提供接口数据来进行开发，Mock可以根据数据模板生成随机数据，并且拦截Ajax请求返回模拟数据

安装

npm install mockjs

导入

    import Mock from 'mockjs'

通过传入数据模板对象生成数据

    const data = Mock.mock({
        'list|10': [{
            "id|+1": 1,
            "name": "@cname",
            "age|18-25": 25
        }]
    })
    console.log(data)

配置响应数据（当匹配url的ajax请求时，会根据数据模板生成模拟数据，并且作为响应返回，这里通过axios发送get请求）

    const Data = Mock.mock('/api/name','get',{
        code: 200,
        data: {
            'list|10': [{
                "id|+1": 1,
                "name": "@cname",
                "age|18-25": 25
            }]
        }
    })
    axios.get('/api/name').then(
        res => {
            console.log(res.data)
        }
    )

也可以传入第二个参数，表示匹配的请求是哪个请求方法的

记录数据模板

    const Data = Mock.mock('/api/name',(options) => {
        return {
            code: 200,
            options
        }
    })
    axios({method: 'get', url: '/api/name' , data: {'name':'chenjunlin'}}).then(
        res => {
            console.log(res.data)
        }
    )

配置拦截ajax请求时的行为，目前只有指定响应时间功能，单位毫秒

    Mock.setup({
        timeout: '200-500'
    })

表示响应时间介于200到500毫秒之间，默认值为10-100

Mock.Random工具类，Random.extend扩展方法（扩展Mock.Random工具类的方法，也能扩展到Mock.mock数据模板）

    const Random = Mock.Random
    const data1 = Random.cname()
    const data2 = Random.email()
    console.log(data1,data2)
    Random.extend({
        emailall: function(date){
            const emailall = [
                'a@cjlio.com',
                'b@cjlio.com',
                '1@cjlio.com',
                '10001@qq.com',
                '123456@qq.com'
            ]
            return this.pick(emailall)
        }
    })
    const data3 = Mock.mock({
        'list|3': [{
            'email': '@EMAILALL'
        }]
    })
    console.log(data3)

校验data数据是否符合数据模板

    const template = {
        "id|+1": 1,
        "name": "@cname",
        "age|18-25": 25
    }
    const data = {
        id: 1,
        name: '小陈',
        age: 20,
        pass: '123456'
    }
    const abc = Mock.valid(template,data)
    console.log(abc)

如果符合返回是空数组，如果不符合，会返回不符合在哪里，这个功能可以验证模拟数据和生产数据是否符合模型，避免模拟数据模型和生产数据模型不同，导致生产事故

将Mock风格的模板转换为json

    const template = {
        "id|+1": 1,
        "name": "@cname",
        "age|18-25": 25
    }
    const abc = Mock.toJSONSchema(template)
    console.log(abc)

详细使用方法请看Mockjs官方文档，<https://github.com/nuysoft/Mock/wiki>
