---
title: "Pinia---vuejs的轻量级状态管理库"
categories: [ "默认" ]
tags: [ "vuejs","Pinia" ]
draft: false
slug: "124"
date: "2021-12-26 20:22:00"
---

Pinia是vuejs的轻量级状态管理库，Pinia支持Vue devtools浏览器扩展工具，可扩展，模块化设计，热模块更新，轻量级，支持TypeScript，支持SSR服务器端渲染，支持vue2，vue3

Pinia作者也是vuex核心之一

安装pinia

npm install pinia@next

或者

yarn add pinia@next


导入pinia并且挂载为vue插件(在Vite脚手架下)

src/main.js

    import { createApp } from 'vue'
    import App from './App.vue'
    const app = createApp(App)
    import { createPinia } from 'pinia'
    app.use(createPinia())
    app.mount('#app')

src/stores/main.js（pinia通过defineStore函数来创建state，并且接收一个id来标识state）

    import { defineStore } from 'pinia'
    export const useDataStore = defineStore('data', {
        state: () => {
            return { count: 666 }
        },
        //state: () => {
            //name: chen
        //},
        getters:{ 
            // getters计算属性
            doubleCount(state){
                return state.count * 2
            }
            doublePlusOne(){
                // 在getters中调用其他getters，只需要this访问一下就可以
                // return this.doubleCount + 123
                // 通过箭头函数参数，来动态计算
                return (a)=>{
                    this.doubleCount + a
                }
            }
        },
        actions: {
            increment() {
                this.count++
            }
        }
    })

src/app.vue（在组件引入useDataStore，并且获取state状态）

    import { useDataStore } from "./stores/main.js"
    const store = useDataStore()
    console.log(store.count)
    ...
    <template>
        <h2>{{ store }}</h2>
        <h2>{{ store.count }}</h2>
    </template>


使用storeToRefs函数解构状态（storeToRefs解构，可以响应式状态）

src/app.vue

    import { storeToRefs } from "pinia"
    import { useDataStore } from "./stores/main.js"
    const { count , doubleCount , doublePlusOne} = storeToRefs(useDataStore())
    ...
    <template>
        <h2>{{ count }}</h2>
        <h2>{{ doubleCount }}</h2>
        <h2>{{ doublePlusOne(6) }}</h2>
    </template>


---


调用其他state的getters（假如在其他地方定义了一个getters，想使用）

    addData(state){
        // 获取另一个通过defineStore创建state的实例
        const store = useAddStore()
        return state.count + store.intData
    }



---

Actions操作state（同步异步都可以使用Actions）

    export const useStore = defineStore('main', {
        state: () => ({
            counter: 0,
        }),
        actions: {
            countPlusOne() {
                this.counter++
            },
            countPlus(a) {
                this.counter += a
            }
        }
    })
