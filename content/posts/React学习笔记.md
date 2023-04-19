---
title: "React学习笔记"
categories: [ "学习" ]
tags: [ "React" ]
draft: false
slug: "55"
date: "2021-07-09 22:23:00"
---

React是构造用户界面的JavaScript库，就是负责视图层的，只负责视图的渲染，其特点是声明式，组件化

安装react

npm i react react-dom

必需：react和react-dom

react包是提供创建元素，组件等功能，是核心（废话）

react-dom包提供DOM相关的功能

通常还需要导入babel来解析jsx（注意：在浏览器使用babel编译jsx效率很低，babel内嵌了对于jsx的支持），babel可以将es6语法转化为es5，方便运行在不支持es6的浏览器上


也可以通过引入src文件的方式引入cdn文件或者本地文件

第一个例子

    <script type="text/babel">
         ReactDOM.render(
              <h1>hallo,react!!!</h1>,
              document.getElementById("app")
         );
    </script>


使用babel解析jsx，react官方推荐使用jsx，因为使用jsx渲染dom简单明了


jsx是一种JavaScript语法扩展，例如：

    const hallo = <h1>hallo</h1>;

像上面这种就是jsx，jsx本身是一个表达式，经过编译（例如babel）后jsx表达式会转换成JavaScript对象（注意：jsx不是标准的ECMAScript语法，是语法的扩展，不进行编译处理，直接使用是会报错的）

在jsx中插入JavaScript表达式，用大括号包含起来，例如：


    function hallo(yes){
        return yes.name;
    }
    const yes = {
       name: "react"
    };
    const hallo = <h1>hallo,{hallo(yes)}</h1>;
    ReactDOM.render(
        hallo,
        document.getElementById("app")
    );


当然react并没有强制要求必须使用jsx，例如：

    const hallo = React.createElement("h1",{class: "main"},React.createElement("p",null,"hallo word!!!"))
    /* 上面提供了三个参数，分别是元素名称，元素属性，元素的子节点 */
    ReactDOM.render(hallo,document.getElementById("app"))
    /* 上面提供了个两个参数，分别是要渲染的react元素，挂载点*/




---

react脚手架

初始化项目

npx create-react-app app

启动项目（在项目根目录执行）

npm start或者yarn start


模块化导入react

import React from "react"
import ReactDOM from "react-dom"


---

React元素的属性名采用驼峰命名法

class就需要改为className，for要改为htmlFor等等，例如：

    const app = (<div className="app">hallo</div>)


如果一个元素中没有子节点可以直接写为<div/>

推荐使用小括号包裹jsx，避免陷入自动插入分号陷阱


JavaScript表达式可使用大括号{}来包裹，直接使用，例如：

    const app = (<div>hallo，{3 < 8 ? "react" : "word"}</div>)


任何数据类型和函数调用都可以（对象除外），表示式的特点就是有值就可以，jsx本身也是个JavaScript表达式

因为if或者for之类的语句不能出现在大括号中




条件渲染（根据条件进行渲染指定jsx结构）

    import React from "react"
    import ReactDOM from "react-dom"
    const datamain = true;
    const hallo = () => {
        if(datamain){
            return <div>hallo word</div>;
        }
        return <div>数据加载完成</div>;
    }
    const app =(
        <div>
            {hallo()}
        </div>
    )
    ReactDOM.render(app,document.getElementById("app"))



当然也是支持三元表达式的，例如：

    const Data = () => {
        return datamain ? (<div>yes</div>) : (<div>no</div>)
    }


