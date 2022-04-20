---
title: "深入学习vuejs"
categories: [ "学习" ]
tags: [ "vuejs" ]
draft: false
slug: "75"
date: "2021-08-07 23:25:00"
---

Vue CLI是vue官方脚手架，可以快速创建vue项目

安装vue-cli3

npm install -g @vue/cli

或者

yarn global add @vue/cli



升级

npm update -g @vue/cli

或者

yarn global upgrade --latest @vue/cli



创建vue项目

vue create vue-demo

如果提示vue : 无法将“vue”项识别为 cmdlet、函数、脚本文件或可运行程序的名称，可以使用npx vue create vue-demo

default (babel, eslint)  //  默认选项，包含babel和eslint

Manually select features //  自定义创建配置工程



一般比较常用的有Babel，TypeScript，Router，CSS Pre-processors，Linter / Formatter


空格为选择，enter为下一步


跑vue项目

npm run serve

或者

yarn start/yarn run dev


如果运行报错error  Component name "Home" should always be multi-word  vue/multi-word-component-names

只需要在vue.config.js添加lintOnSave: false配置，例如：

    module.exports = defineConfig({
        transpileDependencies: true,
        lintOnSave: false
    })

这个问题的原因在于vue-cli默认开启了ESLint检测，关闭就好了


---

Vue-router是vue官方推出的vue官方路由管理工具，和vue核心深度集成

该Vue-router路由被用于单页面应用，组件的切换，而不想其他那样可以使用超链接来切换页面


安装router

npm install vue-router --save-dev

或者

vue add router

例如：


App.vue

    <template>
        <router-link to="/Home/Index">hallo index</router-link>
        <router-link to="/Hallo">hallo</router-link>
        <router-view></router-view>
    </template>

router.js

    import {createRouter, createWebHashHistory} from "vue-router"
    import Hallo from './components/Hallo'
    import Home from './components/Home'
    import Index from './components/Index'      
    const routes = [ 
    {
        path: "/Home",         
        name: "Home",     
        component: Home,
        children: [
            {path:"/",component: Home},
            {path:"Index",component: Index}
        ]
    },
    {
        path: "/Hallo",
        name: "Hallo",
        component: Hallo
    }
    ]
    const router = createRouter({
        history: createWebHashHistory(),
        routes: routes
    })
    export default router

main.js


    import { createApp } from 'vue'
    import App from './App.vue'
    import router from './router'
    createApp(App).use(router).mount('#app')



<router-view>为路由匹配到组件（路由视图），它会表示渲染出使用<router-link>切换的组件

path是链接路径，name为链接名称，component为导入的组件模板名，多个地址用逗号分开，children为子路由声明，


带参数的动态路由

User.vue

    <template>
        <div>
            {{ $route.params.id }}
        </div>
    </template>

router.js

  {
      path: "/User/:id",
      name: "User",
      component: User
  }


访问http://localhost:8080/#/User/admin

可以看页面显示了admin，说明通过路由传递了参数到模板中，其中id的参数在模板是以$route.params.id方式获取的

去掉#：因为我这里vue-router路由模式设置为hash（createWebHashHistory），需要开启history（createWebHistory）

    import {createWebHistory} from "vue-router"
    const router = createRouter({
        history: createWebHistory(),
        routes: routes
    })


获取获取以什么开头的路由

    {
        path: '/User:pathMatch(.*)*',
        name: 'User',
        component: User
    },

可以看到使用了路由，例如http://localhost:8080/Useraaa/等等，只要是Userxxx以及Userxxx/xxx都会渲染User组件

path: '/User:afterUser(.*)',这个效果和上面一样


当Url需要纯数字时：
    
    {
        path: '/User/:id(\\d+)',
        name: 'User',
        component: User
    },


http://localhost:8080/User/666


复用参数（0个或多个用*，1个或多个用+）


在上面的基础上改：

    {
        path: '/User/:id(\\d+)+',
        name: 'User',
        component: User
    },


访问http://localhost:8080/#/User/666/111，可以看到，模板的{{ $route.params.id }}，渲染出[ "666", "111" ]这样的数组，而不是字符串了，因此可以使用$route.params.id[1]，来精确获取第几个路由



嵌套路由（User路由中使用了Admin路由，子路由）

User.vue

    <template>
        <div>
            {{ $route.params.id }}
            <router-view></router-view>
        </div>
    </template>

UserAdmin.vue

    <template>
        <div>
        hallo admin
        </div>
    </template>

router.js

    {
        path: '/User/:id(\\d+)+',
        name: 'User',
        component: User,
        children: [
          {
            path: 'admin',
            component: UserAdmin
          }
        ]
    },


