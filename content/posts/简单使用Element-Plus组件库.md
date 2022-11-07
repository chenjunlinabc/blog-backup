---
title: "简单使用Element Plus组件库"
categories: [ "学习" ]
tags: [ "vue","Element" ]
draft: false
slug: "157"
date: "2022-11-03 17:40:00"
---

Element Plus是基于vue3开发的组件库，而Element使用vue2开发的组件库

安装Element Plus

npm install element-plus --save

或者

yarn add element-plus


导入Element Plus

    import ElementPlus from 'element-plus'
    import 'element-plus/dist/index.css'


按需导入

安装插件

npm install -D unplugin-vue-components unplugin-auto-import

如果是使用Vite则配置vite.config.ts文件

导入并且启用插件

    import AutoImport from 'unplugin-auto-import/vite'
    import Components from 'unplugin-vue-components/vite'
    ...
    plugins: [
        AutoImport({
            resolvers: [ElementPlusResolver()],
        }),
        Components({
            resolvers: [ElementPlusResolver()],
        }),
    ],


这个组件库还支持module导入的方式手动按需使用，例如：

    <template>
        <el-button>I am ElButton</el-button>
    </template>
    <script>
        import { ElButton } from 'element-plus'
            export default {
                components: { ElButton },
            }
    </script>


