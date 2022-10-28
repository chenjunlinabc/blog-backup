---
title: "Nestjs学习笔记"
categories: [ "学习" ]
tags: [ "NestJS" ]
draft: false
slug: "139"
date: "2022-03-05 14:00:00"
---

NestJS是一个nodejs服务端应用开发框架，基于typescript开发，http服务框架默认为Express，也支持Fastify，支持面向对象，函数式以及函数响应式编程


安装

npm install -g @nestjs/cli

创建demo项目

nest new demo

选择使用包管理器（支持npm，yarn，pnpm）

创建完成后可以看到src目录，是典型的MVC架构

app.controller.ts（应用路由控制器，例如Get()方法，该路由控制器将从应用服务文件中获取数据，并且将数据返回到页面上）
app.controller.spec.ts（应用控制器单元测试）
app.module.ts（应用模块文件，nest模块化，一个nest项目最少有一个模块，通过controllers()方法接收一个模块组(数组形式)，）
app.service.ts（应用服务文件，数据来源于该文件）
main.ts（应用程序入口文件，实质上是async/await异步函数（bootstrap()）

从main.ts入口文件可以看出，nest应用实例是基于NestFactory类（该类来源于@nestjs/core，nest核心程序）对外暴露的方法创建的



启动项目

npm run start

访问http://localhost:3000/，如果看到Hello World!表示启动成功


nestjs cli支持对mvc模块的生成

新建nest项目
nest new demo

打包nest项目
nets build

运行nest项目
nest start

查看nest当前项目的一些信息
nest info



创建控制器
nest g controller 名称
或者
nest g co 名称

创建服务
nest g service 名称
或者
nest g s 名称

创建模块
nest g module 名称
或者
nest g mo 名称

创建异常过滤器
nest g filter 名称

创建拦截器
nest g interceptor 名称

创建中间件
nest g middleware 名称

创建管道 
nest g pipe 名称

创建守卫
nest g gu 名称

创建具备完整CRUD功能的模块
nest g res 名称


---




Swagger是一个用于生成，描述，调试RESTfulAPI的web服务框架，nest提供了模块来调用该框架

安装Swagger

npm install --save @nestjs/swagger swagger-ui-express


导入Swagger

main.ts

    import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';

在bootstrap(){}内部

    const options = new DocumentBuilder()
        .setTitle('api test')
        .setDescription('api test app')
        .setVersion('1.0')
        .addTag('cats')
        .build();
      const document = SwaggerModule.createDocument(app, options);
      SwaggerModule.setup('test', app, document);

127.0.0.1:3000/test

因为Swagger提供的是默认api测试，如果想对其他的controller进行测试，直接在controller文件中使用@Apitags装饰器

test.controller.ts

导入

    import { Apitags } from '@nestjs/swagger';
    @Apitags('test测试')

返回127.0.0.1:3000/test，可以看到除了默认的外，还有一个test测试的栏目

ApiOperation添加api的注解

导入

    import { Apitags, ApiOperation } from '@nestjs/swagger';

在想要设置的@Post()下，添加@ApiOperation({sumnary:'这是注解'})


添加传参数据约束

从tets.controller.ts中可以看到，create()方法中存在一个类型约束，该约束默认是空的，约束文件一般在dto文件夹下，名称为create-test.dto.ts

    import { ApiOperation } from '@nestjs/swagger';
    export class CreateTestDto{
        @ApiOperation({description:'这是用户名'})
        readonly name: string
        @ApiOperation({description:'这是年龄'})
        age: number
        @ApiOperation({description:'这是密码',default:'123456'})
        password: string
    }



传参必须存在这三个，而且必须符合类型检查，其中readonly表示不可更改，会对目标做一个只读属性的功能，default对应着传参默认值





---


mongoose是mongodb对象建模工具

安装mongoose

npm install --save @nestjs/mongoose mongoose


导入

app.module.ts

    import { MongooseModule } from '@nestjs/mongoose';


连接mongodb

    @Module({
        imports: [MongooseModule.forRoot('mongodb://localhost:27017/test')],
    })


mongodb模型注入

定义scheme

test/schemas/test.schema.ts

    import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
    import { Document } from 'mongoose';
    export type TestDocument = Test & Document;
    @Schema()
    export classTest extends Document {
          @Prop({required: true type:String})
          name: string;
          @Prop()
          age: number;
          @Prop({required: true,select: false})
          password: string;
    }
    export const TestSchema = SchemaFactory.createForClass(Test);


使用scheme


test.service.ts

导入需要的模块


    import { Injectable } from '@nestjs/common';
    import { InjectModel } from '@nestjs/mongoose';
    import { InjectModel } from '@nestjs/mongoose';
    import { TestDocument } from './schemas/test.schema';

在TestService()内部设置

    constructor(@InjectModel('Test.name') private testModel: Model<TestDocument>) {}


    async create(createTestDto: CreateTestDto){
        await const createdTest = new this.testModel(createTestDto);
        return createdTest; // 增
      }

      async findAll():{
            return await this.testModel.find().exec() // 查
      }
      async findOne(id: string){
           return await this.testModel.findByid('id').exec() // 根据id查找
      }
      async update (id: string, updateTestDto: UpdateTestDto){
            return await this.testModel.findByidAndUpdate(id,updateTestDto,{new: true}).exec() // 更改
      }
      async remove(id: string){
          return await this.testModel.findByidAndRemove(id) // 删除
     }


test.module.ts

导入

    import { MongooseModule } from '@nestjs/mongoose';
    import { Test, TestSchema} from './schemas/Test.schema';

在@Module修饰器内部的imports下添加MongooseModule.forFeature([{ name: Test.name, schema: TestSchema }])







---

