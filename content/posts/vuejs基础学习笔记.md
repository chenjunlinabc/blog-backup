---
title: "vuejs基础学习笔记"
categories: [ "默认" ]
tags: [ "vuejs" ]
draft: false
slug: "23"
date: "2021-06-16 10:13:00"
---

vuejs的核心层就是只关心视图层的，本笔记使用的是最新版本的vue3

vue全家桶：Vue+VueRouter+Vuex

vue名字来源于法语（中文翻译为视图），可以看出其对视图层的重视

导入一般都是使用cdn导入或者直接下载vuejs进行托管，也可以使用npm安装或者使用官方的CLI来构建一个应用

https://unpkg.com/vue@next


cjs版本：完整版，包含编译器

prod.js都是开发版，代码进行了压缩

global版本：可以直接通过scripts标签导入，会建立一个全局Vue对象

browser版本：包含esm,浏览器模块

bundler版本：该版本不是完整版，min

vuejs模板支持所有JavaScript表达式

vuejs本身是用声明式渲染把数据渲染到html dom中，渲染格式为{{...}}，例如：

    <div id="app">
        <p>{{ hallovuejs }}</p>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    hallovuejs: "hallo vuejs!"
                }
            }
        }
        Vue.createApp(hallo).mount("#app")
    </script>

vue本身携带了一些指令，主要特征就是带有v-前缀，用来渲染html dom上的一些行为，例如：

    <div id="app">
        <a v-bind:href="url">{{ main }}</a>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    url:"https://cjlio.com",
                    main: "小陈的辣鸡屋"
                }
            }
        }
        Vue.createApp(hallo).mount("#app")
    </script>


这个v-bind:href，就是将a标签的href属性绑定，v-bind可以绑定任何属性，例如class属性等等，因此可以在视图层留下一些接口指令，让vuejs来响应式渲染出视图

而vue还提供了可以绑定事件的v-on属性，例如：

    <div id="app">
        <button v-on:click="go">{{ main }}</button>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    main: "hallo"
                }
            },
            methods: {
                go: function(){
                    console.log("hallo vuejs");
                }
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>

v-on可以绑定多个事件，例如：

    <div id="app">
        <button @click="goclick"@dblclick="yesclick">{{ main }}</button>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    main: "hallo"
                }
            },
            methods: {
                goclick:function(event){
                    console.log("hallo vuejs")
                    console.log(event.target) // event.target可以获取到触发该事件的dom元素
                },
                yesclick:function(){
                    console.log("hallo word")
                }
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>






v-model可以进行数据的双向绑定，不但可以赋予元素中的数据，也可以获取元素中的数据，例如：

    <div id="app">
        <input v-model="main">{{ main }}</input>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    main: "hallo"
                }
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>

v-show可以根据表达式的值来判断是否显示，方法和v-if一样，但是隐藏的方法是加上了display: none;，v-show不支持template和v-else，如果没有频繁切换的需求可选择v-if，频繁切换会重新渲染情况严重，而且v-show只是修改css属性而已

v-if可以进行条件判断，会根据表达式的值来判断，例如

    <div id="app">
        <span v-if="yes_no">{{ main }}</span>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    yes_no: true,
                    main: "hallo"
                }
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>

当值为false时，该元素就会被隐藏

v-else 该指令必须跟着v-if或者v-else-if指令的元素的后面，否则不会被识别，例如：

    <div id="app">
        <span v-if="yes_no">{{ main }}</span>
        <span v-else>这是v-if判断为假时渲染，v-if判断为真时不渲染</span>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    yes_no: false,
                    main: "hallo"
                }
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>


v-else-if，必须前面的元素中有v-if或者v-else-if指令，例如：

    <div id="app">
        <span v-if="yes_no == 'yes'">yes_no == yes</span>
        <span v-else-if="yes_no == 'no'">yes_no == no</span>
        <span v-else>yes_no != no||yes</span>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    yes_no: "yes",
                    yes: "yes",
                    no: "no",
                    main: "hallo"
                }
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>

