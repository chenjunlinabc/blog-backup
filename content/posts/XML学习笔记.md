---
title: "XML学习笔记"
categories: [ "学习" ]
tags: [ "xml" ]
draft: false
slug: "17"
date: "2021-06-16 09:32:00"
---


xml中文全称为可扩展标记语言（Extensible Markup Language）

xml和html类似，但是xml是用来传输和存储数据的，xml大小写敏感，xml的标记是自定义的

xml声明，该声明位在xml文档的第一行，<?开头，?>结尾

<?xml version="1.0" encoding="UTF-8" standalone="no"?>

version属性表示xml的版本，encoding属性表示该xml文档的编码方式，standalone属性表示该文档是否为独立，no表示依赖于外部文档

xml的标记也可以像html一样，认为是元素，一般由开始标签，属性，内容，结束标签组成，例如<hallo abc="yes">xml<hallo/>

<hallo>和</hallo>就是xml文档中的标签（元素），和html一样，可以嵌套n个子元素，如果一个元素没有被嵌套到其他元素上，那么这个元素就是根元素

一般来说一个格式良好的xml文档只有一个根元素，而且根元素是xml文档的第一个元素，如果一个元素没有嵌套子元素也没有内容，那么这个元素为空元素，空元素不需要结束标签，例如</hallo>

属性是元素的描述和说明，可以使用多个属性，每个属性都有自己名称和值，值用""或者''包起来

xml的注释方式和html一样，<!--这是注释-->

DTD约束

XML是自定义标签，浏览器不知道这个标签是用来干什么的，因此制定的一套约束，遵循一定的语法

DTD约束写在一个DTD文件里，dtd导入，例如：

<!DOCTYPE 根元素名称 SYSTEM "DTD文件的位置，可以为本地，也可以为url">


<!DOCTYPE 根元素名称 PUBLIC "DTD名称" "DTD文件的位置，可以为本地，也可以为url">


如果导入了本地的DTD文档，那么standalone属性的值不能为yes

DTD除了导入外，还可以内嵌，例如

<!DOCTYPE 根元素名称[
xxx
...
]>


<!ELEMENT 元素名称 元素内容>

元素内容包括元素的声明，包括数据类型和符号

PCDATA  中文意思就是被解析的字符数据，会被解析器解析

子元素  例如<!ELEMENT hallo (a,b,c)>

空元素  <!ELEMENT hallo EMPTY> 一般用来定义空元素

ANY  表示这个元素没有包含任何字符数据和子元素，例如<!ELEMENT hallo ANY>，在实际开发中，尽量不要使用ANY，除了根元素外，其他使用ANY的元素会失去DTD对XML的约束

符号

问号?:表示该对象可以出现0次或者1次
星号*:表示该对象可以出现0次或者多次
加号+:表示该对象可以出现1次或者多次
竖线|:表示在列出的对象中选择一个
逗号,:表现对象必须按照指定的顺序出现
括号():用于给元素进行分组

DTD除了给定义元素外，也可以为元素定义属性

<!ATTLIST 元素名称
属性名称a 属性类型 设置说明
属性名称x 属性类型 设置说明

>

属性类型指定该属性是属性哪个类型的，属性说明一般用来说明该属性是否必须出现

属性类型

CDTATA // 表示属性类型为字符数据，如果想在属性设置值中出现特殊符号（例如<），那么需要使用其转义字符序列来表示，例如 "&lt;"来表示"<"

Enumerated(枚举类型)  //  在声明一个属性时，可以限制该属性的取值只能从一个列表中选择，但是在DTD文档中不会出现Enumerated关键字，用法例如：<!ATTLIST Alphabet property(a|b|c|d|e) "a"> 

ID  // 表示该属性类型为唯一标识，一个元素只能有一个id类型的属性，而且属性说明必须为REQUIRED或者IMPLIED

IDREF和IDREFS  //   一般用来关联元素与元素之间的关系，而且IDREF类型的属性的值必须为一个已经存在的ID类型的属性值，例如：

<!ATTLIST abc xyz ID #REQUIRED hallo IDREF #IMPLED>

<abc xyz="01">1</abc>
<abc xyz="02" hallo="01">2</abc>

说明ID为01和02的元素之间，存在关联

IDREFS就是引用多个ID类型的属性值，例如：

<!ATTLIST abc xyz  IDREFS #REQUIRED>

<abc xyz="01 02 03">1</abc>


属性说明

REQUIRED // 表示这个元素的这个属性是必须的

IMPLIED //  表示这个元素可以包含这个属性，也可以不包含

FIXED  //  表示这个固定的属性默认值，不能将该属性的值设置为其他值，使用该说明时还需要提供一个默认值，如果XML没有定义该属性，那么其值就被自动设置为DTD定义的默认值

默认值 // 和FIXED一样，不同的是，这个属性的值可以改变，如果在xml文档中设置了新的值，那么新的值会覆盖DTD定义的默认值