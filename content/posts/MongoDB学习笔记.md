---
title: "MongoDB学习笔记"
categories: [ "学习" ]
tags: [ "MongoDB" ]
draft: false
slug: "76"
date: "2021-08-08 14:20:00"
---

MongoDB是一个以键值对存储数据的数据库（基于json描述数据，实质上是一个叫BSON的数据格式，BSON是基于二进制字节流，json基于文本）

MongoDB是No SQL家族的成员之一，No SQL一般指的是非关系型数据库（Not only SQL）

关系型数据库和Excel表格类似，表与表之间存在着复杂的关联关系，例如MySQL，sql server

而非关系型数据库不使用SQL作为查询，不需要遵循ACID（Atomicity Consistency Insolation Durability）

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



聚合查询


$match：过滤，类似于sql的where

$project：投影，类似于sql的as

$sort：排序，类似于sql的order by

$group：分组，类似于sql的group by

$skip/$limit：结果限制，类似于sql的skip/limit

$lookup：左外连接，类似于sql的left outer join



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

MongoDB的选举：具备投票权的节点之间互相发送心跳（默认每2秒发送一次心跳包），当5次心跳没有接收到时，认定该节点失联，如果失联是主节点，会发起选举，成为候选者，如果失联的是副节点，则不会发起选举，成为候选者后，其他节点只有当对方的oplog日志和自己一样新时，投票给对方，一个节点一个选举期内只能投一票，候选者票数占大数时为主节点，如果主节点突然存活了，则判断该节点的oplog日志是否和自己一样，如果不是则继续选举，如果是则放弃选举，重新恢复到副节点身份

oplog同步数据，主节点当一个写操作完成时，向oplog写入一条日志，副节点通过该oplog拉取新日志，达到数据同步

当主节点发生修改，插入，更新，删除等操作时，主节点会将这些操作记录下来，而这个记录叫oplog

副节点通过在主节点上打开tailable游标来获取主节点的新oplog，并且将oplog的记录在自身回放，来保持于主节点的数据一致

注意：oplog的容量并不是无限的，是有固定大小的，当容量满了，会将旧日志滚动清理

副本集架构在实现高可用的同时，也可以完成读写分离，数据分发，异地容灾

一个副本集架构最少需要3个节点（并且节点都具备投票权）：一个主节点，2个副节点


主节点接受写入工作，副节点承担复制主节点的数据工作

副本集最多50个节点，具备选举投票权节点最多7个，其他节点则不参与投票


成为主节点的节点选举必须是集群中大多数节点存活（选举投票权节点有7个时，最少要有4个选举节点存活），并且该节点能于多数节点连接通信，而且具备较新的oplog，而且还可以设置优先级来影响选举结果


副本集节点的选配设置：是否具备投票权（v），优先级（优先级为0则表示该节点不会成为主节点，优先级越高则优先成为主节点，priority），隐藏（对应用不可见，但是可以复制主节点数据，隐藏节点可以具备投票权，但是优先级必须为0（不能成为主节点）hidden），延迟（复制多少秒之前的数据，保持于主节点的数据时间差（避免主节点数据因为误操作而导致副本集节点也跟着更新了），slaveDelay）


副本集节点注意：副节点存在成为主节点的可能，因此不要将一些副节点的硬件配置设置很低，应该保持和主节点的配置一致，而且硬件尽量保持独立性，副本集节点软件版本应保持一致，增加副本节点不会提高副本集架构的写入性能，副本集的写入由主节点完成，主节点只能有一个，增加副本节点只能提高读的性能（读写分离，主写，副读）




搭建mongodb副本集

每个mongodb实例需要一个数据目录来存储数据文件，每个mongodb实例需要一个配置文件和一个日志文件