列表渲染


    const arrs = [
      {id: 1,name: "root"},
      {id: 2,name: "admin"},
      {id:3,name: "user"}
    ]
    const lists = (
      <ul>
        {arrs.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    )
    ReactDOM.render(lists,document.getElementById("app"))




jsx样式


行内样式style，类className，例如：

    const app = (<div style={{color: "#ccc"}}>hallo</div>)


---

react组件的概念和vue组件一样，拼装积木，重复性，独立性


函数创建组件

函数名开头必须大写（否则报错），函数组件必须要有返回值（否则报错），如果返回值为null，就不渲染任何内容，例如：


    function Hello(){
      return(
        <div>hallo,react</div>
      )
    }
    ReactDOM.render(<Hello />,document.getElementById("app"))

或者

    const Hello = () => (<div>hallo,react</div>)


类创建组件

同样类名开头也要是大写，而且类组件要继承React.Component父类，使用父类中的方法和属性，而且还要提供render()方法，render()还要有返回值

    class Hello extends React.Component{
      render(){
        return (<div>hallo,react</div>)
      }
    }
    ReactDOM.render(<Hello />,document.getElementById("app"))


如果组件很多，应该放到单独的js文件，独立化个体，导入react，导出组件（export），用到该组件就导入




---



事件绑定

react事件采用了驼峰命名法，比如onClick，onMouseEnter，onFocus，例如：

    class Hello extends React.Component{
      mainClick(){
        console.log("被点击了")
      }
      render(){
        return (<div onClick={this.mainClick}>hallo,react</div>)
      }
    }



注意：如果是函数组件，就不需要加this，直接return (<div onClick={mainClick}>hallo,react</div>)



事件对象

可以通过事件处理程序的参数来获取到事件对象，在react中事件对象又叫为合成事件，兼容所有浏览器，无需担心浏览器兼容问题

阻止浏览器的默认行为

    class Hello extends React.Component{
      mainClick(e){
        e.preventDefault();
        console.log("被点击了")
      }
      render(){
        return (<a onClick={this.mainClick} href="https://xiaochenabc123.test.com">hallo,react</a>)
      }
    }


状态组件（状态即数据的更新）

函数组件为无状态组件，类组件为有状态组件


如果数据不需要变化就用函数组件，否则就类组件


状态（state）是组件内部的私有数据，只能在组件内部使用，state的值是一个对象，表示组件可以存在多个数据

第一种存储state方法

      constructor(){
        super()
        this.state ={
          count :0
        }
      }

第二种存储state方法（推荐）

      state = {
        count: 0
      }


获取state值

      render(){
        return (<div>{this.state.count}</div>)
      }


修改状态（状态可变）

this.setState({要修改的数据})


      render(){
        return (<div onClick={()=>{
          this.setState({
            count: this.state.count + 1
          })
          }}>{this.state.count}</div>)
        }
      }


注意：不要直接修改state的值，setState的作用是先更新state，然后更新视图

state抽离出逻辑

注意：箭头函数自身并不绑定this，箭头函数中this取决于外部环境

    class Hello extends React.Component{
      state = {
        count: 0
      }
      mainClick(){
        this.setState({
          count: this.state.count + 1
        })
      }
      render(){
        return (
            <div>
              <div onClick={()=> this.mainClick()}>+1</div>
              <div>{this.state.count}</div>
            </div>
          )
        }
      }
    ReactDOM.render(<Hello />,document.getElementById("app"))



解决绑定事件的this指向问题

如果不想利用指向函数（也可以给事件函数定义为指向函数），也可以利用bind()方法，例如：


    this.mainClick = this.mainClick.bind(this)


---




受控组件（其值受到react所控制），例如：

      state = {
        txt: ""
      }
      <div>
          <input type="text" value={this.state.txt}
          onChange ={ e => this.setState({
            txt: e.target.value
          })}
          />
          <div>
          {this.state.txt}
          </div>
      </div>


input（text），textarea，select都是用value控制，而input（CheckBox）就用checked控制

需要根据表单元素来判断其使用不同的类型控制（可以使用三元表达式来指示）



如果是非受控组件，想获取属性值，通过使用React.createRef()创建ref对象，使用ref来获取，例如：

      mainClick = () =>{
        console.log(this.Ref.current.value)
      }
      constructor(){
        super()
        this.Ref = React.createRef()
      }
      <div>
          <input type="text" ref={this.Ref}/>
          <div onClick={this.mainClick}>
              hallo
          </div>
      </div>


---


深入学习React组件

