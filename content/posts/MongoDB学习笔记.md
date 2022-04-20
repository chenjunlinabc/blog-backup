---
title: "MongoDB学习笔记"
categories: [ "默认" ]
tags: [ "MongoDB" ]
draft: false
slug: "76"
date: "2021-08-08 14:20:00"
---

MongoDB是一个以键值对存储数据的数据库（基于json描述数据，实质上是一个叫BSON的数据格式，BSON是基于二进制字节流，json基于文本）

基于json有个好处就是不需要额外对数据进行转换（例如sql，在调用其数据时需要进行数据的转换）

MongoDB使用了WiredTiger存储引擎（3.2+版本开始为默认引擎），利用可用内存来缓存数据，来提供优秀的读取性能，该引擎使用WiredTiger内部缓存和文件系统缓存这俩种缓存

WiredTiger提供了内存快照，而且每隔60秒（创建检查点）就将内存快照写入磁盘（数据持久化，而且持久化的数据还可以作为校验，确保数据在最后一个检查点是一致的，而且旧检查点可以作为有效检查点恢复因为写入新检查点时的错误而且重启导致丢失的数据）

对于60秒内的数据丢失，WiredTiger采用了Journal机制（WAL预写日志）来提供断电保护，Journal每隔100ms刷新日志，数据被存储在Journal文件（发生断电情况，可通过Journal文件根据记录追加数据），Journal机制会保留检查点之间保留所有数据修改

而且不用担心100ms以内的数据丢失，因为MongoDB提供了其特有的写入安全机制（Write Concern），默认使用Acknowledged安全策略，该策略在每次写入操作时确认状态，这个状态取决于内存的写入（不保证数据不丢失）

Journaled策略要求每一次写入操作必须在journal落盘后（确保数据不丢失，吞吐和响应会有影响），该策略会确认落盘后等待30ms，将30ms内的全部写入操作统一按照顺序写入盘中

majority副本集策略要求只有当数据被复制到绝大多数节点（包括主节点）后才应答（适合集群）


Write Concern用法

例如：

db.test.insert({"name":"hallo"},{writeConcern:{w:1}})


w有这几个常用值，分别为0（非应答式写入），1（应答式写入），>1（设置副本写入节点数量），majority(表示majority副本集策略)

而且还提供了wtimeout参数来限制节点写入时间（超过该时间，报错，只适用集群环境，单位ms）

j参数是用来开启写入操作必须在写入journal日志后响应（Journaled策略，参数值为布尔值）


MongoDB有三种集群部署架构，主从，副本集，分片

一个副本集架构由一个主节点和多个副本节点组成，个主节点和多个副本节点的数据同步基于oplog，当主节点发生故障，副本节点会自动选择一个新的主节点来继续工作

在分片架构上，数据均衡分布在每一个节点上（负载均衡），可通过增加或者减少分片来实现按需扩展，


---


简单使用MongoDB数据库


mongod.conf文件是MongoDB数据库配置文件

启动数据库（没有该数据库则创建数据库）

mongod --dbpath D:\mongodb

默认端口为27017

启动服务

net start mongodb

连接数据库

mongo --port 27017

切换数据库（没有该数据库就会创建）

use demo

查看全部数据库

show dbs


注意：默认数据库为test，没有创建新数据库，那么就存储在test数据库中



删除数据库（当前数据库）

db.dropDatabase()


创建集合（类似于MySQL中的表）

db.createCollection("hallo")


插入数据（插入到test集合中，当该集合不存在时，将自动创建该集合）

db.test.insert({"name":"hallo"})


查看当前已有集合

show collections

或者

show tables


db.createCollection("test", { capped : true, autoIndexId : true, size : 102400, max : 10000 } )

整个集合空间大小102400B, 文档最大个数为10000个


删除集合（删除test集合）

db.test.drop()


插入数据（数据结构和json一样）

db.test.insert() // 当数据主键已存在，抛出错误


    db.test.insert({
        title: "小陈的辣鸡屋", 
        description: "小陈辣鸡屋",
        by: "小陈的辣鸡屋",
        url: "https://cjlio.com",
        tags: ["小陈", "hallo", "chenjunlin"],
        likes: 1
    })

查看插入的文档数据（默认只输出20条，输入it指令查看下一批，返回值为游标对象）