访问http://localhost:8080/#/User/666/admin，可以看到hallo admin



编程式路由（非router-link）

App.vue

    <template>
      <div @click="goAdmin">go</div>
      <br/>
      <router-view></router-view>
    </template>
    <script>
      import {useRouter} from 'vue-router'
      export default{
        setup(){
          const router = useRouter()
          let goAdmin = () =>{
            router.push("/User/123")
          }
          return{
            goAdmin
          }
        }
      }
    </script>


可以看到点击了<div @click="goAdmin">go</div>，就会跳转到http://localhost:8080/User/123

除了通过字符串外，还可以router.push({path: '/User/123'})

传递参数（模板{{ $route.params.id }}）

params方式

router.push({name: 'User', params: { id: 666 }})

访问http://localhost:8080/User/666

query方式

router.push({path: '/User', query: { id: 123 }})

访问http://localhost:8080/User?id=123

query方式获取参数，需要使用{{$route.query.id}}，而不是$route.params.id了

hash方式

router.push({ path: '/User', hash: '#admin' })

访问http://localhost:8080/User#admin

使用{{$route.hash}}获取参数

动态路由参数

let user = 'admin'
router.push({ name: 'User', params: { user } })

replace（使用该跳转，将不会创建会话历史记录，会替代上一个会话的记录）

router.replace({ path: '/User'})

使用replace: true参数也是可以给push设置（push的replace默认为false）

router.push({ path: '/User', replace: true })


会话历史记录跳跃

前进n条记录（前进2页）
router.go(2)

返回n条记录（后退2页面）

router.go(-2)


例如：

App.vue

    import {useRouter} from 'vue-router'
    export default{
      setup(){
        const router = useRouter()
        let goAdmin = () =>{
          router.go(2)
        }
        return{
          goAdmin
        }
      }
    }


路由的name属性的作用：


除了像这样用外<router-link to="/User:id">Go</router-link>

还可以用name属性指向一个路由的path，防止路由地址写错，设置params属性给模板接收参数（这个参数不会作用于URL），<router-link :to="{ name: 'User',params: { id: 'admin' }}">Go</router-link>


当需要在同级下渲染多个router-view，如何确保其router-view渲染的是自己想要的，利用router-view的name属性（该属性默认值为default）

App.vue

<router-view name="a"></router-view>
<router-view name="b"></router-view>
<router-view name="c"></router-view>


router.js

    import User from './components/User.vue' 
    import A from './components/a.vue'   
    import B from './components/b.vue'   
    import C from './components/c.vue' 
    const routes = [ 
        {
            path: '/User',
            name: 'User',
            components:{
              default: User,
              a:A,
              b:B,
              c:C
            }
        }
    ]
    const router = createRouter({
        history: createWebHashHistory(),
        routes: routes
    })
    export default router


访问http://localhost:8080/User，可以看到渲染出了a组件，b组件，c组件

其中router.js的小写a就是指向了router-view的name属性，大写的A就是组件，和name属性对应上（键值对），将其对应渲染到相应的router-view中

components属性为多组件配置


重定向（当希望访问到某个路由时，能跳转到另一个路由时）

    const routes = [ 
        {
            path: "/Home",         
            name: "Home",
            redirect: '/',  
            components: {
                default: Home
            }
        }
    ]

访问http://localhost:8080/Home

设置还可以指向一个路由的name属性，const routes = [{ path: '/Home', redirect: { name: 'User' } }]

另外如果一个路由使用了redirect重定向，那么是可以不用写component了，因为不会真正访问到（访问到就重定向到另一个路由了），但是存在子路由时就需要写component了

当重定向发生时，也需要传递参数到重定向时，例如：

    {
        path: '/User/:id',
        name: 'User',
        components:{
          default: User,
        } ,
        redirect: to =>{
          return { path: '/', query: { id: to.params.id } }
        }
    }

当访问http://localhost:8080/User/123时，会重定向到http://localhost:8080/?id=123，如果/User/123的123，被传到query了，另外还可以在重定向的目标组件模板用{{$route.query.id}}获取到该参数


重定向别名


    {
        path: "/Home",         
        name: "Home",
        components: {
            default: Home,
        },
        alias: '/User',
    }

别名就是访问http://localhost:8080/User/时，实质上是访问/Home这个路由，和redirect的区别就是，redirect会改变URL，而alias不会改变URL

利用子路由来给多路由匹配组件

    {
        path: "/Home/:id",         
        name: "Home",
        component: Home,
        children: [
            { 
                path: '', 
                component: Homea, 
                alias: [':id','admin', '/User'] 
            },
        ]
    }

