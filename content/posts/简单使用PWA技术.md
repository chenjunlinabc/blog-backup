---
title: "简单使用PWA技术"
categories: [ "技术" ]
tags: [ "PWA" ]
draft: false
slug: "154"
date: "2022-10-28 01:21:00"
---

PWA，Progressive Web App（渐进式web应用），PWA技术可以将web应用具备接近原生应用的特性和用户体验，无需额外安装，支持离线缓存，消息推送等功能

PWA由Service Worker，Promise，fetch，cache Api，Notification Api等技术组成

Service Worker：服务工作线程，独立于主线程，常驻内存，代理网络请求，依赖HTTPS通信

注册Service Worker

    navigator.serviceWorker.register('./sw.js',{scope: '/'}).then(
        registration => {
            console.log(registration)
        },error => {
            console.error(error)
        }
    )
    window.onload = function() {
        document.body.append('PWA!')
    }

sw.js

    const cachename = 'v1'
    self.addEventListener('install', function (event) {
        console.log('install',event)
        // 安装新的Service Worker脚本时触发，只有Service Worker脚本不同，会认为是不同的Service Worker版本
        event.waitUntil(new Promise(resolve =>{
            setTimeout(resolve, 1000) // 安装新的Service Worker脚本后等待1秒后激活该脚本
        }))
        // event.waitUntil(self.skipWaiting) // 强制停止老的Service Worker，激活启动新的Service Worker，只要有更新就激活新的
        event.waitUntil(caches.open(name).then(cache =>{
            cache.addAll([
                '/',
                './1.img'
            ])
        })) // 开启cache api缓存系统
    })
    self.addEventListener('activate', function (event) {
        console.log('activate',event)
        // 激活新的Service Worker脚本事件
        event.waitUntil(self.clients.claim())
        event.waitUntil(caches.keys().then(cacheNames =>{
            return Promise.all(cacheNames.map(cacheNames =>{
                if(cacheNames !== cachename){
                    caches.delete(cacheNames) // 当前缓存名称不等于设置的缓存名称时，清除缓存，重新获取资源
                }
            }))
        }))
    })
    self.addEventListener('fetch', function (event) {
        console.log('fetch',event)
        // 获取资源请求事件，例如增加外链
        event.respondWith(caches.open(cachename).then(cache => {
            return cache.match(event.request).then(response =>{
                if(response){
                    return response // 命中缓存则直接使用缓存
                }
                return fetch(event.request).then(response =>{
                    cache.put(event.request,response.clone())
                    // 没有命中则获取该资源
                    return response
                })
            })
        }))
    })







manifest.json：让web应用具备app的效果，例如logo，启动页面

    {
        "name": "hallo",
        "short_name": "你好",
        "display": "standalone",
        "orientation": "landscape",
        "start_url": "/",
        "theme_color": "purple",
        "background_color": "purple",
        "icons": [
            {
                "src": "logo.jpg",
                "sizes": "48x48",
                "type": "image/jpg"
            },
            {
                "src": "logo1.jpg",
                "sizes": "144x144",
                "type": "image/jpg"
            }
        ]
    }



Notification消息推送

查看授权

Notification.permission

有3个值，在浏览器默认为default，当同意时，为granted，当不同意时，为denied

在Service Worker中，默认为denied，因为Service Worker不能弹出授权请求，必须先在浏览器中同意后，Service Worker只能发送通知

请求授权

Notification.requestPermission().then(permission => console.log(permission))

发送通知

new Notification('hallo word', {body: 'hallo'})

在Service Worker发送通知

self.registration.showNotificatio('hallo word')



成熟的PWA插件，workbox




