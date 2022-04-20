---
title: "简单使用TailwindCSS"
categories: [ "默认" ]
tags: [ "css","TailwindCSS" ]
draft: false
slug: "130"
date: "2022-01-27 12:00:00"
---

TailwindCSS是一个CSS框架，我个人理解这东西就是根据class来生成css（按需），而不是像bootstrap那样，TailwindCSS是原子化的


安装

npm install tailwindcss

初始化tailwind.config.js

npx tailwindcss init



在tailwind.config.js中content属性，表示着项目的html或者js文件

    content: [
        './src/**/*.{html,js}'
    ],

如果没有配置content属性，会警告

warn - The `content` option in your Tailwind CSS configuration is missing or empty.
warn - Configure your content sources or your generated CSS will be missing styles.
warn - https://tailwindcss.com/docs/content-configuration


创建一个css文件配置tailwind三大组件（base，components.utilities）

    @tailwind base;
    @tailwind components;
    @tailwind utilities;

如果使用的是webpacker或者postcss-import，不能使用@tailwind指令，需要

    @import "tailwindcss/base";
    @import "tailwindcss/components";
    @import "tailwindcss/utilities";

也可以将css导入到js中

    import "tailwindcss/tailwind.css"


tailwind编译

npx tailwindcss -i ./src/index.css -o ./dist/main.css

如果使用的是postcss

postcss ./src/index.css -o ./dist/main.css



例如:

    <h1 class="text-gray-900">
        hallo word
    </h1>


编译后的结果为

    .text-gray-900 {
        --tw-text-opacity: 1;
        color: rgb(17 24 39 / var(--tw-text-opacity));
    }


具体样式根据tailwindcss官方文档为准，https://tailwindcss.com/docs/


当然也允许自定义，直接配置tailwind.config.js文件就好了（theme属性）


Visual Studio Code插件Tailwind CSS IntelliSense，让tailwindcss使用更舒适