组件通讯（组件是独立并且封闭的，需要共享组件的数据，就需要组件通讯了）


接收传递给组件的数据需要通过props，函数组件通过props参数接收，类组件通过this.props接收，例如：


    const Hallo = props =>{
        console.log(props)
        return (
            <div>数据：{props.name}</div>
        )
    }
    ReactDOM.render(<Hallo name="root"/>,document.getElementById("app"))


从上面返回的props中可以看出是一个对象，也就是说可以传递多个数据


    class Hi extends React.Component{
      render(){
        return(
          <div>数据：{this.props.name}</div>
        )
      }
    }
    ReactDOM.render(<Hi name="root" pass={123}/>,document.getElementById("app"))

props可以传递任意类型的数据，props是只读的对象，只能读取属性的值，不能修改该对象，使用类组件，如果有构造函数，需要将其props传递给super()，通过props参数，否则无法在构造函数获取props（super(props)）


组件的通讯有三种，分别是父对子，子对父，兄弟


父组件传递数据给子组件：只需要在父组件中定义子组件的属性，例如：


    const Hallo = props =>{
      console.log(props)
      return (
        <span>{props.name}</span>
        
      )
    }
    class Hi extends React.Component{
      state = {
        Name: "root"
      }
      render(){
        console.log(this.props)
        return(
          <div>数据：
            <Hallo name={this.state.Name}/>
          </div>
        )
      }
    }
    ReactDOM.render(<Hi/>,document.getElementById("app"))



子组件传递数据给父组件

通过回调函数，父组件提供回调，子组件调用，将数据通过回调函数的参数传递，例如：


    class Abc extends React.Component{
      state = {name: "root"}
      DataClick = () =>{
        this.props.getMsg(this.state.name)
      }
      render(){
        return (
          <div onClick={this.DataClick}>yes</div>
        )
      }
    }
    class Hi extends React.Component{
      state = {
        Name: "默认值"
      }
      getDate = data =>{
        console.log(data)
        this.setState({
          Name: data
        })
      }
      render(){
        return(
          <div>数据：{this.state.Name}
            <Abc getMsg = {this.getDate}/>
          </div>
        )
      }
    }
    ReactDOM.render(<Hi/>,document.getElementById("app"))




---





兄弟组件通讯


将共享状态提升到最近的公共父组件中，由公共父组件管理这个状态（状态提升）

公共父组件：提供共享状态，提供操作共享状态的方法


A组件修改共享状态，公共父组件收到新的状态，B组件通过props接收到新的状态，从而达到兄弟组件之间的通讯


例如：



    class Hallo extends React.Component{
        state = {
            count: 0
        }
        onMax = () =>{
            this.setState({
                count: this.state.count +1
            })
        }
        render(){
            return (
                <div>
                    <Abc count={this.state.count}/>
                    <Xyz onMax = {this.onMax}/>
                </div>
            )
        }
    }
    const Abc = (props) => {
        return (
            <div>
                hallo,{props.count}
            </div>
        )
    }
    const Xyz = (props) => {
        return (
            <div onClick = {() => props.onMax()}>
                xyz
            </div>
        )
    }
    ReactDOM.render(<Hallo />,document.getElementById("root"))




---



Context（跨组件传递数据）

Context实质上就是生产者-消费者模式

调用React.createContext()，创建provide（提供数据）和consumer（获取数据）这两个组件

const {Provide,Consumer} = React.createContext()

通过value属性提供数据，然后通过回调函数来获取到value的数据


例如：

    const {Provider,Consumer} = React.createContext()
    class Hallo extends React.Component{
        state = {
            count: 0
        }
        onMax = () =>{
            this.setState({
                count: this.state.count +1
            })
        }
        render(){
            return (
                <div>
                    <Provider value = "react">
                        <Abc />
                    </Provider>
                    
                </div>
            )
        }
    }

    const Abc = (props) => {
        return (
            <div>
                <Consumer>
                    {
                       data => <div>hallo,{data}</div>
                    }
                </Consumer>
            </div>
        )
    }



