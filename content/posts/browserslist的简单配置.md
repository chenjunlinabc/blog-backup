---
title: "browserslist的简单配置"
categories: [ "默认" ]
tags: [ "npm","browserslist" ]
draft: false
slug: "140"
date: "2022-03-12 10:00:00"
---

browserslist是查询浏览器列表的工具，browserslisp的配置可写在package.json中，也可以单独写在.browserslistrc配置⽂件中

browserslist的配置文件会被Autoprefixer，Babel，postcss-preset-env，eslint-plugin-compat，stylelint-no-unsupported-browser-features，postcss-normalize，obsolete-webpack-plugin工具读取，并且对配置的目标浏览器做适配工作




npx browserslist可查看根据条件输出的浏览器列表

查看全球用户份额大于0.2%的浏览器

npx browserslist "> 0.2%"

查询Chrome最新1个版本

npx browserslist "last 1 Chrome versions"

查看browserslist的默认配置

npx browserslist "defaults"

browserslist的默认配置为> 0.5% and last 2 versions adn Firefox ESR and not dead

not dead的意思是不输出官方不再维护的浏览器（例如ie10），dead是不维护，not是不输出

and就是和，or是或者

browserslist的配置

package.json（browserslist官方推荐用这个）

    "browserslist": [
        '> 0.2%',
        'last 1 Chrome versions'
        'not dead'
    ]


或者写成

    "browserslist": [
        '> 0.2% and last 1 Chrome versions and not dead',
    ]



.browserslistrc

    > 0.2% and last 1 Chrome versions and not dead


browserslist数据的优先级：当前项目的package.json的browserslist，当前项目的.browserslistrc，BROWSERSLIST环境变量，browserslist的defaults


另外还可以设置development（开发环境），production（生产环境的配置），例如：


    "browserslist": [
        'development':[
            'last 1 Chrome versions',
            'last 1 firefox versions',
            'last 1 safari versions',
            '> 5%',
        ],
        'production': [
            'not dead'
        ]
    ]