v-for可以多次渲染元素，例如：

    <div id="app">
        <ul>
            <li v-for="a in arr">
                {{a}}
            </li>
        </ul>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    arr:[
                        "hallo",
                        "abc",
                        "xyz",
                        "yes",
                        "no"
                    ]
                }
            }
        }
        Vue.createApp(hallo).mount("#app")
    </script>


注意：当v-for和v-if出现在同级时，v-for优先级比v-if高，因此不要在v-for下，使用v-if过滤，应该在v-if下使用v-for，v-if应该使用<template>（<template>不会创建dom元素）



v-html可以插入html


    <div id="app">
        <span v-html="main"></span>
    </div>
    <script>
        const hallo ={
            data() {
                return {
                    main: "<p>hallo word</p>"
                }
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>

v-once表示该元素或者组件只渲染一次，后面不会因为数据的改变而重新渲染


动态参数的实现

    <div id="app">
    </div>

    const hallo = {
        data() {
            return {
                main: "hallo",
                abc: "classa"
            }
        },
        template: `<h1 :[abc] = "main">hallo word</h1>`
    }
    Vue.createApp(hallo).mount("#app")


vuejs提供了一个叫组件的概念，将重复的程序进行封装，用这些被封装的组件来构建大型应用，可以理解为积木

根组件：挂载应用时，该组件被认为是渲染的起点

如果想挂载一个应用到id为app的dom元素中，那么就需要挂载#app，通过一个vue实例，调用其component()方法，创建一个组件


    <div id="app">
        <hallo v-for="item in groceryList" v-bind:todo="item" v-bind:key="item.id"></hallo>
    </div>
    <script>
        const main ={
            data(){
                return{
                    groceryList: [
                        {id:0,text:"abc"},
                        {id:1,text:"xyz"},
                        {id:2,text:"hallo"}
                    ]
                }
            }
        } 
        const app = Vue.createApp(main)
        app.component("hallo",{
            props: ["todo"],
            template: `<h1>{{todo.text}}<h1>`
        })
        app.mount("#app")
    </script>

上面简单说明了一下组件的概念,下面详细说明一下组件和组件实例

页面是这些组件的容器，因此，组件在页面中表达出来的是原生html

模板（template）：模板定义了数据和html dom的绑定关系

初始数据（data）：组件的初始数据，组件具有作用域，因此是处于封装私有状态的

接收的外部参数（props）：组件之间通过该参数进行数据的传递和分享(单向，只能从父组件中获取，不能进行修改,父传子)

方法（methods）：对数据的改动一般都在组件的方法内进行操作，通过v-on指令把输入事件和组件方法进行绑定

生命周期钩子（lifecycle hooks）：每个实例都会触发多个生命周期钩子，从创建到废弃的过程就是生命周期，可以在不同时候进行逻辑处理，另外还有组件生命周期

组件：一种对数据和方法的封装

mount()返回的是组件实例

组件实例：组件继承于组件，通过createApp()建立组件实例

当需要进行数据计算处理时，例如将两个数据合并，如果通过正常的表达式来表示，那么一旦数据处理复杂了，程序会显得很繁杂，vuejs中提供了一个叫计算属性的东西，例如：

    <div id="app">
        {{hallo}}
    </div>
    <script>
        Vue.createApp({
            data() {
                return {
                   yes:{
                        main: "abc"
                   }
                }
            },
            computed: {
                hallo() {
                    return "hallo"+" "+this.yes.main
                }
            }
        }).mount('#app')
    </script>



样式绑定

    <style>
        .abc{
            width: 100px;
            height: 100px;
            background-color: #ccc;
        }
    </style>

    <div id="app">
    </div>
    <script>
    const app = Vue.createApp({
        data() {
            return {
                classdata: "abc"
            }
        },
        template: `
            <div :class = "classdata">hallo word</div>
        `
    })
    app.mount('#app')
    </script>


组件是可重复使用的实例，例如：

    <div class="app">
        <test></test>
        <test></test>
    </div>

    const app = Vue.createApp({})
    app.component("test",{
        data() {
            return {
                abc: "hallo"
            }
        },
        template: `
            <div>
                {{abc}} word
            </div>`
    })
    app.mount("#app")


全局组件

一般来说组件的数据是独享的，只有全局组件才能进行数据访问，例如：


    const app = Vue.createApp({
        template: `
            <div>
                <test></test>
                <abc><abc>
            </div>
        `
            // 父组件
    })

    app.component("test",{
        template: `<abc><abc>`
            // 全局组件
    })

    app.component("abc",{
        data() {
            return {
                text: "hallo word"
            }
        },
        template: ` <div>{{text}}</div> ` 
            // 全局组件
    })

    app.mount('#app')


abc组件分享了数据给test组件


局部组件

    <test></test>
    <Test_datea></Test_datea>


    const Test_date ={
        template: "<p>hallo word</p>"
        // 局部组件
    }
    const Test_datea ={
        template: "<p>hallo hhh</p>"
        // 局部组件
    }
    const app = Vue.createApp({
        components: {
            "test" :Test_date
            Test_datea
        }
    }).mount('#app')

其中test为组件名，指向了Test_date局部组件，




父子组件之间传递数据

    <div id="app">
        <test :name="text"></test>
    </div>
    <script>
    const app = Vue.createApp({
        data() {
            return {
                 text: "hallo",
            }
        }
    })
    app.component("test",{
        props: ["name"],
        template: `<p>{{name}}</p>`
    })
    app.mount('#app')
    </script>


静态传值只能传字符串类型，如果要传其他数据类型或者想动态传递数据，可以使用v-bind


通过循环来输出一组数据

    <div id="app">
        <test v-for="data in datas" :key="data.id" :href="data.url" :text="data.text"></test>
    </div>
    <script>
    const app = Vue.createApp({
        data() {
            return {
                datas: [
                    {id: 1, url: "https://cjlio.com", text: "hallo"},
                    {id: 2, url: "https://test.cjlio.com", text: "abcxyz"}
                ]
            }
        }
    })
    app.component("test",{
        props: ["text"],
        template: `<a><p>{{text}}</p></a>`
    })
    app.mount('#app')
    </script>



传值校验

 props中的值进行验证，例如：

    <div id="app">
        <test text="123" url="https://cjlio.com" main="yes" ifmain=11></test>
    </div>
    <script>
    const app = Vue.createApp({})
    app.component("test",{
        props: {
            text: String, // 基本类型验证 null和undefined会通过任何类型验证，如果不是这个类型，将返回警告
            url: [Number,String], // 多个类型验证，当数据不是这的类型
            main: {
                type: String, // 类型String，必填，如果设置没有该值
                required: true
            },
            hallo: {
                type: String, //  类型String，带默认值default
                default: "hallo vuejs"
            },
            obj: {
                type: Object, // 类型Object，带默认值函数
                default: function(){
                    return{
                        text: "abc"
                    }
                }
            },
            ifmain: { 
                validator: function(value){
                    return(value>10)  // 验证函数
                }
            }
        },
        template: `
            <p>{{text}}</p>
            <p>{{url}}</p>
            <p>{{main}}</p>
            <p>{{hallo}}</p>
            <p>{{obj}}</p>
            <p>{{ifmain}}</p>
        `
    })
    app.mount('#app')
    </script>
        






---

vuejs事件修饰符

.stop：阻止冒泡事件的发生，只触发自身的事件，一个子元素定义了事件，但是它的父元素也定义了一个事件，而且这两个事件的触发机制还是一样的，那么就会导致这两个事件都触发，而想避免发生就需要在目标元素上的事件加上.stop，例如：


    <div id="app" @click = "divclick">
        <input type="button" value="GO" @click.stop = "inputclick">
    </div>

.prevent：拦截默认事件的发生，有一些标记拥有自身的默认事件，例如a标记，例如：

    <div id="app">
        <a href="https://cjlio.com" @click.prevent = "alinkclick">{{data}}</a>
    </div>
    <script>
        const hallo ={
            data(){
                return{
                    data: "GO"
                }
            },
            methods: {
                alinkclick: function(){
                    console.log("hallo")
                },
            },
        }
        Vue.createApp(hallo).mount("#app")
    </script>

这里阻止了a标记的默认事件


.capture：捕获事件，当发生冒泡事件时，先触发带有该修饰符的，如果有多个，则从父元素到子元素触发

触发机制：优先触发带有.capture的，然后再按照从内往外的冒泡


.self：只有当事件在该元素上时才触发，冒泡和捕获都无法让其触发事件，只要当事件发生在该元素本身才会触发

.once：指定该绑定的事件只会触发一次


.stop和.self的区别：.stop会阻止全部冒泡事件，而.self只阻止自身的冒泡


.passive：每一次发生事件，浏览器都去查询有没有preventDefault来阻止该事件的默认行为，.passive就是来声明，没有使用preventDefault来阻止该事件的默认行为，因为其是告诉没有使用阻止事件的默认行为，所有passive是和prevent冲突，不能一起使用，否则.prevent会被忽略

因为在移动端，一般都是监听滚动事件比较多，而每移动一次都会触发一次事件，会进行查询有没有使用prevent，可能会导致滑动卡顿，使用passive能有效提升移动端的性能和流畅度



---

生命周期钩子函数会在某一些特定的时刻自动触发，方便在该时刻完成一些工作

vue实例生命周期


vue实例的创建，运行，到销毁期间，会触发一些叫生命周期钩子的函数，可以通过该来监听数据变化

按照生命周期排序：

beforeCreate：实例刚刚初始化之后，数据观察和事件机制（data 和 methods）都还未初始化，不能获取dom节点

created：数据观察和事件机制（data 和 methods）都已经初始化，dom节点处理完成，组件未加载

beforeMount：实例已经加载完毕，但是还没有执行挂载操作，得不到具体的DOM元素

mounted：实例已经加载完毕，也执行了挂载操作，DOM已被渲染出来

beforeUpdate：处于数据更新之前，data中的值是最新的，但是页面上还是旧的数据，还没有重新处理dom节点

updated：处于数据更新之后，data中的值和页面上的数据都已经更新，dom节点也被重新处理

beforeUnmount：实例销毁之前，实例还是在可用状态，可使用this获取实例

unmounted：实例销毁后，数据绑定和事件监听器全部清除，子实例销毁


例如：

    <div id="app"></div>


    const app = Vue.createApp({
        data(){
            return{
                message: 'hallo word'
            }
        },
        beforeCreate() {
            console.log("beforeCreate")
        },
        created() {
            console.log("created")
        },
        beforeMount(){
            console.log("beforeMount")
        },
        mounted() {
            console.log("mounted")
        },
        beforeUpdate() {
            console.log("beforeUpdate")
        },
        updated() {
            console.log("updated")
        },
        beforeUnmount() {
            console.log("beforeUnmount")
        },
        unmounted() {
            console.log("unmounted")
        },
        template: "<h2>{{message}}</h2>"
        
    })
    let main = app.mount("#app")
    setTimeout(()=>{
        main.$data.message = 'abc' // 挂载3秒后触发更新事件
    },3000)
    setTimeout(() => {
        app.unmount() // 6秒后触发销毁事件
    }, 6000)










访问组件的生命周期

vue组件周期

实例生命周期加on就是组件周期

beforeCreate和created统一为setup()

beforeMount为onBeforeMout，mounted为onMounted，beforeUpdate为onBeforeUpdate，updated为onUpdated，beforeUnmount为onBeforeUnmount，unmounted为onUnmounted



setup：data 和 method 都已经初始化

onBeforeMount：组件被挂载到dom节点之前

onMounted：组件被挂载到dom节点完毕

onBeforeUpdate：组件更新之前

onUpdated：组件更新之后

onBeforeUnmount：组件销毁之前

onUnmounted：组件销毁完毕之后

onErrorCaptured：当捕获到来自子孙组件的异常时

onRenderTracked：当虚拟DOM重新渲染时调用

onRenderTriggered：当虚拟DOM重新渲染被触发时调用

onActivated：当被包含在<keep-alive>中的组件时

onDeactivated: 当被包含在<keep-alive>中的组件发生改变时（例如切换组件，原来的组件销毁时）



注意：使用<keep-alive>的组件会把数据保留在内存中，避免需要重新加载多次数据






---

实例的data()是一个函数，并非方法，vue在创建vue的组件实例过程中会调用该函数，以$data的形式进行存储在vue实例中，data中的所有值都可以通过实例来暴露（访问或者修改），例如：

    const hallo = Vue.createApp({
        data() {
            return {
                hallovuejs: "hallo vuejs!"
            }
        }
    }).mount("#app")
    console.log(hallo.$data.hallovuejs)
    console.log(hallo.hallovuejs)


---

方法

vue允许使用methods向组件实例添加方法

vue会自动为methods绑定this，使其始终指向组件实例

methods一样可以在组件模板中暴露，也可以在模板中用做事件监听

<div @click="alinkclick"></div>

---

计算属性

计算属性和方法的区别就是：计算属性只在其依赖发生变化才重新计算，只要依赖未发生改变，那么就会直接从缓存中获取，不会再执行其函数

如果使用一个方法来计算时，那么data()返回的值，发生一次改变，事件方法就会重新计算一次，因此涉及计算类的使用计算属性，而不是方法

例如：

    const app = Vue.createApp({
        data() {
            return {
                hallovuejs: "hallo vuejs!"
            }
        },
        computed: {
            hallo(){
                return this.hallovuejs == "hallo vuejs!" ? "Yes" : "No"
            }
        }
    }).mount("#app")


---

watch监听器

watch接收2个参数，被监听的data()属性值，回调函数，当监听的变量发生改变时，自动执行回调函数，例如

    const app = Vue.createApp({
        data() {
            return {
                num: 100
            }
        },
        watch: {
            num(current, prev) {
                console.log('num发生改变')
                console.log("改变之后的值:", current)
                console.log("改变之前的值:", prev)
            }
        }
    })
    let vm = app.mount("#app")
    vm.num = 300
    vm.$data.num = 600

另外一种使用方法


    const count = ref(100) 
    watch(count,(current, prev) => {
        console.log('num发生改变')
        console.log("改变之后的值:",current)
        console.log("改变之前的值:",prev)
    })


watch和计算属性的区别：虽然都可以监听data()属性值的改变，但是计算属性会在页面渲染时触发一次，而watch只在被监听的发生改变时触发，而且watch可以得到改变之后的值和改变之前的值

watch可以监听多个值




---

class绑定


vue允许使用:class对象来动态切换class，例如：


    <div id="app">
        <p :class="{container: yes,nav: no}"></p>
    </div>
    
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                yes: true,
                no: false
            }
        }
    }).mount('#app')
    </script>

