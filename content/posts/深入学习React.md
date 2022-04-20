---
title: "深入学习React"
categories: [ "学习" ]
tags: [ "React" ]
draft: false
slug: "63"
date: "2021-07-25 12:00:00"
---

setState()


注意：如果调用多次setState()，但是还是只触发一次重新渲染（性能优化，如果每更新一次状态就渲染一次的话，很影响性能）

setState()是异步更新数据的，因此setState()不要依赖于前面的setState()，因为其不会立刻更新数据


如果当前setState()依赖于前面的setState()，解决方法：


    this.setState((state, props) =>{
        return{
            count: state.count + 1
        }
    })
    console.log(this.state.count)
    this.setState((state, props) =>{
        return{
            count: state.count + 1
        }
    })
    console.log(this.state.count)

state和props参数分别获取到最新的state和最新的props，通过回调函数返回值，保证每次都获取到最新的state和props




如果想在状态更新后（页面完成重新渲染）立刻执行某个操作，那么可以使用setState()的第二个参数，这个参数是一个回调函数

例如：


    this.setState((state, props) =>{
        return{
           count: state.count + 1
        }
    },() => {
        console.log(this.state.count)
    })


可以看出 console.log(this.state.count)会在状态更新后被执行，因此可以获取到更新后的count值

因此setState()一定会改变状态，但是不会立刻更新，而是当页面渲染完毕了（状态更新完毕之后）才会更新




---


jsx语法的转化


jsx实质上还是createElement()方法的语法糖（简化），因为jsx语法最后还是会编译（@babel/preset-react插件）成createElement()方法

而createElement()方法也会被转换为js对象（ReactElement），用来描述页面上显示的内容，因此不管是jsx还是createElement()最后都会转换为js对象（ReactElement），ReactElement然后通过虚拟DOM实现DOM创建和更新

例如：

    const Abc = (
        <div>hallo</div>
    )
    console.log(Abc)


可以看到输出返回的是一个js对象


React.createElement()接收3个参数，type（表示标签或者组件），config（对象，表示组件的所有属性），children（对象，表示组件之间的嵌套关系）




---



组件更新机制

setState()的作用：修改state，更新组件


父组件重新更新state，子组件也会更新，不过只会影响到当前组件和其他子组件（后代组件）（组件树），对于该组件的兄弟组件和根组件（父组件）是不会影响


初次渲染，当页面刷新时就会初次渲染，先渲染根组件，再按顺序渲染更新父组件和后代组件

更新根组件，那么其下全部的组件树都会更新




---



组件性能优化


state只存储和组件渲染相关的数据，例如列表数据，而不用来渲染的数据不放在state中


如果需要在多个方法中使用（共享）的数据，应该放在this中



因为组件更新机制的原因，子组件没有变化也会重新渲染


解决方法：

使用钩子函数 shouldComponentUpdate(nextProps, nextState)，该钩子函数是更新阶段的，组件重新渲染前执行

通过该钩子函数的返回值来决定该组件是否重新渲染，返回true表示重新渲染，false表示不重新渲染

根据该钩子函数中定义的条件，来表示是否重新渲染，例如：


    shouldComponentUpdate(nextProps, nextState){
        console.log(nextProps, nextState)
        console.log(this.props, this.state)
        return false
    }


shouldComponentUpdate()的两个参数分别表示最新的props和state

而通过this获取的props和state是更新前的


因此可以通过比较更新前的数据和更新后的数据是否一致来决定是否重新渲染，不一致就重新渲染，一致就不重新渲染

例如：


    shouldComponentUpdate(nextProps, nextState){
        if(nextState.count !== this.state.count){
            return true
        }
        return false
    }
    

也可以直接进行比较来返回值，例如：return nextState.count !== this.state.count


给子组件加shouldComponentUpdate()，子组件判断是否一致，通过props来判断






纯组件（PureComponent）（内部对比是shallow compact，浅层对比）

使用方法：class App extends React.PureComponent{}


PureComponent内部自动实现了shouldComponentUpdate()钩子功能，不需要手动比较


纯组件内部通过比较更新前和更新后两次props和state的值，来决定是否重新渲染组件

浅层对比对于基础类型对比没有影响，就是值和值对比，而对于引用类型，只比较对象的引用地址是否相同，只要地址相同，就返回true，这个很坑