db.test.find()

也可以将数据定位为一个变量

    data=({
        title: "小陈的辣鸡屋", 
        description: "小陈辣鸡屋",
        by: "小陈的辣鸡屋",
        url: "https://cjlio.com",
        tags: ["小陈", "hallo", "chenjunlin"],
        likes: 
    })


更新数据（upsert参数是表示特殊的更新，如果目标不存在，那么就是插入）

    db.test.update({
        title: "小陈的辣鸡屋",
        tags: ["小陈", "hallo", "chenjunlin"],
    },{$set:{"title":"hallo 小陈的辣鸡屋",tags: ["小陈", "hallo", "chenjunlin","cjlio.com"]}},{"upsert": true})


更新数据（主键存在则更新，不存在则插入）

    db.test.save({
        "_id" :  ObjectId("610f84ac242ec572ff8a1678"),
        "title": "小陈的辣鸡屋", 
        "description": "小陈辣鸡屋",
        "by": "小陈的辣鸡屋",
        "url": "https://cjlio.com",
        "tags": ["小陈", "hallo", "chenjunlin"],
        "likes": 666
    })


查看更新后的数据

db.test.find().pretty()



删除数据

    db.test.remove({
        "title": "小陈的辣鸡屋"
    },1)

将title为小陈的辣鸡屋的第一条记录移除

删除指定条件的数据

    db.test.remove({
        "title": "小陈的辣鸡屋"
    })

删除全部数据

db.test.remove({})

或者db.test.drop


查询数据

AND，OR

查询条件默认为AND（和），只要有一项为真，则输出

db.test.find({"title":"小陈的辣鸡屋","by": "小陈的辣鸡屋"}).pretty()

OR条件（或），需要全部项都真，则输出

db.test.find({$or:[{"title":"小陈的辣鸡屋","by": "小陈的辣鸡屋hallo"}]}).pretty()

其他逻辑符：非（$not）,即非（$nor）

条件判断

大于（$gt）

db.test.find({likes : {$gt : 100}})


大于等于（$gte）

db.test.find({likes : {$gte : 100}})


小于（$lt）

db.test.find({likes : {$lt : 1000}})


小于等于（$lte）


db.test.find({likes : {$lte : 1000}})


组合（大于100，小于1000）

db.test.find({likes : {$gt : 100,$lte : 1000}})

其他比较符：等于（$eq）,数组是否包含（$in）,不等于（$ne）,数组是否不包含（$nin）

数组比较符：全包含（$all），大小匹配（$size），仅一个元素匹配（$elemMatch）

按照降序输出（-1降序，1为顺序（默认为顺序，可省略1））

db.test.find().sort({id: -1})


分页输出查询（这里是查看第3页的数据，每个页有5个数据）

db.test.find().skip(10).limit(5)

skip: 指定跳过指定数量的数据
limit: 指定一个页有多少条数据


投射（指定字段输出，默认输出_id，需要通过参数去除）

db.test.find({},{_id:0, title: 1,by: 1}).linit(5)



聚合

    db.test.aggregate({
        {$sort({_id: -1})}
        {$group: {_id: "$title", data: {$sum :1}}}
    })

按照id降序输出，并且按照title字段输出，每个数据累计一行输出

查看数据库信息（其中size字段为数据库的大小，默认单位为字节,这里输出的单位是MB）

db.test.stats(1024*1024)


---


登录验证（auth）


选择库

use admin

创建用户

    db.createUser({
        user:"root",
        pwd:"root",
        roles:[
            {
                role:"userAdminAnyDatabase",
                db:"admin"
            },
            {
                role:"clusterAdmin",
                db:"admin"
            },
            {
                role:"dbAdminAnyDatabase",
                db:"admin"
            }

        ]
    })

如果创建普通用户，role为readWrite和read

read：允许读数据的权限

readWrite：允许读和写数据的权限

userAdminAnyDatabase：允许管理所有数据库的用户的权限，只能用于admin用户

clusterAdmin：获取管理集群的最高权限，只能用于admin用户

dbAdminAnyDatabase： 获取管理所有数据库的权限，只能用于admin用户


root没有任何限制（吴迪）

    db.createUser(
        {
            user:"root",
            pwd:"pwd",
            roles:["root"]
        }
    )