而class的存在取决于键值对的值，允许传入多个值，也可以直接调用对象，例如：

    <div id="app">
        <p :class="classnav"></p>
    </div>
    
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                classnav: {
                    container: false,
                    nav: true
                }
            }
        }
    }).mount('#app')
    </script>

vue也允许通过数组来传入class，例如：


    <div id="app">
        <p :class="[classnav,classa]"></p>
    </div>
    
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                classnav: "nav",
                classa: "texta"
            }
        }
    }).mount('#app')
    </script>


同样也允许使用三元表达式，例如：

    <div id="app">
        <p :class="[no ? classnav: '',classa]"></p>
    </div>
    
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
               no: false,
               classa: "texta"
            }
        }
    }).mount('#app')
    </script>

始终添加classa，classnav取决已经no的值

vue也是允许在数组中使用对象来处理

如果在组件中已经绑定了class，而在调用其又加上其他class，那么就会合并，vue允许在组件中进行:class，如果有多个元素，在调用时绑定class，但是又想指定其class给予某个，可以使用$attrs组件属性，例如：


    <div id="app">
        <test class="nav"></test>
    </div>
    <script>
    const hallo = Vue.createApp({}).component("test",{
        template: `
            <div>hallo</div>
            <div :class="$attrs.class">vuejs</div>
            `
    }).mount('#app')
    </script>