副本集节点mongod.conf实例：

    systemLog:
        destination: file
        path:  "/mongodb1/mongodb.log" # 日志文件路径
        logAppend: true # 实例重新启动时，将写入日志的末尾
    storage:
        dbpath: "/mongodb1/data" # 数据存储目录
        journal: 
            enabled: true # 启用持久性日志
    processManagement:
        fork: true # 使用守护进程（独立进程）的模式运行实例
        pidFilePath: "/data/mongodb.pid" # 保存实例进程id的文件路径，会将pid写入到该文件中
    net:
        bindIP: 0.0.0.0 # 0.0.0.0表示所有网卡都监听，如果设置为127.0.0.1，外界无法访问
        port: 27017
    replication:
        replSetName: a1 #副本集节点名称



以配置文件的模式启动mongodb实例

mongod -f db/mongod.conf

查看mongod进程运行情况

ps -ef | grep mongod


配置副本集

进入一个节点，初始化，再将节点一个个传进副本集

    rs.initiate()
    rs.add('mongodb1:27017')
    rs.add('mongodb2:27017')
    rs.add('mongodb3:27017')

mongodb1需要被解析（hosts文件配置主机名）

或者节点初始化时就配置好

    rs.initiate({
        _id: 'rs0',
        members: [{
            _id: 0,
            host: '196.168.1.2:27017'
        },{
            _id: 1,
            host: '196.168.1.3:27017'
        },{
            _id: 2,
            host: '196.168.1.4:27017'
        }]
    })


验证副本集是否工作：在主节点上写入，然后在副节点上读，如果读到则表示工作正常


---


分片集群架构


--



数据模型设计

数据模型是由一组符号，文本组成的集合，用来表达信息

组成一个数据模型的元素有实体（Entity），属性（Attribute），以及关系（Relationship）

传统的模型设计：概念模型（CDM），逻辑模型（LDM），物理模型（PDM）


JSON文档模型设计（通过字段或者集合来表示关系）


---


事务开发


mongodb提供了事务来保证数据不丢失


写入操作事务

writConcern决定一个写入操作落到多少个节点才算成功，取值为0时表示不关心，1至集群max节点数表示写入操作被复制到指定节点数才算成功，majority表示写入操作被复制到大多数节点才算成功，all表示全部节点都被写入时认定成功

注意：当设置非0的值时，发起写入操作将导致阻塞，一直到认为成功为止

journal决定这个节点写入操作如何才算成功，true表示写入操作落到journal文件中时认定成功，false表示写入操作落到内存时就认定成功

db.test.insert({count: 1}, {writConcern: {w: "majority"})

读取操作事务

readPreference决定数据从哪个节点读，primary表示只选择主节点读，primaryPreferred表示优先选择主节点，如果不可用则选择副节点，secondary表示只选择副节点，secondaryPreferred表示优先选择副节点，如果副节点不可用，则选择主节点，nearest表示选择最近的节点（通过ping延迟来选择最近）

在连接时设置

mongodb://196.168.1.2:27017,196.168.1.3:27017,196.168.1.4:27017/?replicaSet=rs&readPreference=nearest

或者在mongo shell设置

db.collection.find({}).readPref('nearest')



readConcern决定什么数据可以读，available表示读取全部可用数据，local表示全部可用并且熟悉当前分片的数据，majority表示读取在大多数节点上完成的数据，linearizable表示可线性化读取（只读取大多数节点确认的数据（会等待其他节点去更新，最后读取最新的数据，比majority更安全，但是读取数据时很慢，需要等待节点更新）），snapshot表示读取最近快照中的数据（只读同一个快照下的数据）


mongodb通过维护快照来链接不同版本的数据（MVCC机制），被大多数节点确认过的版本都是快照，快照一直持续没有被使用才会被删除

安全的读写分离：搭配writConcern和readConcern，writConcern和readConcern都设置为majority，表示数据写入安全和数据读取安全


事务的ACID：原子性，一致性，隔离性，持久性

事务完成之前，事务外的操作对该事务的修改不可访问
