当访问http://localhost:8080/Home/123，http://localhost:8080/Home/admin，http://localhost:8080/User时，渲染的是Home组件


路由props传参（通过props接收参数）

routes.js

    {
        path: '/hallo/:id',
        component: Home,
        props: true
    }

Home.vue

    <template>
        <div>
            hallo {{id}}
        </div>
    </template>
    <script>
    export default {
        props: ['id']
    }
    </script>


当props为true时，其route.query（router.params）将被设置为该组件的属性，可直接访问其参数

对于有命名视图的路由，必须为每个命名视图定义props配置，例如：


    {
        path: "/Home/:id",         
        name: "Home",
        components: {
            default: Home,
            index: Index
        },
        props:{
            default: true, 
            index: false
        },      
    },

在上面例子中props是对象形式，可以看到index组件就算props接收了id参数，但是因为其路由props设置了false，导致其route.query（router.params）没有设置为该组件的属性，因此没有id参数

routes.js

    {
        path: "/Home",         
        name: "Home",
        component: Home,
        props: (route) => ({id : route.query.id }) 
    },

Home.vue

    <template>
        <div>
            hallo {{id}}
        </div>
    </template>
    <script>
    export default {
        props: ['id']
    }
    </script>

访问http://localhost:8080/Home?id=123，Home输出123

上面例子中route.query.id就是当前query（id=123），将其传入到id中，然后模板接收


注意：函数模式请不要用于多组件components中，在多组件components中，需要给除default外的组件设置router-view的name属性



导航守卫（导航守卫实质上就是路由跳转时的钩子函数，守卫是异步执行的）

全局前置守卫：beforeEach

router.beforeEach((to,form)=>{ return false})

守卫接收2个参数，to是跳转的目标路由，form是跳转离开的路由，返回值如果为false时，表示取消当前导航，当返回值为路由地址时，会跳转到该路由地址，还有一个next()可选参数，该可选参数表示放行

当next(false)时，表示取消当前导航，当next('/Home')时，会跳转到/Home（和router.push类似）

如果守卫抛出Error时，取消导航并且调用router.onError()

守卫正常时是undefined或返回true

例如：

routes.js

    const router = createRouter({
        history: createWebHistory(),
        routes: routes
    })
    router.beforeEach((to, from, next) => {
        if (to.name !== "Home") {
            next({
                name: 'Home'
            })
        } else {
            next()
        }
    })


像上面的例子中，全局路由守卫将会监听要跳转的路由的name属性是否为Home，如果不是Home就会调用next()方法跳转到name为Home的路由，如果是Home的话守卫放行

全局解析守卫：beforeResolve

全局解析守卫和全局前置守卫类似，都是路由跳转前调用，但是beforeResolve会在beforeEach（全局前置守卫）和beforeRouteEnter（组件路由守卫）之后触发，afterEach（全局后置守卫）之前触发

    router.beforeResolve((to, from, next) => {
        if (to.name !== "Home") {
            next({
                name: 'Home'
            })
        } else {
            next()
        }
    })

可以看到效果和beforeEach一样


全局后置守卫：afterEach（没有next()，全局后置守卫顾名思义，就是路由跳转后触发）

    router.afterEach((to, from) => {
        console.log(to.name)
        console.log(from.name)
    })


路由独享的守卫beforeEach（在路由内部定义的beforeEach守卫）

    {
        path: "/Home",         
        name: "Home",
        component: Home,
        beforeEach: (to,from) =>{
            if (to.name !== "Home") {
                next({
                    name: 'Home'
                })
            } else {
                next()
            }
        }
    },

路由独享的守卫beforeEach只在进入路由时触发（params，query，hash改变并不会触发）

组件路由守卫：beforeRouteEnter，beforeRouteUpdate，beforeRouteLeave

beforeRouteEnter：在渲染该组件时对应的路由被确定前触发，在该守卫触发时，组件实例未被创建，因此不能获取组件实例的任何东西

beforeRouteUpdate：当前路由发送改变，但是该组件又被复用时触发，比如传参，只是改变参数，但是复用了当前组件，该守卫触发时，组件实例已被挂载，可访问组件实例

beforeRouteLeave：当导航离开渲染该组件的路由时触发，可访问组件实例


例如：

