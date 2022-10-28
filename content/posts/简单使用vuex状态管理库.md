---
title: "简单使用vuex状态管理库"
categories: [ "技术" ]
tags: [ "vuejs","vuex" ]
draft: false
slug: "123"
date: "2021-12-13 06:19:00"
---

vuex是一个专门为vuejs应用程序的设计的状态管理

集中式存储管理应用的所有组件的状态

多组件状态共享，不同的组件改变同一个状态


vuex知识点：state，getter，mutation，action


安装vuex

npm install vuex --save

或者

yarn add vuex


导入vuex包

import Vuex from "vuex"



创建vuex实例

new Vuex.store()


将vuex实例挂载在vue对象上

index.js

    Vue.use(Vuex)
    const store = new Vuex.Store({
        state: {
            count: 0
        }
    })
    export default store

main.js

    new Vue({
        store // 将store对象添加到vue实例上
    })

可以通过this.$store.state.count获取到状态（官方推荐将获取装态的操作放到computed中）

使用解构获取状态：
    import { mapState } from 'vuex'
    export default {
        mounted() {
            console.log(this.count)
        },
        computed: {
            ...mapState(['count'])
        }
    }


---

Getter


    getters: {
        getCount(state) {
            return state.count + 1
        }
    }

    ...

    export default {
        mounted() {
            console.log(this.$store.state.count)
            console.log(this.$store.getters.getCount)
        }
    }


mapGetters函数解构到计算属性（将store中的getter映射到局部计算属性）

    computed: {
        ...mapGetters(['getCount'])
    }

或者起别名(起别名是导入对象{})

    computed: {
        ...mapGetters({mainData: 'getCount'})
        // this.mainData为this.$store.getters.getCount
    }






---


Mutation


在vuex中不能直接修改store的值，而是通过Mutation方法来修改，例如：

    mutations: { 
        // setData(state) {  
            // state.count = 3
        // }
        setMain(state, intData) {  
            state.count = intData.count
            // 通过传参的方式修改
        }
    }


App.vue

    export default {
        mounted() {
            console.log(this.$store.state.count)
            this.$store.commit('setMain',{count: 123})
            console.log(this.$store.state.count)
        },
    }

注意：Mutation方法内部的函数必须是同步的，是不能处理异步的

在组件中也是可以使用Mutation解构的（Mutation）

导入并且解构

    import { mapMutations } from 'vuex'
    export default {
        mounted() {
            this.setMain({ count: 666 });
        },
        methods: {
            // 通过mapMutations解构到methods里面，其别名也是一样的操作
            ...mapMutations(['setMain'])
        },
    }







---


Actions（异步操作，也支持mapActions解构起别名）

    const store = new Vuex.Store({
        state: {
            count: 0
        },
        mutations: {
            setMain(state, intData) {  
                state.count = intData.count
                // 通过传参的方式修改
            }
        },
        actions: {
            intData (context, mainData) {
                return new Promise(resolve => {
                    setTimeout(() => {
                        context.commit('setMain', {count: mainData:count})
                        resolve()
                    },3000)
                })
            }
        }
    })


App.vue

    async mounted() {
        console.log(this.$store.state.count)
        await this.$store.dispatch('intData',{count: 123})
        console.log(this.$store.state.count)
    }






---

推荐使用pinia来代替vuex，pinia对ts的支持程度更好，更轻量
https://github.com/vuejs/pinia
