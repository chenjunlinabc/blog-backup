---
title: "Mobx学习笔记"
categories: [ "默认" ]
tags: [ "Mobx" ]
draft: false
slug: "78"
date: "2021-08-08 21:35:00"
---

Mobx是一个状态管理库（状态管理实质上就是在管理数据变化）

Mobx通过透明的函数响应式编程使得状态管理变得简单和可扩展

Mobx状态管理是基于观察者模式的（Mobx6.0移除了修饰器）


安装Mobx

    npm install mobx mobx-react --save

或者

    yarn add mobx mobx-react --save



导入

    import {observable, autorun, computed, action, makeObservable,reaction,when} from "mobx"
    import {observer} from "mobx-react"

注意：在严格模式中，是不允许在action之外改变状态的

启动严格模式

    import {configure} from 'mobx'
    configure({enforceActions: true})

observable定义可观察的状态
action修改状态（动作）
computed计算值



数据变化可被观察，例如：

    let appdata = observable([1,2,3,4,5])
    console.log(appdata[1])
    appdata = observable({a:1,b:2,c:3})
    console.log(appdata.a)
    appdata.a += 1
    console.log(appdata.a)
    appdata = observable.box(100)
    console.log(appdata)



响应式对象

makeObservable（手动配置observable，action，computed）

    const store = makeObservable({    
        count: 666,    
        get double(){
            return this.count * 2
        },
        increment(){
            this.count += 1
        },
        decrement(){
            this.count -= 1
        },
    },{
        count: observable, // 响应的属性
        double: computed, // 计算属性
        increment: action, // 修改状态
        decrement: action, // 修改状态
    })



makeAutoObservable（自动配置observable，action，computed，全部自有属性都为observable，全部getters为computed，全部computed都为action）


    const store = makeAutoObservable({  
        count: 666,
        get double(){
            return this.count*2
        },
        increment(){
            this.count+=1
        },
        decrement(){
            this.count-=1
        }
    })




计算属性（observable.box）


    let datas = observable.box(1000)
    let dataa = observable.box(666)
    let abc = computed(function(){
        return dataa + datas
    })
    console.log(abc.get())




操作状态（添加观察者，当状态发生变化，就会触发）

    autorun(() => {
      appdata.datas = appdata.datas + 1
      console.log(appdata.datas)
    })





监听数据变化（当store的状态发生改变，autorun方法就会被调用）

    let xyz = observable({
      title: "hello world"
    })
    autorun(() => {
        console.log(xyz.title)
    })
    xyz.title = "hallo mobx"

当被观察数据被修改时，autorun()就会被触发



reaction监听


    reaction(() => store.count,
        (a, b) => {
            // 当store.count修改就会被调用
            // 第一个值是当前值，第二个值为修改后的值
            console.log(a - b)
        }
    )



when监听


    when(() => store.count > 666, () => {
        console.log(store.count)
    })
    (async function() {
        // when还会返回一个promise
        await when(() => store.count > 666)
        console.log('store.count > 666')
    })()







Mobx和Redux的比较：Redux通过限制数据的修改，Store中的数据是不可修改，只能通过action来修改，减少冗余。而Mobx就是通过自动更新来确保数据一致

mobx鼓励使用多个store仓库，而Redux鼓励一个应用使用一个Redux仓库