Home.vue

    export default {
        beforeRouteEnter(to, from, next) {
            next(vm =>{
                console.log(vm) // 可以看到vm是组件实例，说明可以通过next来访问组件实例，next()会在导航被确认时被回调
            })
        },
        beforeRouteUpdate(to, from, next) {
            if (to.name !== "Home") {
                next({
                    name: 'Home'
                })
            } else {
                next({
                    name: 'User'
                })
            }
            console.log(to.name)
            console.log(from.name)
        },
        beforeRouteLeave(to, from, next) {
            if(to.name !=="Home"){
                if(confirm('您的信息还未保存，确定要离开吗？') == true){
                    next()
                }else{
                    next(false)
                }
            }
            console.log(to.name)
            console.log(from.name)
        },
    }

路由元信息（路由meta字段属性）


    {
        path: "/Home",         
        name: "Home",
        component: Home,
        meta: { ha: false }
        }
    },

    router.beforeEach((to, from, next) => {
        if (to.name == 'Home' && to.meta.ha){
            next()
        }else{
          next(false)
        }
    })


路由懒加载（动态导入）

    const User = () => import('./components/User.vue')
    const routes = [{ 
        path: '/User', 
        name: 'User'
        component: User 
    }]


动态路由

    const routes = [{ 
        path: '/:id', 
        name: 'User'
        component: User 
    }]
    const router = createRouter({
        history: createWebHistory(),
        routes: routes
    })
    router.addRoute({ path: '/Home',name: 'Home', component: Home }) // 添加路由
    router.removeRoute('Home') // 删除路由


注意：当路由被删除时，路由别名和子路由同时也被删除

不只是在路由router中设置，还可以在路由守卫设置，而且当2个addRoute路由的name重名时，也会触发删除路由，当路由没有name属性时，可通过定义回调函数，让其重新添加路由（实质上是删除路由，因为重复了）,例如：

    const RoutesData = router.addRoute(path: '/Index', component: Home )
    RoutesData()



嵌套路由

    router.addRoute({  path: '/User', name: 'User', component: User })
    router.addRoute('User', { path: 'Admin', component: Home })



---


vue中解决xss脚本攻击（依赖于xss模块）

npm install xss --save

引用xss模块

    import xss from 'xss'
    Object.defineProperty(Vue.prototype, '$xss', {
        value: xss
    })


在评论框或者其他输入框等等要针对免疫xss地方使用$xss()方法

自定义拦截规则

在data字段，return字段下设置白名单，格式就是标签名加属性，只要不是白名单的标签和属性，就是会被过滤掉

    options : {
        whiteList: {
            a: ['href', 'title'],
            div: ['class']
          }
    }


---


vue中解决html和Markdown转换

MarkdownToHtml（处理md转换为html）

npm install showdown --save

使用方法：

    import showdown
    let converter = new showdown.Converter()
    let text = '# hallo word' // md格式的文本
    let html = converter.makeHtml(text)



HtmlToMarkdown（处理html转换为md）

npm install turndown --save

使用方法：

    import turndown
    let turndownService = new turndown()
    let markdown = turndownService.turndown('<h1>hallo word</h1>')


---

vuejs是声明式视图层框架，视图层框架分为命令式和声明式两种，命令式关注过程，声明式关注结果，命令式的代码描述了工作过程，而声明式的代码只声明，结果由框架来提供

vuejs内部实现肯定是命令式的，但是进行封装了工作过程，对外暴露的是声明式，声明式的性能并没有命令式的性能优，因为命令式的工作流程一目了然，去除或者添加某个功能，理论上能做到最优性能

而声明式只关注了结果，需要查找前后代码的差异（虚拟dom+diff算法），然后再更新，vuejs选择声明式的方案是为了可维性，不需要关心工作过程，只需要关注结果就好了



虚拟DOM对比innerHTML操作DOM

创建DOM：虚拟DOM和innerHTML操作DOM都是创建所有DOM元素，因此虚拟DOM和innerHTML操作DOM基本上性能是一致的（DOM层计算）

更新DOM：innerHTML操作DOM更新是先重新创建HTML字符串，然后再更新DOM元素（销毁旧的DOM元素，重新创建一个完整的DOM元素），而虚拟DOM是通过创建新的JavaScript对象，然后前后俩个JavaScript对象进行DIff比较，得到差异，只更新差异

不难看出，虚拟DOM的优势在于更新DOM



渲染器的作用就是将虚拟DOM（JavaScript对象）渲染成真实的DOM

渲染器将JavaScript对象的值获得，并且以原生JavaScript方式渲染出来，当tag为字符串时，渲染标签元素，如果是函数（或者对象）那么表示该是组件，组件函数返回值本身就是虚拟DOM，如果是对象的话，获取对象的render函数，得到其返回值（这个实质上也是虚拟DOM），编译器（模板）原理上和渲染器是一样的


---


transition（过渡和动画）


