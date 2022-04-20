---
title: "Taro脚手架学习笔记"
categories: [ "默认" ]
tags: [ "Taro" ]
draft: false
slug: "92"
date: "2021-08-31 12:15:00"
---

Taro是京东凹凸实验室推出的一个脚手架，设计目的是多端统一开发解决方案，一次开发，多端运行

Taro可以使用React/Vue等前端框架开发的，当然typescript肯定是支持

Taro官方文档：https://taro-docs.jd.com/taro/docs/README


Taro支持微信小程序，百度小程序，支付宝小程序，字节小程序，QQ轻应用，ReactNatvie等等


安装Taro脚手架

npm install -g @tarojs/cli

或者

yarn global add @tarojs/cli


升级脚手架工具

taro update self


创建项目

taro init hallo


创建h5项目

yarn dev:h5



创建微信小程序项目

yarn dev:weapp



创建完毕会在dist目录下生成小程序程序



作为一个可以使用react开发的脚手架，React Hooks和jsx也是支持的


Taro组件化（Taro可以使用react开发，因此也具备react的组件化功能）

    import Taro, {  useState } from '@tarojs/taro'
    import { View, Text } from '@tarojs/components'
    import './index.less'
    function Main(){
        const [count ,setUserName] = useState("hallo word")
       return ( 
           <View>
               <Text>{count}</Text>
           </View>
        )
    }
    export default Main


子组件


    import { View, Text } from '@tarojs/components'
    function Data(){
        return ( 
            <View><Text>hallo word</Text></View>
        )
    }
    export default Data


导入

import Data from './data.jsx'


直接在父组件中使用<Data></Data>就好


父组件传递参数给子组件

<Data count={count}></Data>


学过react都知道，父组件传递参数给子组件要通过props接收，因此子组件要加props参数


    function Data(props){
        return ( 
            <View><Text>{props.count}</Text></View>
        )
    }
    export default Data



Taro路由（通过app.jsx的pages，谁在第一个数组的值，那么该就是默认打开的首页）


    pages: [
        'pages/main/main',
        'pages/data/data'
    ],

注意，不需要导入页面，Taro可以自动化完成


Taro 提供了6个路由api，分别是

navigateTo：记录上级页面，可以返回上级页面

redirectTo：不记录上级页面

switchTab：Tab之间进行切换

navigateBack：返回上一级页面

getCurrentPages：获取当前页面信息（页面栈），注意h5不支持

relaunch：销毁所有页面（关闭）



路由配合页面之间传递参数

发送参数

例如：

    import Taro ,{useState} from '@tarojs/taro'
    import {View , Text ,Button} from '@tarojs/components'
    function Main(){
        const [data,setData] = useState("hallo")
        const UrlIndex=()=>{
            Taro.navigateTo({url:'/pages/index/index'+data})
        }
        return (
            <View>
                <Text>hallo word</Text>
                <Button onClick={UrlIndex}>GO</Button>
            </View>
        )
    }
    export default Main


只需要在Index.jsx下接收就可，例如：

    import Taro, {  useState ,useEffect } from '@tarojs/taro'
    ...
    const [data,setData] = useState('')
    useEffect(()=>{
        setData(this.$router.params.data)
     },[])
     ...
     <Text>{data}</Text>




多参数传递也是可以

        const [data,setData] = useState("hallo")
        const [name,setName] = useState("root")
        const [pass,setPass] = useState("123")
        const UrlIndex=()=>{
            Taro.navigateTo({url:'/pages/index/index?data='+data+'&name='+name+'&pass='+pass})
        }


接收多参数


    const [data,setData] = useState('')
    const [name,setName] = useState('')
    const [pass,setPass] = useState('')
    useEffect(()=>{
        setData(this.$router.params.data)
        setName(this.$router.params.name)
        setPass(this.$router.params.pass)
     },[])
     ...
     <Text>{data}</Text>
     <Text>{name}</Text>
     <Text>{pass}</Text>



导入静态资源


    import Taro ,{useState ,useEffect}from '@tarojs/taro'
    import {View , Text ,Button, Image} from '@tarojs/components'
    import {imgdata,cssdata} from '../../data' // 导入方法（函数）
    import logo  from '../../img/1.jpg' // 导入静态资源
    useEffect(()=>{
        imgdata()
        cssdata()
    },[])
     ...
     <Image src={newbbd} width="300px" height="300px" />


或者

     <Image src={require('../../img/1.jpg')} width="300px" height="300px" />




请求远程端口

    const testData= ()=>{
        Taro.request({
            url:'https://xxx.xxx/xxx'
        }).then(mainData=>{
            console.log(mainData.data)
        })
    }


url是目标接口，then()是请求完毕获取到的数据