上面只有<div>vuejs</div>接收到class

---


内联样式绑定（:style）

例子：

    <div id="app">
        <div :style="{ color: a, margin: b + 'px' }"></div>
    </div>
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                a: "#ccc",
                b: 300
            }
        },
    }).mount('#app')
    </script>


多重值

    <div id="app">
        <div :style="{ color: ['#ccc','#fff','#000']}"></div>
    </div>

一般来说只会选择最后一个（前提是浏览器支持）

---


列表渲染

v-for可以将一个数组迭代成一组元素

使用的形式是 item in items，items为数组，item为迭代的元素，例如：

    <div id="app">
        <div v-for="item in items" :key="item.message">
            {{ item.message }}
        </div>
    </div>
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                items: [{message: "xyz"},{message: "abc"},{message: "hallo"}]
            }
        },
    }).mount('#app')


而:key的作用是为了更加效率的渲染dom，更快判断出某个不存在的元素，可以根据key动态来重新排列元素的顺序，key的目的是保证唯一性

v-for支持index参数，即当前的索引，例如：

    <div id="app">
        <div v-for="(item,index) in items">
            {{ index }}:{{ item.message }}
        </div>
    </div>
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                items: [{message: "xyz"},{message: "abc"},{message: "hallo"}]
            }
        },
    }).mount('#app')
    </script>