如果state或者props中属性值是引用类型的，应该新建一个新数据，而不是直接修改原来的数据，例如：

    const halloobj = { ...state.obj, a: "hallo" }
    setState({ obj: halloobj })








---




虚拟DOM和Diff算法


react渲染视图：只要state发生变化就重新渲染视图，部分更新，只更新变化的区域

虚拟DOM配合Diff算法实现了部分更新功能



虚拟DOM实质上是一个js对象，来描述视图显示的内容

在初次渲染的时候，react会根据初始state（Model），来创建一个虚拟DOM树，然后根据虚拟DOM来生成真正的DOM，渲染到视图中

当数据（state）发生变化时，会重新根据新的数据，创建新的虚拟DOM树

然后会与上一次得到的虚拟DOM对象，使用Diff算法进行比较，来找到需要更新的内容


然后只将变化的内容更新（patch）到DOM中，重新渲染到视图



当组件的render()被调用时，会根据state和jsx结构来生成虚拟DOM对象


render()方法调用，不代表视图重新渲染，只是说明要进行Diff比较





---





React路由

现代前端应用大多是SPA（单页应用程序），也就是只有一个html页面的应用程序，为了使用单页面来管理原来多页面的功能，因此前端路由诞生了


路由实质上就是从一个视图导航到另一个视图，是一套映射规则，在React中是url路径与组件的对应关系


安装路由（react）

yarn add react-router-dom


导入路由的核心组件（Router, Route, Link）


    import { BrowserRouter as Router, Route, Link } from "react-router-dom"



使用Router组件来作为根组件来包裹整个应用

使用Link组件来作为路由入口

使用Route组件来配置路由规则和要显示的组件（路由出口）


例如：


    const Hallo = () => (
        <div>
            hallo React路由
        </div>
    )
    const PostMax = (props) =>{
        return (
            <Router>
                <div>
                    <Link to="/hallo">跳转</Link>
                    <Route path="/hallo" component={Hallo}></Route>
                </div>
            </Router>
        ) 
    }





react路由的执行过程：

点击Link组件，修改URL

React路由监听到URL的变化，React路由内部遍历全部Route组件

使用路由规则（path）与pathname（浏览器URL）进行匹配

当路由规则和pathname匹配时，显示该Route组件的内容





编程式导航（通过js程序来实现页面跳转）


history是react路由提供的，用来获取浏览器历史记录的相关信息


用法：this.props.history.push("/hallo")

返回上一页面：props.history.go(-1)


push中的参数就是要跳转的路径

例如：


    const PostMax = (props) =>{
        return (
            <Router>
                <div>
                    <Link to="/demoabc">跳转</Link>
                    <Route path="/demoabc" component={Demoabc}></Route>
                    <Route path="/abc" component={Abc}></Route>
                </div>
            </Router>
        ) 
    }
    const Abc = (props) => {
        const onbock = () =>{
            props.history.go(-1)
        }
        return(
            <div>
                <div>hallo</div>
                <div onClick={onbock}>返回</div>
            </div>
        )
    }
    class Demoabc extends React.Component{
        onGo = () =>{
            this.props.history.push("/Abc")
        }
        render(){
            return(
                <div onClick = {this.onGo}>goDemo</div>
            )
        }
    }



默认路由（进入页面就显示指定组件）：进入页面就匹配的路由

默认路由为/，例如：


    <Route path="/" component={Demoabc}></Route>




匹配模式

默认情况下，react路由是模糊匹配

模糊匹配规则：当pathname以path开头就会匹配成功


因此默认路由（/）永远都会匹配成功，得出如果当前页面是/Demoabc/xxx，那么/Demoabc和默认路由（/）也是会匹配成功



精确匹配（避免默认路由匹配成功）

给Route组件添加exact属性就是将其转换为精确匹配模式

使用方法：

<Route exact path="/" component={Demoabc}></Route>


---


react的jsx中的onClick事件之类的都会封装在组件中，不会污染全局

在react中使用了一个叫事件委托的事件处理方式

无论页面有多少个事件，DOM上还是只添加一个事件函数


Virtual DOM

前端性能优化有个原则：尽量减少DOM操作

DOM树是对html的抽象，那么Virtual DOM就是DOM的抽象，react会对比这次和上次渲染出来的Virtual DOM，找到发生改变的地方，达到局部更新数据，只重新渲染发生改变的那个区域，而不是改变个数据，整个dom结构都重新渲染




