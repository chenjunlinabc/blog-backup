---
title: "React Hooks学习笔记"
categories: [ "默认" ]
tags: [ "React" ]
draft: false
slug: "90"
date: "2021-08-20 23:00:00"
---

React Hooks提供了新特性来给纯函数组件可以管理状态


学过react都知道纯函数组件没有生命周期钩子，而且还不能更新状态，只有class组件有生命周期钩子和状态


而一个好的组件要有高独立性，高可复用性，而class组件本身就是有状态，因此要复用起来比较麻烦，而且还有个this缺陷

React Hooks的出现，是因为类组件会继承React.Component父类，而React.Component父类拥有大量的方法和属性，在开发一些小型组件时，完全没有必要用到这么多的方法和属性，就好吧杀鸡用屠龙刀一样，可以但是没有必要，而纯函数组件就不存在这些方法和属性，可谓是轻量级


例如：

class组件：


    class Hi extends React.Component{
        constructor(props) {
            super(props)
            this.state = { count:0 }
        }
        render(){
            console.log(this.state.count)
            return(
                <div>
                    <p>数据：{this.state.count}</p>
                     <button onClick={this.addGo.bind(this)}>GO</button>
                </div>
            )
        }
        addGo(){
            this.setState({count:this.state.count+1})
        }
    }


那么函数组件+React Hooks怎么实现状态更新呢？

而function组件想更新状态可以通过React Hooks来实现，例如：

例如：

    import React, { useState } from 'react'
    function Hi(){
        const [ count , setCount ] = useState(0)
        return (
            <div>
                <p> 数据：{count} </p>
                <button onClick={()=>{setCount(count+1)}}>GO</button>
            </div>
        )
    }


没错就是这么简单，看起来函数组件+React Hooks更简洁，更容易理解


useState是一个hook函数，是react原生自带的

useState提供了声明状态，读取状态，修改状态的方法


const [ count , setCount ] = useState(0)

意思就是声明状态变量为count，count状态的初始值设置为0


<button onClick={()=>{setCount(count+1)}}>GO</button>

而这个就是改变状态，调用setCount()函数来实现，该函数接受的参数是已经修改过的新状态，react就会进行重新渲染组件



但是有一个坏处就是，如果存在多个状态，又没有key表示，如何知道状态是对于哪个useState呢？


实质上是通过useState出现顺序来确定，如果对应的顺序不对就会报错，因此React Hooks不能出现于条件判断或者定时器之类会影响顺序的语句中




---




状态有，生命周期也有

例如：

    import React, { useState , useEffect } from 'react'
    function Hi(){
        const [ count , setCount ] = useState(0)
        useEffect(()=>{
            console.log("data:"+count)
        })
        return (
            <div>
                <p> 数据：{count} </p>
                <button onClick={()=>{setCount(count+1)}}>GO</button>
            </div>
        )
    }
    export default Hi;


可以看到在第一次渲染和更新渲染都会执行一次，而在这里useEffect()的参数是一个匿名指针函数，函数里面是打印输出一句话，从这句话可以看出在第一次渲染的时候和更新的时候都会触发


useEffect()相当于componentDidMonut和componentDidUpdate


而且useEffect()中定义的函数是异步执行的，而componentDidMonut和componentDidUpdate是同步执行，各有优点缺点




---




实现类似componentWillUnmount生命周期钩子（组件销毁之前执行）的功能

注意：useEffect会出现副作用，当每次的状态发生变化时候，useEffect都会进行解除绑定


通过useEffect第二个参数，该参数是一个数组，该数组可以存放大量于状态相对应的变量，也可以了解为监控状态，而当传入空数组[]的时候，那么就是当组件被销毁才进行解除绑定，也就是说实现了componentWillUnmount的功能

    function Data() {
        useEffect(()=>{
            console.log('hallo')
            return ()=>{
                console.log('goodbye')
            }
        },[])
        return (<div>data</dv>);
    }



父子组件传值useContext


使用类组件的时候，父子组件之间传值是通过props进行的，但是函数组件并没有constructor构造函数，也因此没有props



通过createContext函数创建useContext


    import React, { useState , createContext, useContext } from 'react'
    const CountContext = createContext()
    ...
    const [ count , setCount ] = useState(0);
    ...
    <CountContext.Provider value={count}>
    </CountContext.Provider>


导入createContext函数，得到一个组件，并且通过该组件来传递或者使用状态


接收状态

const count = useContext(CountContext)
return (<div>{count}</div>)




---


Redux有3大核心概念：Action和Reducer和Store


这个Redcuer是一个纯函数，这个函数可以接收两个参数，一个是状态，另一个是控制状态的判断参数，例如：


    function demoReducer(state, action) {
        switch(action.type) {
            case 'a':
                return state + 1
            case 'b':
                return state - 1
            default: 
                return state
        }
    }

useReducer是用于增强Redcuer来实现类似redux的功能，例如：


    import React, { useReducer } from 'react'
    function demoReducer(){
        const [ count , dispatch ] =useReducer((state,action)=>{
            switch(action){
                case 'a':
                    return state+1
                case 'b':
                    return state-1
                default:
                    return state
            }
        },0)
        return (
           <div>
               <h2>{count}</h2>
               <button onClick={()=>dispatch('a')}>加</button>
               <button onClick={()=>dispatch('b')}>减</button>
           </div>
        )
    }
    export default demoReducer



---


useMemo优化hooks性能


useMemo用于解决无用渲染，函数组件不具备shouldCompnentUpdate生命周期钩子，因此不能通过该组件来决定组件是否更新，每一次调用函数组件都会执行组件内部的全部逻辑

    import React , {useState,useMemo} from 'react'
    ...
    const [name, setName] = useState('root')
    const data = useMemo(()=>{
        return {
            name
        }
    },[name])


useMemo第一个参数是()=>value，第二个参数是监听指定的值是否发生改变

上面会根据name来判断一下，如果name值没有发生改变，那么就不会进行render，而是重复用之前的value，只要发生改变才会重新计算value






useRef可以获取jsx的dom元素，可以用于控制dom，也可以用来存储变量

获取dom元素


    import React, { useRef } from 'react'
    function Demo(){
        const inputData = useRef(null)
        const onDataClick=()=>{ 
            inputData.current.value="hallo，word"
            console.log(inputData) 
        }
        return (
            <div>
                <input ref={inputData} type="text"/>
                <button onClick = {onDataClick}>显示</button>
            </div>
        )
    }
    export default Demo



useRef存储变量

        const [text, setText] = useState('goodbye')
        const textRef = useRef()
        useEffect(()=>{
            textRef.current = text;
            console.log(textRef.current)
        })
        ...
        <input value={text} onChange={(e)=>{setText(e.target.value)}} />


每次状态发生改变，都会在同时保存到useRef中



---


自定义Hooks函数


