in可以用of替代<div v-for="(item,index) of items">

v-for也可以使用对象(从某个角度来看，数组也是对象的一种)，例如：

    <div id="app">
        <div v-for="(item,key) of items" >
            {{ key }}:{{ item }}
        </div>
    </div>
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                items: {
                    hallo: "yes",
                    abc: "2002",
                    xyz: "np"
                }
            }
        },
    }).mount('#app')

第二参数为键名，这里命名为key，实质上就是一个key

也可以加index做为第三个参数，作为索引值

vue在渲染元素时，如果数据的顺序发生改变，vue是不会将dom元素进行适配顺序，而是重新更新数据，确保正确渲染

v-for也支持整数

<div v-for="a in 6" :key="a"></div>

和遍历一样，会重复对应的次数

---


事件处理


一般用v-on进行监听事件，例如：<div v-on:click="boom"></div>

多个事件用逗号,分开

可以简写为@click="boom"，boom是方法，不推荐直接将逻辑写在事件v-on，而是使用一个方法来调用，例如：

    <div id="app">
        <div @click="boom('yes')">
            hallo
        </div>
    </div>
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                abc: "hallo"
            }
        },
        methods: {
            boom(a){
                console.log(this.abc + "boom")
                console.log(a)
            }
        }
    }).mount('#app')