过渡：元素属性从一个属性过渡为另一个属性，例如元素的背景颜色从黑色过渡到白色

动画：一个元素从一个地方移动到另一个地方

vue内置了组件和API来完成动画和过渡

过渡：

    const app = Vue.createApp({
        data(){
            return {
                ishallo: false
            }
        },
        methods:{
            onhalloClick(){
                this.ishallo = ! this.ishallo
            }
        },
        template: `  
            <button @click='onhalloClick'>点我隐藏/显示</button>
            <transition>
                <div v-if="ishallo">hallo word</div>
            </transition>
        `
    })
    const vm = app.mount("#app")


动画

vue

    const app = Vue.createApp({
        data(){
            return {
                ishallo: false
            }
        },
        template: `  
            <div :class='{ hallo: ishallo }'>
                <button @click='ishallo = true'>点我触发动画</button>
                <div v-if="ishallo">hallo word</div>
            </div>
        `
    })
    const vm = app.mount("#app")

css样式

    .hallo {
        animation: hallo 1s linear
    }
    @keyframes hallo {
        0% {
            transform: translate3d(0px, 0, 0)
        }
        10% {
            transform: translate3d(-2px, 0, 0)
        }
        60% {
            transform: translate3d(2px, 0, 0)
        }
        100% {
            transform: translate3d(0px, 0, 0)
        }
    }

硬件加速：使用了perspective，backface-visibility和transform: translateZ()都可以触发硬件加速


解决过渡和动画时间不一致

<transition type="animation">，当使用该属性，动画结束，过渡会跟着结束

<transition type="transition">，当使用该属性，过渡结束，动画会跟着结束


统一管理动画和过渡时间

<transition :duration="1000">，该属性值单位为毫秒，意思为1秒后结束动画和过渡

<transition :duration="{enter: 1000,leave: 2000}">，enter为进入动画和过渡时间，leave为离开动画和过渡时间

transition内置组件在一个过渡周期会触发6个状态，分别是v-enter-from，v-enter-active，v-enter-to，v-leave-from，v-leave-active，v-leave-to

v-enter-from：表示过渡的开始状态，元素插入之前触发，元素插入后的下一帧移除

v-enter-active：表示过渡生效时状态，在元素插入之前触发，在动画和过渡完成后移除

v-enter-to：表示过渡的结束状态，在元素插入后下一帧触发，在动画和过渡完成后移除

v-leave-from：表示离开过渡的开始状态，离开过渡时立即触发，下一帧后移除

v-leave-active：表示离开过渡的生效状态，离开过渡时立即触发，在动画和过渡完成后移除

v-leave-to：表示离开过渡的结束状态，离开过渡后下一帧触发，在动画和过渡完成后移除

用法：


    const app = Vue.createApp({
        data(){
            return {
                ishallo: false
            }
        },
        template: `  
            <transition>
                <button @click='ishallo = true'>点我触发动画</button>
                <div v-if="ishallo">hallo word</div>
            </transition>
        `
    })
    const vm = app.mount("#app")

样式

    .v-enter-active{
        animation: hallo 1s; // 表示入场时触发时hallo动画
    }
    .v-leave-active{
        animation: hallo 1s;  // 表示出场时触发时hallo动画
    }
    @keyframes hallo {
        0% {
            transform: translate3d(0px, 0, 0)
        }
        10% {
            transform: translate3d(-2px, 0, 0)
        }
        60% {
            transform: translate3d(2px, 0, 0)
        }
        100% {
            transform: translate3d(0px, 0, 0)
        }
    }


关闭css动画：<transition :css="false">，使用该属性表示将通过组件事件来触发动画，不再使用css动画了

过渡模式：<transition mode="out-in">，该属性有2个值，out-in和in-out，默认同时进行

out-in：当前元素先进行过渡，完成后新元素过渡进入

in-out：新元素先进行过渡，完成后当前元素过渡离开

该常用于动态组件的切换过渡，也是说out-in是当前元素先进行过渡，切换到新元素时，过渡进入，而in-out是新元素过渡，切换到当前元素时过渡离开

name属性：<transition name="hallo">，该属性表示自动生成css过渡类名，像例子中的，将生成为.hallo-enter等等，就是将过渡周期中的6个状态的v改成name属性值了



---

vue devtools调试工具

下载https://github.com/vuejs/vue-devtools

解压，并且在该目录执行npm install

修改manifest.json（该文件在项目目录的packages/shell-chrome），将persistent改为true

打包npm run build


以谷歌浏览器为例子，加载已解压的扩展程序，选择刚刚打包的目录的packages/shell-chrome文件夹




