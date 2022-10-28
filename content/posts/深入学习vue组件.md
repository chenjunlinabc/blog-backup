---
title: "深入学习vue组件"
categories: [ "学习" ]
tags: [ "vuejs" ]
draft: false
slug: "53"
date: "2021-07-03 11:16:00"
---

组件注册

.component()接收2个参数，其中第一个参数是组件名（数据类型是字符串）


组件名命名：

全部小写，多个单词用连字符连接（-减号）

因为html中是大小写不敏感的，浏览器会将大写解析为小写，因此不要使用驼峰命名法来命名自定义标签，而是使用连字符分隔

组件被引用时，也必须是相同的名，例如：组件名是img-data，那么被引用时标签为<img-data>，闭不闭合看组件的定义，如果组件的定义是img之类的，就不需要闭合

直接暴露在vue实例的组件都是全局组件，可以直接在组件实例中调用

局部组件是使用一个JavaScript对象进行定义封装，例如：


    <div id="app">
        <datas-a></datas-a>
    </div>
    <script>
    const datas ={
        template: `
            <div>
                hallo
            </div>
        `
    }
    const hallo = Vue.createApp({
        components: {
            "datas-a" : datas
        }
    }).mount('#app')
    </script>


components对象，键为自定义元素的名称，值为组件的实质对象


局部组件的属性是不能直接被调用的，但是可以在另一个组件中指向其为自己的子组件，例如：

    const datab ={
        components: {
            "datas-a" : dataa
        }
    }


模块系统

通过导入模块的方式导入组件，例如：

import datas from './datas'

那么datas组件就是可以在当前使用了

---

Props

prop类型

用对象的方式列出prop，并且定义其类型，当传入的prop类型不对就会报错（开发版本），例如：


    const hallo = Vue.createApp({
        component: {
            props:{
                abc: String,
                xyz: Boolean,
            },
            template: `<h1>{{abc.text}}-{{xyz.text}}<h1>`
        }
    }).mount('#app')

如果没有限制类型，那么任何类型的值（能装进变量里的值都行，因为连变量里面的值都可以传入）都可以传给prop

传入数字

<test :nums = "666"></test>

传入布尔值

<test :xyz = "true"></test>

传入数组

<test :array = "[123,987,666]"></test>

传入对象

<test :obj = "{user: 'root',pass: 'root'}"></test>


单向数据流

父组件的prop数据可以流向子组件，但是子组件不能流向父组件，避免因为子组件而修改了父组件

父组件发生变化时，子组件的prop都会刷新为当前最新的值


属性继承