按键修饰符

一般用于监听键盘事件，精确的知道某个按键是否被触发

.enter：顾名思义 回车键

.tab：tab键

.delete：删除或者退格

.esc：esc键

.space：空格键

.up：上键

.down：下键

.left：左键<

.right: 右键>

系统修饰符

.ctrl：ctrl键

.alt：alt键

.shift：shift键

.meta：在win为windows键，在mac为command键

例如：

<input @keydown.enter="submit" />

上面只要触发一个要求就会被触发

.exact：精确触发

@keydown.enter.exact // 只有当enter被按下时触发

@keydown.exact // 只有在没有任何系统修饰符被按下才触发


鼠标修饰符

@click.left：鼠标右键

@click.right：鼠标右键

@click.middle：鼠标中键


例如：




    <div id="app">
        <test class="nav"></test>
    </div>
    <script>
    const hallo = Vue.createApp({}).component("test",{
        methods: {
            entersubmit(){
                console.log('回车事件已触发')
            }
            clickmouse(){
                console.log('鼠标中键事件已触发')
            }
        }
        template: `
            <input @keydown.enter.exact="submit" />
            <div @click.middle = "clickmouse">hallo word</div>
            `
    }).mount('#app')
    </script>




---


自定义指令（directive）

vue允许指定义指令，例如：

    <div id="app">
        <input type="text" v-hallo>
    </div>
    <script>
    const app = Vue.createApp({})
    app.directive("hallo",{
        mounted(abc) {
            abc.focus()
        },
    })
    app.mount('#app')
    </script>

