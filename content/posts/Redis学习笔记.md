---
title: "Redis学习笔记"
categories: [ "默认" ]
tags: [ "Redis" ]
draft: false
slug: "97"
date: "2021-09-10 21:10:00"
---

Redis是NoSQL数据库（Not Only SQL），其特点就是基于内存运行，支持分布式，key-value存储


可以将内存中的数据存储到磁盘中，重启的时候再加载使用，保证数据的持久性，支持备份恢复，常用于缓存数据库（辅助持久化数据库）

因为其是以内存作为存储介质，因此读写数据的效率极高，读取速度可高达110000次/s，写速度高达81000次/s

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合），zset(Sorted Set: 有序集合)




---


Redis安装

编译安装

下载redis-x.x.x.tar.gz

解压

tar xzf redis-x.x.x.tar.gz

进入解压出来的文件夹中，make

因为用的是默认配置

进入src，./redis-server ../redis.conf


测试客户端

./redis-cli








---

启动服务器

redis-server


启动客户端

redis-cli


远程连接服务

redis-cli -h 127.0.0.1 -p 6379 -a "root"


---


redis.conf配置

默认端口为6379

port 6379

要远程访问则设置为0.0.0.0

bind 127.0.0.1


以守护进程运行（yes为守护进程），注意如果是以守护进程运行，那么会默认将pid写入到/var/run/redis.pid

daemonize yes

指定pid位置

pidfile /var/run/redis.pid

数据库文件

dbfilename xxx.rdb

当客户端被闲置了多久关闭链接，0为关闭该功能

timeout 0

存储到本地数据库的时候是否压缩数据，采用LZP压缩

rdbcompression yes

超时

timeout 300

密码

requirepass root



---

name键存储字符串root

SET name "root"

查询name

GET name



哈希是键值对的集合，是字段和值之间的映射

HMSET user:1 name pass

HGETALL user:1 


列表

lpush hallo abc

lpush hall xyz

lrange hallo 0 5


集合（无序集合）

sadd hallo abc

sadd hallo xyz

smembers hallo




有序集合


zadd hallo 0 abc

zadd hallo 0 xyz

ZRANGEBYSCORE hallo 0 5


---






删除键（如果删除成功，将输出1，否则输出0）

DEL hallo


删除哈希表

HDEL hallo