可以在组件中写属性，例如：

    template: \`<div class="main"></div>\`

同样也是可以在组件写事件监听器之类的


禁用属性继承

    inheritAttrs: false,
    template: \`<div class="main"></div>\`

包含未被props接收的参数

this.$attrs

可以根据需求来传入属性，例如：

    <test class="data"></test>

    <div :class="$attrs.class"></div>


---

父子组件之间通过事件进行通信

vue实质上是单向数据流，子组件不能修改父组件传递的数据，不过可以通过事件来进行修改，props传递数据：只能父传子，而不能子传父

因此vue提供了自定义事件来先父组件传递数据，然后父组件监听子组件的事件

自定义事件

如果定义了原生事件，那么组件的事件将会代替原生事件

emits选项可以定义事件


兄弟（非父子）组件数据交互

通过一个事件中心（实质上这是一个发布-订阅模型）来管理组件间的通信

这个事件中心叫Vue事件总线（EventBus）

let hallo = new Vue()


监听事件和销毁事件

hallo.$on("onadd", onAdd)

hallo.$off("onadd")

触发事件

hallo.$emit("onadd", id)


组件插槽


    const app = Vue.createApp({
        template: `
            <div>
                <hallo>
                    <test/>
                </hallo>
            </div>
        `
    })
    app.component('test', {
        template: `<span>hahaha</span>`
    })
    app.component('hallo', {
        // props: ['num'],
        template: `
            <div>hallo word
                 <slot></slot>
            </div>
        `
    })
    const vm = app.mount("#app")

   

<slot></slot>内部的内容就是vuejs，插槽可以表示任何的DOM元素，子组件一样也可以表示

当插槽使用了动态数据时，以根组件的data属性为准

插槽默认值

    app.component('test',{
        template:`
            <div>
                data：<slot><span>hallo word</span></slot>
            </div>
        `
    })

当插槽没有dom元素需要表示，将显示<span>hallo word</span>，当有dom元素表示时，会替换该元素





具名插槽：可以给slot和子组件分别使用name属性，slot属性，当name属性和slot属性相同，那么就匹配

当出现多个插槽时，希望一个插槽代表一个dom元素时，就可以具名插槽了，例如：

    const app = Vue.createApp({
        template: ` 
            <test>
                <template v-slot:a><div>我是第一个</div></template>
                <template v-slot:b><div>我是第三个</div></template>
            </test>
        `
    })
    app.component('test',{
        template:`
            <div>
                <slot name="a"></slot>
                <div>hallo word</div>
                <slot name="b"></slot>
            </div>
        `
    })
    const vm = app.mount("#app")

另外v-slot可以简写成#a



作用域插槽可以访问子组件定义的数据，例如：

    const app = Vue.createApp({
        template: `  
            <test v-slot="props"> 
                {{props.item}}
            </test>
        `
    })
    app.component('test', {
        data() {
            return {
                items: ['hallo', 'hahaha','abc','hallo word','xyz']
            }
        },
        template: `
            <ul>
                <li v-for="(item, index) in items">
                    <slot :item="item"></slot>
                </li>
            </ul>
        `
    })


其中props是子组件传递过来的数据（对象格式）

简化作用域插槽写法

    const app = Vue.createApp({
        template: `  
            <test v-slot={item}> 
                {{item}}
            </test>
        `
    })


动态组件

    const app = Vue.createApp({
        data(){
            return {
                hallo:'test'
            }
        },
        template: ` 
            <keep-alive>
                <component :is="hallo"></component>
            </keep-alive>
        `
    })
    app.component('test', {
        template: `
            <div>hallo word</div>
            <input />
        `
    })

    

决定渲染组件由hallo决定,在上面例子中可以看到hallo指向了test，因此渲染的是test，不需要手动设置子组件，而是动态设置，只需要修改:is指向的值就可以了

而且当使用动态组件时，切换该组件后，该组件的状态将会失活，无法保存状态，需要搭配<keep-alive>使用，保存这些状态，不只是方便保存缓存，而且还可以避免重新渲染


异步组件

    const app = Vue.createApp({
        data(){
            return {
                hallo:'asyncs'
            }
        },
        template: ` 
            <keep-alive>
                <component :is="hallo"></component>
            </keep-alive>
        `
    })
    app.component('asyncs', Vue.defineAsyncComponent(() => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve({
                    template: `<div>hallo word</div>`
                })
            },1000)
        })
    }))
    const vm = app.mount("#app")

可以看到该组件并不会立马渲染，该组件不会在第一次加载渲染时显示，而是当需要用到它时才会渲染，可以优化页面首次渲染性能



provide/inject

当存在多级组件时，祖先组件想传递组件给子孙组件时，如果使用props传递数据的话，需要一层接收然后在传递给后辈，非常麻烦

    const app = Vue.createApp({
        data(){
            return {num: 100}
        },
        provide:{
            haha: 200
        },
        template: `
            <hallo :num="num" />
        `
    })
    app.component('hallo',{
        props:['num'],
        inject:['haha'],
        template:`
            <div>{{num}},{{haha}}</div>
        `
    })

这里只是举个例子，如果是多层组件结构，这个方法也是可以发送和接收到的，只需要provide发送一下数据，然后子组件inject接收一下


---

受控组件和非受控组件



