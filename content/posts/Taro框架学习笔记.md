---
title: "Taro框架学习笔记"
categories: [ "学习" ]
tags: [ "Taro" ]
draft: false
slug: "92"
date: "2021-08-31 12:15:00"
---

Taro是京东凹凸实验室推出的一个框架，设计目的是多端统一开发解决方案，一次开发，多端运行

Taro可以使用React/Vue等前端框架开发的，当然typescript肯定是支持

Taro官方文档：https://taro-docs.jd.com/taro/docs/README


Taro支持微信小程序，百度小程序，支付宝小程序，字节小程序，QQ轻应用，ReactNatvie等等


安装Taro框架

npm install -g @tarojs/cli

或者

yarn global add @tarojs/cli


升级框架

taro update self


创建项目

taro init hallo


创建h5项目

yarn dev:h5



创建微信小程序项目

yarn dev:weapp



创建完毕会在dist目录下生成小程序程序

支付宝小程序
yarn dev:alipay

百度小程序
yarn dev:swan

ReactNative
yarn dev:rn

编译完成的端源码在dist目录下

作为一个可以使用react规范开发的框架，React Hooks和jsx也是支持的


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


---



生命周期与state

Taro支持react的生命周期，另外Taro还为小程序补充了一些react不存在的生命周期，方便Taro开发小程序时使用

组件的生命周期

    class Test extends Component{
        constructor (props) {
            // constructor会在组件挂载（组件渲染到Taro虚拟DOM之前）之前被执行
            super(props)
            this.state = {
                data: 'admin'
            }
        }

        componentWillMount (){
            console.log('第一次渲染之前执行') // 组件被装载渲染之前执行
        }
        componentDidMount(){
            console.log('第一次渲染之后执行') // 组件被装载渲染之后执行
            this.setState({ data: 'root' }) // 修改state状态，需要通过setState方法来修改，而不是直接修改
        }
        componentWillUnmount(){
            console.log('组件被销毁或者被卸载时执行')
        }
        componentWillUpdate(){
            console.log('state数据更新前执行') // 当组件接收到新的state或者props，该钩子会在渲染之前执行，其中也可以接收2个参数，nextProps, nextState
        }
        componentDidUpdate(){
            console.log('state数据更新后执行') // 当发送更新时执行。第一次渲染不会被触发
        }
        shouldComponentUpdate(nextProps, nextState){
            // 表示该组件不受state状态或者props影响，通过返回值来表示是否在每次state状态更新时重新渲染，默认返回true
            // 该钩子函数接收2个参数，分别是nextProps, nextState，分别表示最新的props和state
            console.log(nextProps, nextState)
            return false
        }
        componentWillReceiveProps(nextProps){
            console.log('Props参数发生改变之前执行') // 当已被装载渲染的组件接收到新的props属性之前执行，nextProps参数表示最新的props属性
        }
        getSnapshotBeforeUpdate(){
            console.log('在最近一次渲染之前执行') // 可用来组件渲染之前捕获数据
        }
        render(){
            return(
               <View>
                  <Text>hallo word {this.state.data}</Text>
               </View>
            )
        }
    }




---




Taro Props（父子组件通信）

子组件

    export default class DataTest extends Component{
        render(){
            let {data} = this.props
            return(
               <View>
                  hallo {data}
               </View>
            )
        }
    }

父组件

    import DataTest from './datatest'
    export default class Test extends Component{
        constructor (props) {
            super(props)
            this.state = {
                data: 'admin'
            }
        }
        render(){
            let {data} = this.state.data
            return(
               <View>
                  <Text>hallo word</Text>
                   <DataTest data={data}/>
               </View>
            )
        }
    }



props默认值

     DataTest.defaultProps = {
          data: 'user'
     }

注意：当props为undefined时，才将读取defaultProps的值作为props


---