这里的abc指定的是当前元素

局部指令：组件中接受directives选项

指令的钩子函数

created：当绑定元素的属性和事件监听器被应用之前调用

beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用

mounted：绑定元素的父组件被挂载后调用

beforeUpdate：更新包含组件的VNode之前调用

updated：包含组件的VNode及其子组件的VNode更新后调用

beforeUnmount：在卸载绑定元素的父组件之前调用

unmounted：当指令与元素解除绑定，并且父组件也被卸载时调用一次


---

表单输入绑定

v-model可以用于数据的双向绑定，但是v-model会忽略表单元素的初始值，而使用实例中的数据作为数据源（需要在data中声明初始值）

复选框的数据绑定返回的是布尔值，也是可以将数据绑定到一个数组中，例如：

    <div id="app">
        <input type="checkbox" value="hallo" v-model="datas"/>
        <label for="hallo">hallo</label>
        <input type="checkbox" value="abc" v-model="datas"/>
        <label for="abc">abc</label>
        <input type="checkbox" value="xyz" v-model="datas"/>
        <label for="xyz">xyz</label>
        <p>
            data: {{datas}}
        </p>
    </div>
    <script>
    const hallo = Vue.createApp({
        data() {
            return {
                datas: []
            }
        },
    }).mount('#app')
    </script>



单选框（radio），数据存放在字符串中，输出的也是字符串（选择框和输入框也是字符串）

如果想输入或者选择返回的不是字符串，而是其他类型，可以使用v-model.number，当然也是可以进行手动转类型，但是v-model.number可以自动判断输入是否是数字还是字符串

true-value和false-value可以在被选中或者取消选中时触发，例如：


data: {{datas}}
<input type="checkbox" v-model.lazy="datas" true-value="halloword" false-value="hahahah"/>

v-model.lazy是v-model数据双向绑定的懒加载，在每次input事件（失去焦点）触发后将输入框的值与数据进行同步

v-model.trim可以自动过滤掉输入数据的首尾空格

---


组件

全局注册

app.component


局部注册

const componentA = {}

  components: {
    'component-a': componentA,
  }


局部注册的组件，在其子组件是不可用的

通过Props传递数据给子组件

通过Props共享一些数据，例如：


    app.component('titles', {
        props: ['title'],
        template: `<h3>{{ title }}</h3>`
    })

    <titles title="hallo"></titles>

在上面的例子里，title="hallo"被传递数据给模板，任何值的可以传递给props，如果一个值被传递给props属性，那么这个值就成了这个组件实例的共享数据，这个值可以在模板中访问


监听子组件事件