启用auth


mongod.conf

security:
  authorization: enabled

或者

mongo.conf

auth=true


也是可以终端 mongod --auth


注意：MongoDB允许同user，但是db不允许相同，因为同用户名的用户之间是根据db参数区别的，db参数就是该用户在该数据库的权限（admin用户除外）



查看用户

show users


修改密码

db.changeUserPassword("admin", "abc123")

或者

    db.runCommand(
        {
            updateUser:"admin",
            pwd:"abc123",
        }
    )

删除用户

db.dropUser('root')

验证用户（用户鉴权）（符合1表示验证成功，0为失败）

db.auth('admin', 'abc123')

登录

mongo -u admin -p abc123 --host localhostt --port 27017 --authenticationDatabase admin

或者

mongo mongodb://admin:abc123@127.0.0.1:27017


mongo shell 基于JavaScript语法，因此可以执行JavaScript程序，其JavaScript解析器引擎为SpiderMonkey（正是Firefox浏览器同款解释器，由Mozilla提供）（注意：在3.2+版本，才是SpiderMonkey引擎，之前的版本是v8引擎）


---

索引，通过关键词来快速得到自己想要的数据

升序输出单字段索引
db.test.ensureIndex({title:1})

复合索引就是多字段组合的索引

db.test.ensureIndex({_id:1, title:1})


数组索引（tags字段是一个数组）

db.test.ensureIndex({tags:1})

注意：MongoDB不允许在复合索引中出现多个数组字段


唯一性约束（保证数据的唯一性，写入重复的数据，会导致报错（输出的字段也不是唯一的也报错），使用unique: true声明）

db.test.ensureIndex({title:1}, {unique: true})

数组索引的唯一性并不能保证数组元素的唯一性

TTL索引（数据老化处理）

db.test.ensureIndex({Date:1}, {expireAfterSeconds: 60})

创建一个TTL索引，指向Date字段，并且expireAfterSeconds: 60表示该数据60秒后过期（过期后，会进行清理该数据）

修改过期时间（已创建TTL索引）

db.runCommand({collMod:"test",index: {keyPattern:{Data:1},expireAfterSeconds: 600}})

修改过期时间为10分钟

MongoDB允许多个结构不同的数据存储在同一个数据库中（索引该数据时，不存在的字段将输出null）

稀疏索引

db.test.ensureIndex({title:1}, {sparse: true})

文本索引

db.test.ensureIndex({title: "text", by: "text"})

通过文本索引进行搜索文本（通过$text操作符输出包含$search指向的关键词的数据，对中日韩等语言支持不怎么理想，对英文支持很好）

db.test.find({$text: {$search: "chen"}})

模糊索引（假设data是一个嵌套数据，其下还有多个不同的属性）

db.test.createIndex({"data.$**": 1})

---


副本集架构（由一个主节点和多个副本节点组成）

主节点写入数据后，副本节点进行同步复制工作，确保副本节点拥有和主节点的数据副本，当主节点发生故障不可用时，副本节点将通过Raft算法选举出新的主节点（这个步骤是全自动的，无需进行额外配置）

Raft选举算法：主节点会向副节点发送心跳包，确保主节点存活，如果在一段时间内副节点没有接收到心跳包，副节点将转换为候选者，候选者得到投票数最多者成为主节点（副节点在每个选举期中只能给候选者投一票，而且只有当对方的任期时间和日志时间与自己一样新时，才进行投票给对方，如果没有得到最多数票，会重新发起选举，如果在该选举期中发现有主节点的心跳包时，进行判断对方任期是否和自己一样新，如果是则为副节点，如果不是，继续选举）

只能是当任期结束了或者主节点没有发送心跳包了才会触发选举，而且Raft使用了一种预投票的机制（preVote），通过预投票的方式来试探是否选举成功，只有当预投票通过才能发起真实的投票

MongoDB基于Raft算法进行了一些扩展，例如副节点可以选择离自己最近（心跳延时）的节点来复制数据，并不一定要向主节点复制数据

oplog同步数据，主节点当一个写操作完成时，向oplog写入一条日志，副节点通过该oplog拉取新日志，达到数据同步

注意：oplog的容量并不是无限的，是有固定大小的，当容量满了，会将旧日志滚动清理


---


分片集群架构