---




props深入学习



children属性：表示组件标签的子节点，当组件标签有子节点时，props就会拥有该属性

children属性值可以是任意值（文本，jsx，组件，函数等等）

children属性指向到当前子节点的数据，{props.children}，例如：


    const Button = () => (
        <button>
            我是一个button组件
        </button>
    )
    
    const Yes = (props) =>{
        return(
            <div>
                组件的子节点：{props.children}
            </div>
        )
    }
    ReactDOM.render(<Yes><Button/></Yes>,document.getElementById("app"))



---

props校验（保证组件传入指定合法格式的数据）

安装prop-types

yarn add prop-types

导入prop-types包并且建立个props校验实例

    import propTypes from "prop-types"
    class Noabc extends React.Component{
        render(){
            return(
                <div>hallo,{this.props.name}</div>
            )
        }
    }
    Noabc.propTypes = {
        name: propTypes.string
    }

    ReactDOM.render(<Noabc name="react"/>,document.getElementById("app"))


当传入的值的类型不对，将抛出错误


---


props默认值

给props设置默认值，在未传入props时生效

    class Defaultabc extends React.Component{
        render(){
            return(
                <div>props的默认值：{this.props.Defaultdata}</div>
            )
        }
    }
    Defaultabc.defaultProps = {
        Defaultdata: 999
    }
    ReactDOM.render(<Defaultabc/>,document.getElementById("app"))



---




单向数据流：指的是当前组件的state以props的形式流动时，只能流向组件树中比自己层级低的组件，不能向上流，也就是说可父传子，祖先传后辈，但是不能子传父


父传子通信，基于数据流单向的特性，父组件可以直接将this.props传入子组件中

子传父通信，因为数据流单向的特性，不能直接将数据传递给父组件，而是通过父组件传递子组件一个携带了上下文的函数，子组件得到这个函数后，传递给父组件的数据将以函数参数的形式发送回去

兄弟通信，利用父传子通信的特性，可以在两个兄弟组件之上创建一个父组件，来当中间件，实质上还是父传子通信

消息订阅与发布机制是组件通信的万金油，这边使用的是PubSubjs

安装

npm i pubsub-js

引入

import PubSub from 'pubsub-js'

只需要在父组件（祖先组件）中发布，然后在子组件（后辈组件）中订阅，就可以实现了


发布

PubSub.publish('test','hallo word')

发布信息实质是就是发送数据



订阅

const DataMain = PubSub.subscribe('test',data)

其中data就是'hallo word'的数据，订阅是获取发布的数据

取消订阅

PubSub.unsubscribe(DataMain)











---




组件的生命周期

组件的生命周期有利于掌握组件的运行方式，实现更复杂的组件功能，分析组件错误原因

生命周期：被创建，到挂载页面，卸载组件的过程

生命周期的每个阶段都可以伴随一些方法来调用，而这些方法就是生命周期的钩子函数（和vue的生命周期概念是一样的）

注意：只有类组件才有生命周期，函数组件没有


创建时：当组件被创建时（页面加载时），constructor() -> render() -> componentDidMount()

例如：


    class Defaultabc extends React.Component{
        constructor(props){
            super(props)
            console.log("constructor钩子被触发")
        }
        componentDidMount(){
            console.log("componentDidMount钩子被触发")
        }
        render(){
            console.log("render钩子被触发")
            return(
                <div>hallo react</div>
            )
        }
    }

constructor钩子在创建组件时触发，初始化state，为事件程序绑定this

render钩子在组件渲染时触发，渲染视图，注意：不要在render下使用setState方法

componentDidMount钩子在组件被挂载后触发（完成DOM渲染），发送网络请求，DOM操作


更新时：

当一个组件接收到新属性（new props）时，使用setState()，强制更新forceUpdate()都会触发重新渲染

