---
title: "简单了解SEO"
categories: [ "默认" ]
tags: [ "SEO" ]
draft: false
slug: "2"
date: "2021-03-20 11:50:00"
---

SEO：搜索引擎优化（Search Engine Optimization），是利用搜索引擎的规则来提升网站在该搜索引擎的排名（免费，高回报）

---

robots.txt就是robots协议，用来告诉网络蜘蛛，这个网站什么内容是不应该获取的，那些内容可以获取，例如：淘宝就屏蔽了百度的爬虫

robots.txt文件应该放在网站根目录下

允许指定蜘蛛获取，*为通配符

User-agent: *

允许指定目录可以被获取
 
Allow: /

指定Sitemap文件在哪

Sitemap: sitemap.xml


不允许指定目录被获取（注意该方法会模糊匹配，例如/adminxxx，也是会被屏蔽）

Disallow: /admin





---


description（描述）

<meta name="description" content="这是一个描述">




---

keywords（关键词）

<meta name="keywords" content="关键词1, 关键词2">

---

Sitemap（通知搜索引擎，该网站有哪些可以供爬取的，常见的有xml，html）



---
HTML标签优化

语义化标签，例如header，nav，footer


---

内部连接优化

尽量不要使用JavaScript来设置链接，应该使用简单的a href



---

友情链接

友情链接就是在自己网站上放其他网站的链接，友情链接实质上并不能带来多少访问量，而且是用来增强搜索引擎的收录量爬取量

注意：请不要在友情链接上加rel="nofollow"，该属性会告诉搜索引擎爬虫不用抓取目标页，那么这个友情链接就是废了

而且不要乱加友情链接，应该选择高质量，而且内容相似，更新频率高，而且还要有一定的访问量

注意：如果没有必要就不要单向链接，爬虫跑过去，就不会回来了，一直到找到你的链接，通过该链接回来，所以没有必要不要单向链接


---


尽量避免使用iframe标签，搜索引擎不会抓取iframe标签的内容

重要信息请勿使用js输出，爬虫不会抓取js的内容

给图片加alt信息，重要信息请放头部，有部分搜索引擎爬虫会限制抓取的长度