使用自定义事件监听子组件，通过子组件事件来告诉父组件应该做什么，例如：

    template: `
        <button @click="$emit('hallomain')">
            app
        </button>
    `

    <div :style="{fontWeight: yes}">
        <hallo @hallomain="yes += 1"></hallo>
    </div>

也可以在组件的emits中列出事件
    
    app.component({
        emits: ["hallomain"]
    })


事件也可以用来返回一个值
    <button @click="$emit('hallomain',1)">
        app
    </button>

    <hallo @hallomain="yes += $event"></hallo>


这个值可以使用$event访问到，事件也可以是指向方法的，用事件来触发方法的使用，例如：@hallomain= "hallo"

给这个方法定义一下，在methods选项


组件也可以使用v-model（model-value）


    <hallo :model-value="hallomain" @update:model-value="hallomain = $event"></hallo>


如果是表单元素，例如input也想用自定义事件，需要将value绑定到props上，当input事件被触发时，就会将value通过自定义事件抛出，例如：

    app.component({
        props: ["valueData"],
        emits: ["hallomain"],
        template: `
            <input :value="valueData" @input="$emit("hallomain",$event.target.value)">
        `
    })



vue允许向父组件传递给子组件一些内容，例如：

  template: `
    <div class="hallo">
      <div>hallo</div>
      <slot></slot>
    </div>
  `
    <hallo>vuejs</hallo>


通过<slot>vue的自定义元素来插入，父组件的vuejs插入到slot自定义元素中

如果一个子组件定义了多个slot元素，可以使用name属性和slot属性进行分配，例如：
<slot name="main"></slot>

<div slot="main"></div>

发现name属性值和slot属性值相同时，就会分配该插槽到该元素中



单向数据流：父组件可以修改子组件数据，但是子组件不能修改父组件的数据

单向数据流的目的是确保组件的独立性，不然多个子组件都可以乱改父组件的数据，导致不知道按哪个数据为准了

如果想做到类似修改父组件，可以向获取父组件的数据到自己的data数据中，然后赋值给一个属性，通过修改该属性来做到类似修改了父组件的情况（实质上并没有影响到父组件），例如:


    app.component('hallo', {
        props: ['num'],
        data(){
            return{
                num: this.value
            }
        }
        template: `<div>{{num}}</div>`
    })


在上面的例子中，value就是父组件的数据，将其获取到num中，然后就可以改变num了



Non-Props：当父组件发送一个参数给子组件，但是子组件没有接收到这个参数时，子组件会原封不动的复制到自己的属性上的特性，例如：

    const app = Vue.createApp({
        template: `
            <div>
                <hallo num='hahaha'/>
            </div
        `
    })
    app.component('hallo', {
        // props: ['num'],
        template: `<div>hallo word</div>`
    })
    const vm = app.mount("#app")

可以利用Non-Props的特性，直接在父组件下，给子组件定义一些属性，例如style，class等等，只要子组件没有props接收该数据，都会原封不动的应用在自己的属性中


如果不想使用Non-Props的特性，又不想被影响，可以使用inheritAttrs属性

    app.component('hallo', {
        inheritAttrs: false,
        template: `<div>hallo word</div>`
    })



$attrs属性，当一个组件内部是多重嵌套，或者没有根时（指有多个同级元素），拥有该属性的元素才可以获取到利用Non-Props的特性传递的属性，实现属性穿透，而不用再手动传递了


    const app = Vue.createApp({
        template: `
            <div>
                <hallo num='hahaha'/>
            </div
        `
    })
    app.component('hallo', {
        template: `
            <div v-bind="$attrs">
                <div>
                    <div>
                        <div></div>
                        <div v-bind="$attrs"></div>
                        <div></div>
                    </div>
                </div>
                <div v-bind="$attrs">
                    hallo word
                </div>
                <div>
                    hallo hahaha
                </div>
            </div>
        `
    })

在上面这个例子中，只有带有$attrs属性的元素才能获取该传递的属性