例如：


    class Maxabc extends React.Component{
        constructor(props){
            super(props)
            this.state = {
                count: 0
            }
        }
        goClick = () =>{
            this.setState({
                count: this.state.count + 1
            })
        }
        componentDidUpdate(){
            console.log("componentDidUpdate钩子触发")
        }
        render(){
            console.log("reder钩子被触发")
            return(
                <div>
                    <div>hallo,{this.state.count}</div>
                    <button onClick={this.goClick}>go</button>
                </div>
            )
        }
    }
    ReactDOM.render(<Maxabc/>,document.getElementById("app"))


在这个实例可以看出，只要使用setState方法改变了数据，就会重新渲染组件，同时如果没有更新，就不触发componentDidUpdate钩子，当发生更新才会触发

componentDidUpdate钩子：发生网络请求，DOM操作，注意：如果需要setState()，需要放在一个if条件中，传入prevProp参数，获取上一次更新的props，this.props可以获取到当前的props

例如：


        componentDidUpdate(prevProp){
            console.log("componentDidUpdate钩子触发")
            if(prevProp.count !== this.props.count){
                this.setState({})
            }
        }

判断条件为更新前的props是否和当前的props不相等





卸载时：当组件从页面中卸载时

componentWillUnmount钩子：当组件被卸载时触发，可以用来执行清理工作等等

例如：



    class Maxabc extends React.Component{
        componentDidUpdate(prevProp){
            console.log("componentDidUpdate钩子触发")
            if(prevProp.count !== this.props.count){
                this.setState({})
            }
        }
        constructor(props){
            super(props)
            this.state = {
                count: 0
            }
        }
        goClick = () =>{
            this.setState({
                count: this.state.count + 1
            })
        }
        render(){
            console.log("reder钩子被触发")
            return(
                <div>
                    <p>
                        点击数：{this.state.count}
                    </p>
                    {
                        this.state.count >= 10 ? <p>已经点击了10次</p> : <Gabc/>
                    }
                    <button onClick={this.goClick}>go</button>
                </div>
            )
        }
    }
    class Gabc extends React.Component{
        render(){
            return(
                <div>hallo,react</div>
            )
        }
        componentWillUnmount(){
            console.log("componentWillUnmount钩子被触发")
        }
    }
    ReactDOM.render(<Maxabc/>,document.getElementById("app"))



清理（例如定时器），例如：


    class Gabc extends React.Component{
        componentDidMount(){
            this.timeId = setInterval(()=>{
                console.log("定时器执行中")
            },1000)
        }
        render(){
            return(
                <div>hallo,react</div>
            )
        }
        componentWillUnmount(){
            console.log("componentWillUnmount钩子被触发")
            clearInterval(this.timeId)
        }
    }



当卸载事件发生，就执行清除定时器，使用clearInterval()方法清除


注意：如果创建了一些不是组件本身的事件（例如定时器），一定要在组件被卸载的时候清除，避免一些奇怪的问题发生


----






render-props和高阶组件（HOC）

复用相似功能（复用状态state，组件状态的逻辑）


render-props模式

将复用状态state，组件状态的逻辑封装到一个组件中

添加一个值为函数的prop，通过函数参数来获取，通过该函数的返回值作为要渲染的内容

将复用的状态作为props.render(state)方法的参数，暴露到组件的外部

例如：


    class Hello extends React.Component{
        state = {
            x: 0,
            y: 0
        }
        yesClick = (e) =>{
            this.setState({
                x: e.clientX,
                y: e.clientY
            })
        }
        componentDidMount(){
            window.addEventListener("mousemove",this.yesClick)
        }
        render(){
            return this.props.render(this.state)
        }
    }
    class App extends React.Component{
        render(){
            return(
                <div>
                    <div>hallo,react</div>
                    <Hello render = {(mouse) =>{
                        return(
                            <p>
                                鼠标坐标：{mouse.x},{mouse.y}
                            </p>
                        )
                    }}/>
                </div>
            )
        }
    }


