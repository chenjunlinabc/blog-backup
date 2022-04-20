---
title: "React进阶学习笔记"
categories: [ "学习" ]
tags: [ "React" ]
draft: false
slug: "94"
date: "2021-09-03 12:00:00"
---

react16是Facebook在2017年发布的react版本，几乎对react底层进行重写，但是对外API不变，因此可以无缝转移到react16

render返回类型

除了只能返回单个元素外，react16支持返回字符串和数组（由react元素组成）


例如：

    render(){
        return[
            <div>hallo</div>,
            <div>word</div>
        ]
    }


或者

    render(){
        return "hallo word"
    }




错误处理


react16引入了新的错误处理机制，当组件发生错误，将会将其从组件树中卸载，避免引起整个应用的崩溃

当然也提供了更友好的处理方式，叫错误边界，这个会捕获子组件的错误，并且输出错误日志和出错提示，例如：


    componentDidCatch(error, info){
        console.log(error,info)
    }






Portals

React16的Portals特性可以将组件渲染到当前组件树以外的DOM树上，例如弹框

ReactDOM.createPortal(child, container)


第一个参数是可以被渲染的react节点，第二个参数是dom元素，react节点将会被挂载到该DOM元素上




自定义DOM属性


在react16之前，会忽略不识别的属性，而在react16之后，会将不识别的属性传递给dom元素


---

React AJAX（搭配jQuery）


通过componentDidMount()调用，通过componentWillUnmount()取消未完成的请求




---



Virtual-DOM实质上就是模拟DOM树结构，通过JavaScript对象来描述DOM对象，通过映射成真实的DOM节点来实现

对于DOM节点数据更新，则通过生成一个新的Virtual-DOM，两个Virtual-DOM通过Diff算法进行差异更新，将更新处理为真实的DOM

Virtual-DOM的优势：减少操作DOM，处理视图和状态的关系


没有任何框架能比原生DOM处理快，但是操作原生DOM可能导致浏览器的回流（回流是性能第一杀手），因此在复杂视图下，原生DOM操作就可能没有Virtual-DOM性能好了



---



react-markdown是react官方提供的库，专门用来解析md文件或者符合md语法的变量

安装react-markdown

yarn add react-markdown

导入

import ReactMarkdown from 'react-markdown'

测试

    let markdown =
        "**这是加粗的文字**\n\n" +
        "*这是倾斜的文字*`\n\n" +
        "***这是斜体加粗的文字***\n\n" +
        "~~这是加删除线的文字~~ \n\n" +
        "`console.log(111)` \n\n" +
        "``` var a=11; ```";

    <ReactMarkdown source={markdown} escapeHtml={false} children={markdown}/>



---





