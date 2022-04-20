---
title: "Axios网络请求库学习笔记"
categories: [ "默认" ]
tags: [ "Axios" ]
draft: false
slug: "83"
date: "2021-08-11 12:15:00"
---

Axios是一个基于promise的http网络请求库，可以用于浏览器和nodejs，在nodejs中使用http模块，而在浏览器使用XMLHttpRequests

支持promise api，支持拦截请求和响应，转换请求数据和响应数据，取消请求，自动转换json数据，支持防御XSRF攻击





安装

yarn add axios


实例demo



    import axios from "axios";
    axios.get("https://httpbin.org/get", {
        params: {
            name: "root"
          }
    })
    .then(function (response) {
         console.log(response);
    })
    .catch(function (error) {
          console.log(error);
    });
    axios.post("https://httpbin.org/post", {
         name: "root",
         pass: "root"
    })
    .then(function (response) {
         console.log(response);
    })
    .catch(function (error) {
         console.log(error);
    });



可以看到页面已经发送了get和post请求，.then和.catch分别表示请求成功和请求失败时调用的函数，也可以用箭头函数，其中response参数为请求的数据，error为错误信息


还可以写成这样



    axios({
        method: 'post',
        url: 'https://httpbin.org/post',
        data: {
            name: "root",
            pass: "root"
        }
    });



---


FormData方式


    let data = {
        home: "hallo",
        main: "abc"
    }
    let formData = new FormData()
    for(let key in data){
        formData.append(key,data[key])
    }
    axios.post('https://httpbin.org/post',formData).then(
        res =>{
            console.log(res)
        }
    )




---

put和patch

    let data = {
        home: "hallo",
        main: "abc"
    }
    axios.put("/put",data).then(res =>{
        console.log(res)
    })
    axios.patch("/patch",data).then(res =>{
        console.log(res)
    })





---




delete请求

    axios.delete("/delete",{
        params: {
            home: "hallo"
        }
    }).then(res =>{
        console.log(res)
    })


注意：params是在URL上使用，而data就不是在URL上使用（Request Payload）





---






并发请求（同时进行多个请求，并且统一处理返回值）


    axios.all([
        axios.get("https://httpbin.org/get"),
        axios.get("/data.json")
    ]).then(
        axios.spread((urlRes,dataRes)=>{
            console.log(urlRes,dataRes)
        })
    )

注意：实质上进行请求的时候还是有个顺序的，具体顺序是看axios.all里面的数组元素，只是提供了统一管理请求而且







---


请求返回的数据，其中config是一些请求信息，包含请求方式，文件路径等等，请求到的真实数据实质上是在response.data里，还有像status（状态码），headers（请求头）等等都有



常见请求方式：get，post，put，patch，delete

put和patch一般是用来更新数据，区别就是put要将所有数据推送到后端，patch只将修改过的数据推送到后端，delete是用来删除数据，具体还得看后端怎么定义


params: 请求参数拼接到url中，data：请求参数放在请求体中



---



axios实例


    let AxiosDemo = axios.create({
        baseURL:"http://localhost:8080"
        timeout: 1000
    })
    AxiosDemo.get("/demo.json").then(res =>{
            console.log(res)
        }
    )


或者直接访问属性来定义


    let instance = axios.create()
    instance.default.timeout = 1000
    instance.defaults.baseURL = 'http://localhost:8080'


当多个接口的超时时长（timeout）不同或者需要访问服务地址（服务请求或者响应的结构可能不同时）有多个的时候就可以使用实例化axios来自定义，超时（timeout）默认为1000毫秒，超出就返回超时401


也可以搞全局配置


    axios.defaults.baseURL = 'http://localhost:8080'
    axios.defaults.timeout = 1000




---




拦截器（请求或者响应被处理之前拦截）

请求拦截器

    axios.interceptors.request.use(config =>{
        return config
    },err=>{
        return Promise.reject(err)
    })


可以看到有两个参数，第一个是发送请求前要进行处理什么东西的回调函数，另一个是在请求错误的时候应该处理什么的回调函数




响应拦截器


    axios.interceptors.response.use(res =>{
        return res
    },err=>{
        return Promise.reject(err)
    })


可以看到有两个参数，第一个是当请求成功后对响应数据要进行处理什么东西的回调函数，另一个是在响应错误的时候应该处理什么的回调函数





取消拦截器

    let interceptors = axios.interceptors.response.use(config =>{
        config.headers.token = ""
        return config
    })
    axios.interceptors.request.eject(interceptors)


注意：不要直接使用对象来声明一个属性的值，而是应该使用点来访问属性来定义，否则其他属性都会被覆盖掉，例如：


    config.headers ={
        auth: true
        token = ""
    }



---



取消请求


    let source = axios.CancelToken.source()
    axios.get("/data.json",{
        cancelToken: source.token
    }).then(res=>{
        console.log(res)
    }).catch(err=>{
        console.log(err)
    })
    source.cancel("cancel http")



---

解决跨域问题

    headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
    }