也可以使用children代替render属性，例如：


    class Hello extends React.Component{
        state = {
            x: 0,
            y: 0
        }
        yesClick = (e) =>{
            this.setState({
                x: e.clientX,
                y: e.clientY
            })
        }
        componentDidMount(){
            window.addEventListener("mousemove",this.yesClick)
        }
        render(){
            return this.props.children(this.state)
        }
    }
    class App extends React.Component{
        render(){
            return(
                <div>
                    <div>hallo,react</div>
                    <Hello>
                        {
                            mouse =>{
                                return(
                                    <p>
                                        鼠标坐标：{mouse.x},{mouse.y}
                                    </p>
                                )
                            }
                        }
                    </Hello>
                </div>
            )
        }
    }
    ReactDOM.render(<App/>,document.getElementById("app"))


可以给render props模式加props校验


应该在组件卸载时解除mousemove事件绑定（只要不是react的事件，都应该进行清除其他事件，例如window事件）

    componentWillUnmount(){
        window.removeEventListener("mousemove",this.handleMouseMove)
    }







---


高阶组件



高阶组件（HOC，Higher-Order Component），实质上就是一个函数，接收要包装的组件，返回增强后的组件

高阶组件实质上就是给组件进行“包装”，通过“包装”来增强组件的功能

高阶组件内部会创建一个类组件，在这个类组件中提供状态逻辑程序，通过prop将复用状态传递给被包装组件



创建函数，函数名一般是with开头，函数参数是要渲染的组件，因此参数是大写字母开头


在该函数中创建个类组件，提供复用的状态逻辑程序，状态通过prop传递给参数组件


例如：


    const withHallo = (DemoAbc) =>{
        return class App extends React.Component{
            state = {
                x: 0,
                y: 0
            }
            onAbc = (e) =>{
                this.setState({
                    x: e.clientX,
                    y: e.clientY
                })
            }
            render() {
                return (
                    <div onMouseMove= {this.onAbc}>
                        <DemoAbc {...this.props} mouse={this.state}/>
                    </div>
                )
            }
            componentDidMount(){
                window.addEventListener("mousemove",this.onAbc)
            }
            // eslint-disable-next-line react/no-typos
            componentWillunmount(){
                window.removeEventListener("mousemove",this.onAbc)
            }
            
        }
    }
    const PostMax = (props) =>{
        const { x, y } = props.mouse
        return (
          <div >
            <p>(x:{x}, y:{y})</p>
          </div>
        ) 
    }
    const Demoyes = withHallo(PostMax)
    ReactDOM.render(<Demoyes />,document.getElementById("app"))





可以从上面的实例看出，实质上就是通过函数参数方式接收一个组件，再返回一个新的组件，达到“包装”更多的功能给组件







---






使用高阶组件会导致获得的组件名称相同，名称取自于高阶组件中的类组件名称


通过设置displayName，来区分不同的组件，displayName是用来设置调试信息（react developer tools消息）的

可以通过react developer tools浏览器插件查看效果

通过给高阶组件中的类组件设置displayName，例如：


    const withHallo = (DemoAbc) =>{
        class App extends React.Component{
            ......
        }
        App.displayName = `WithHallo${ADisplayName(DemoAbc)}`
        return App
    }
    function ADisplayName(DemoAbc){
        return DemoAbc.displayName || DemoAbc.name || "Component"
    }



---




传递props（避免props丢失）


通过高阶组件返回的组件中，是获取不到props的，因为数据都传递给高阶组件中的类组件了（高阶组件没有往下传递props）


只需要将state和this.props一起传递就可以解决，例如：


    render() {
        return (
            <div onMouseMove= {this.onAbc}>
                <DemoAbc  {...this.state} {...this.props} mouse={this.state}></DemoAbc>
            </div>
        )
    }
    const PostMax = (props) =>{
        const { x, y } = props.mouse
        // eslint-disable-next-line no-undef
        console.log(props)
        return (
          <div >
            <p>(x:{x}, y:{y})</p>
          </div>
        ) 
    }
    const Demoyes = withHallo(PostMax)
    ReactDOM.render(<Demoyes a="hallo"/>,document.getElementById("app"))



通过上面的方法向下传递props就能获取到a的值了， {...this.state} {...this.props} 

































































