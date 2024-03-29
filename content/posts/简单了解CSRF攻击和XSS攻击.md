---
title: "简单了解CSRF攻击和XSS攻击"
categories: [ "技术" ]
tags: [ "CSRF","XSS" ]
draft: false
slug: "99"
date: "2021-09-12 17:00:00"
---

CSRF

CSRF：跨站点请求伪造(Cross—Site Request Forgery)

CSRF实质上就是盗用身份，来实现发送恶意请求

在访问某个“可信”站点，生成Cookie，再打开某个恶意网站，而这个恶意网站带有CSRF攻击的脚本，然后通过引诱用户触发该脚本，就会向那个可信站点发送恶意请求

例如：

    <img src ="http://xxx.xxx/test?xxx=666">

只要访问就会发生该恶意请求，也可以是某超链接或者按钮等等


而解决方法就是保证请求来源自当前站点，例如Referer字段，这个字段记录来源站点的地址，通过判断Referer字段上的域名，来判断是否是当前网站自己的请求

这个方法有个缺点就是由浏览器提供，不能保证浏览器自己有没有安全漏洞，不然还是能够伪造Referer字段，而且如果用户设置不提供Referer字段，那也会默认导致认为是CSRF攻击

还有一个方法就是通过添加token来处理，请求地址使用一串令牌（加密），来确保是当前站点发送请求，因为哪怕是伪造请求，也不知道令牌是什么，而且还搭配XMLHttpRequest来请求，确保请求信息不被Referer字段收录



JavaScript简单实现方法

    let referer = document.location.href;
    if(referer != null && referer.startsWith("https://xiaochenabc123.test.com")){
        console.log("验证成功!!!")
    }else{
         console.log("验证失败!!!")
    }



防御措施：token身份验证，页面来源验证（referer）等等




---


XSS：跨站脚本攻击（Cross Site Scripting），因为避免和层叠样式表混淆，因此叫XSS

XSS攻击原理就是在页面插入恶意脚本，当页面被访问了，那么这个恶意脚本被执行，从而达到恶意攻击用户的目的

例如：在某个页面评论上加\<script>alert('hack')\</script>，那么如果没有做过xss攻击防御的站点，其他用户访问到这个页面，那么就会执行该程序


做那些标签防御，都可以闭合绕过，都是无用功，例如：通过属性值来输出


也用通过过滤\<script>标签的，但是可以通过大小写绕过（html不区分大小写）


简单的防御方法：

通过严格的过滤白名单来过滤，限制输入值的类型

html实体编码转义，htmlspecialchars()或者htmlentities()

X-XSS-Protection


xss和csrf的区别：csrf是利用网站的自动执行的接口，依赖于用户已经登录页面（Cookie），而且xss是在页面插入js，来恶意执行js脚本的内容

---




同源策略（Same-origin Policy）：只有协议，域名，端口号都相同才满足同源，否则就是跨域


同源策略是浏览器提供的安全策略，限制某个脚本的加载范围（隔离潜在恶意脚本的安全机制）

可以限制脚本无法读取Cookie，LocalStorage和IndexDB，无法操作dom（无法获取dom），请求不能发送等等

跨域通信的方式：jsonp，hash，postmessage，websocket，cors

同域可以使用ajax，cors（支持同源通信，也是支持跨域），websocket（不受同源策略限制）进行通信


内容安全策略（Content Security Policy）：可以一定程度上免疫或者削弱XSS攻击

内容安全策略实质上就是告诉浏览器，什么东西可以加载执行，什么东西不能加载执行，内容安全策略的实现和操作都由浏览器完成，只需要提供配置就好

要开启CSP需要服务器返回Content-Security-Policy头部


html也可以配置该策略，例如：

    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*;">


其中img-src对应的指定图像或者图标的有效来源


常用的有img-src，font-src，script-src，style-src等等



CSRF


X-Frame-Options：DENY/SAMEORIGIN


可以利用Cookie的SameSite属性，这个属性有3个值，分别是Strict，Lax，None，主要用的就是Strict属性值

Strict：完全禁止第三方Cookie，只有当目前网页的url和请求目标一致才会发生Cookie



中间人攻击可以通过开启https来进行大部分的攻击免疫（https中间人攻击还是存在的）


















