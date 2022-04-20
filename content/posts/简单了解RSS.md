---
title: "简单了解RSS"
categories: [ "默认" ]
tags: [ "RSS" ]
draft: false
slug: "122"
date: "2021-12-06 02:32:00"
---

RSS指 Really Simple Syndication（简易信息聚合），RSS定义了方法来获取网站的标题以及内容，而且RSS可以被自动更新，RSS使用了XML进行编写（xml笔记：https://cjlio.com/archives/17.html）


一个RSS例子：


    <?xml version="1.0" encoding="UTF-8" ?>
    <rss version="2.0">
        <channel>
            <title>小陈的辣鸡屋</title>
            <link>https://cjlio.com/</link>
            <description>cjlio.com</description>
            <item>
                <title>简单了解设计模式</title>
                <link>https://cjlio.com/archives/121.html</link>
                <description><![CDATA[设计模式实质就是一套可以通用，复用的设计方案，设计模式是针对面向对象的，在面向对象出来之前，程序是面向过程的，设计模式就是软件设计的工具面向过程：逻辑化过程，以逻辑实现面向对象：思考有哪些对象，...]]></description>
                <content:encoded xml:lang="zh-CN">xxxxxxx</content:encoded>
            </item>
        </channel>
    </rss>


可以看到<title>是RSS频道的标题，<link>是该频道的超链接，<description>是该频道的描述，<item>是定义该频道的某篇文章的，其中<item>又有<title><link><description>，分别表示文章标题，文章的超链接，文章的描述，其中还有<content>表示文章的内容

RSS注释和HTML的注释一样，<!-- 这是RSS注释 -->

注意：RSS是基于XML编写，所以全部元素都要有闭合标签，大小写敏感，属性值要带引号


channel元素除了上面那几个外，还有<category>，<copyright>，<language> ，<image>等等

<item>元素还有<author>，<enclosure>，<comments>，<pubDate>等等


RSS阅读器可以更好读取RSSfeed