根据软件设计的原则，一个优秀的组件要满足高内聚和低耦合的要求

高内聚：将逻辑或者和逻辑相关的封装到一个组件中

低耦合：不同的组件的依赖性要弱化，保证每个组件要尽量独立


组件对外用prop，对内用state

在外部的组件在任何情况下都不应该直接操作prop的值，而是在内部通过state来操作数据








---




react组件性能优化


单个组件：通过shouldComponentUpdata函数的定义，在必要的情况下不需要更新，来节约计算资源


多个组件：利用react的reconciliation算法，在树型结构的根节点不相同的话，react会推倒重新渲染，而如果相同，那么react就会保留节点的DOM元素，只对树型结构树型结构根节点上的属性和内容进行修改，只更新被修改的部分


巧妙使用key：key在react中就是唯一标识，react通过key来确定组件的身份标识，能更快速识别出哪里发生了改变，key的值不要经常改变，要保证其是唯一性，稳定不变性，请勿使用index作为key



---




Redux

Redux是专门管理数据状态和UI状态的（可预测化的状态管理），Redux通过将应用状态存储在公用仓库（store）中，统一管理，这个仓库里有一个状态树（state tree），组件可以直接向仓库请求，可以通过监控仓库的状态来刷新状态（store的变化会导致view的变化）,调试Redux可以用Redux DevTools，Redux使用的是Flux模式



安装Redux

npm install --save redux

或者react-redux

npm install --save react-redux


src/store/reducer.js

    const StateData = {
        data:[
            'hallo',
            'hahaha'
        ]
    }
    export default (state = StateData,action)=>{
        return state
    }

建立reducer管理


src/store/index.js


    import { createStore } from 'redux'
    import reducer from './reducer'
    const store = createStore(reducer)     
    export default store

上面通过createStore方法创建store仓库，并且暴露store仓库

组件获取状态

import React, { Component } from 'react';
import store from './store'
    class Demo extends Component {
        constructor(props){
            super(props)
            this.state=store.getState()
            console.log(this.state)
        }
        render() { 
            return ( 
                <div>
                    <div>{this.state.list}</div>
                </div>
             )
        }
    }






Redux有三个原则：唯一数据源，保持状态只读，数据改变只通过纯函数完成

唯一数据源就是将全部组件（或者应用）状态数据只存放在唯一的一个store上


保持状态只读就是不直接修改状态数据，通过一个action对象来完成，不是不修改状态数据，而是创建一个新的状态对象返回给Redux，让Redux来完成状态的处理


数据改变只通过纯函数完成就是通过reducer来规约数据

React-Redux将全部组件分为两种，UI组件和容器组件

UI组件就是只提供UI，没有涉及逻辑处理的，没有状态，不使用任何Redux的api，数据完全靠this.props来提供

容器组件就是只提供逻辑处理，不涉及任何关于UI的


一般来说容器组件会包裹UI组件，来提供组件通信，将数据传递给只提供UI的组件，UI根据传递过来的数据来渲染更新

在Redux中，容器组件都由Redux提供，只需要专注逻辑和UI组件就可以了

通过connect方法来将UI组件和容器组件连接起来，可以通过connect方法自动生成容器组件

例如：

    import { connect } from 'react-redux'
    const Docker = connect()(Ui)


其中Docker就是通过connect方法生成的容器组件，而Ui就是UI组件


connect()方法还支持两个参数，分别是输入逻辑和输出逻辑

输入逻辑就是如何将数据传递给UI组件的定义（props），输出逻辑就是如何将用户操作UI组件的数据映射到store.dispatch



---

withRouter高阶组件：当某个东西不是router路由时，但是需要它跳转时，就可以使用withRouter

withRouter组件是在react-router-dom中的，因此需要引用，用法：

    import React from 'react'
    import { withRouter, Route } from "react-router-dom"
    class Demo extends React.Component {
        render() { 
            console.log(this.props)
            return ( 
                <div>
                    <Route exact path='/' component={Home} />
                    <Route path='/admin' component={Admin} />
                </div>
             )
        }
    }
    export default withRouter(Demo)


可以看到withRouter接收一个组件，router的history, location, match存储在组件的props属性中































