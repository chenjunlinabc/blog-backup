---
title: "Redis学习笔记"
categories: [ "学习" ]
tags: [ "Redis" ]
draft: false
slug: "97"
date: "2021-09-10 21:10:00"
---

Redis是NoSQL数据库（Not Only SQL）家族的代表之一，其特点就是基于内存运行，支持分布式，key-value存储

Redis具备速度快，支持多种数据结构，可持久化，支持主从复制，具备高可用，分布式等特点

可以将内存中的数据存储到磁盘中，重启的时候再加载使用，保证数据的持久性，支持备份恢复，常用于缓存数据库（辅助持久化数据库）

因为其是以内存作为存储介质，因此读写数据的效率极高，读取速度可高达110000次/s（也就说可达到10W QPS，QPS（Queries Per Second）:每秒查询率），写速度高达81000次/s

Redis将数据存储于内存中，Redis会将数据的更新异步存储到磁盘中来持久化，Redis数据持久化有2种方式，分别是RDB（Redis DataBase）和AOF（append only file）


Redis支持多种数据结构，例如：string（字符串），hash（哈希），list（列表），set（集合），zset(Sorted Set: 有序集合)，BitMaps（位图），HyperLogLog（超小内存唯一值计数），GEO（地理信息定位）

Redis支持分布订阅，Lua脚本，事务，pipeline等功能，支持主从复制，确保高可用


Redis可以用来当做缓存系统，用户访问app应用服务时，一般来说会优先从缓存中读取，如果缓存没有再到存储介质中读取，并且将该数据会在存储在缓存中，这个缓存就是Redis




---


Redis安装

编译安装（linux）

下载redis-x.x.x.tar.gz

解压

tar xzf redis-x.x.x.tar.gz

进入解压出来的文件夹中，执行make&&make install命令

因为用的是默认配置

进入src，./redis-server ../redis.conf


测试客户端

./redis-cli

src目录下可看到6个可执行文件，作用如下：

redis-server可执行文件用于启动redis服务器

redis-cli可执行文件用于redis命令行客户端，用于连接redis服务器

redis-benchmark可执行文件用于redis性能测试，基准测试

redis-check-aof可执行文件用于修复AOF可持久化文件

redis-check-dump可执行文件用于RDB可持久化文件的检查工作

redis-sentinel可执行文件用于启动高可用的sentinel模式（该模式又叫哨兵）




使用redis并不推荐使用windows，虽然微软官方提供了redis补丁，但是redis版本太低了，建议使用Linux虚拟机或者docker容器来跑

---

redis有3种启动服务器方式

默认配置启动

redis-server


动态参数启动

redis-server --port 6380

配置文件启动（推荐，通过redis.conf文件来进行配置）

redis-server configPath



客户端连接服务（6379是redis默认端口）

redis-cli -h 127.0.0.1 -p 6379 -a "root"


---


redis.conf配置

默认端口为6379

port 6379

要远程访问则设置为0.0.0.0

bind 127.0.0.1

logfile，redis日志名

dir，redis工作目录（日志文件和持久化文件存储在哪个文件）

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







redis的通用命令

遍历输出所有key

KEYS *

输出所有以r开头的key

KEYS r*


输出所有以ra或者rb，rc开头的key

KEYS n[a-c]

输出roo?，?号位占位

KEYS roo?

KEYS不推荐使用，因为KEYS命令的时间复杂度为O(n)

输出key的总数（时间复杂度为O(1)，不需要读取全部key，redis会提供一个计数器来实时更新记录key的总数）

DBSIZE

判断一个key是否存在（真返回1，假返回0）

EXISTS a

删除指定key（可删除多个）

DEL a b

设置a将在10秒后过期

EXPIRE a 10

查询a将在多少秒后过期

TTL a

移除a的移除时间

PERSIST a

查询a的类型

TYPE a


上面几条命令除了KEYS不是O(1)外，其他都是O(1)

清除全部数据（key）

FLUSHALL

---


Redis的命令执行是单线程（例如网络请求，数据操作）

一次只能执行一条命令，如果某条命令执行过长时间，会导致阻塞


---

字符串

注意：key不要使用单引号双引号来表示双引号，没有必要

name键存储字符串root

SET name root

name为键（key），root为name键的值（value）

当key不存在时添加value值（存在则不进行操作）

SET name 1 NX

当key存在时添加value值

SET name 1 XX

设置key的过期时间（单位为秒，这里表示10秒钟，可使用TTL查看过期时间）

SETEX name 10 1

设置key的过期时间，单位为毫秒

SET name 1 PX 1000

获取name的值

GET name

删除name

DEL name

将a的值自增1

GET a 1
INCR a

将a的值自减1

DECR a

将a的值自增指定值，这里就是a的值加99

INCRBY a 99

将a的值自减指定值

DECRBY a 99


批量获取多个key

MGET a b c

批量设置多个key-value

MSET a 1 b 2 c 3

设置新的value，并且返回 旧的value

GETSET a 1

将新的value追加到旧的value中去

APPEND a 2

输出字符串的长度（注意中文，一个中文占2个字节，时间复杂度也为O(1)）

STRLEN a

增加key的值，如果a的值为1的话，就是1+3.14

INCRBYFLOAT a 3.14

输出0到2下标的字符串

GETRANGE a 0 2

将第4个下标的字符串修改b（因为字符串下标从0开始）

SETRANG hallo 3 b

上面指令中除了MGET和MSET外（为O(n)），时间复杂度都为O(1)



---


哈希是键值对的集合，是字段和值之间的映射（field（就是字段）不能相同，而value允许相同）

设置哈希表user（name为field，root为value）
HSET user name root

获取哈希表user中指定name字段的值
HGET user name

获取哈希表user中的全部字段和值
HGETALL user

删除哈希表user中的name字段
HDEL user name

判断哈希表user中是否存在name字段
HEXISTS user name

输出哈希表user中存在多少个字段
HLEN user

一次添加多条字段（时间复杂度为O(n)）
HMGET user name root pass 123

一次获取多条字段的值（时间复杂度为O(n)）
HMSET user name pass

输出user中的全部字段和值（时间复杂度为O(n)）
HGETALL user

输出user的全部值（时间复杂度为O(n)）
HVALS user

输出user中的全部字段（时间复杂度为O(n)）
HKEYS user


---


列表（有序，可重复，左右两端都可以插入值）

将值插入hallo列表的最左端中（根据插入的个数决定时间复制度，实质效果就是 c b a，因为是从左端插入的）
LPUSH hallo a b c

值将从hallo列表的最右端中插入（根据插入的个数决定时间复制度）
RPUSH hallo a b c

在指定值之前插入一个新的值，这个表示b插入到a的前面 (时间复杂度为O(n))
LINSERT hallo BEFORE a b 

在指定值之后插入一个新的值，这个表示b插入到a的后面 (时间复杂度为O(n))
LINSERT hallo AFTER a b c

从左边移除一个key
LPOP hallo

从左边移除一个key
RPOP hallo

删除全部于a值相等的（为0是相等，大于0为从列表的最右边删除以a值相等的指定数据的值（例如LREM hallo 1 a，就是从右侧（列表底部）删除1个和a值相等的值（因为列表允许存在重复的值，可用于去重）），小于0为从列表的最左侧删除于a值的值（例如-1就是删除最左侧删除和a值的相等的1个值（绝对值）））
LREM hallo 0 a

栽剪1到3的值（0以及3之后的值将被移除）
LTRIM hallo 1 3

查询0到5的值，正值是从左到右获取（从0开始），负数就是从右到左查（-1开始）（O(n)）
LRANGE hallo 0 5

获取指定索引的值(O(n))
LINDEX hallo 0

获取列表的长度
LLEN hallo

设置1索引的值为a
LSET hallo 1 a

获取列表的第一个元素（最左边）（会输出第一个元素的key，以及值），如果列表不存在元素则阻塞列表，一直到等待指定时间后返回nil，这里是等待10秒
BLPOP hallo 10

删除列表的最后一个元素（最右边）（会输出最后一个元素的key，以及值），如果列表不存在元素则阻塞列表，一直到等待指定时间后返回nil，这里是等待10秒
BRPOP hallo 10


--

集合（无序集合，集合的特定就是没有重复元素，集合是利用哈希表实现的，因此添加，删除，查找的复杂度都是O(1)）

SADD hallo abc

SADD hallo xyz



删除集合（将abc从hallo集合中删除）

SREM hallo abc

输出指定集合中的元素个数

SCARD hallo

判断'abc'是否存在于hallo集合中（真返回1，假返回0）

SISMEMBER hallo 'abc'

随机输出指定集合的元素，其中count参数是随机输出几个元素

SRANDMEMBER hallo count = 2

输出指定集合的全部元素

SMEMBERS hallo 

查找第一个集合和其他集合之间不相同的元素（差集）

SDIFF hallo hhh

将多个集合之前不相同元素存储到另一个集合中去，例子中是将hallo和hhh集合中不相同的元素存储到abcxyz集合中去

SDIFFSTORE abcxyz hallo hhh

输出指定集合中的相同的元素（交集，指定集合中存在相同的元素）

SINTER hallo hhh

输出指定集合的并集，就是将多个集合合并成一个集合

SUNION hallo hhh

随机从集合种移除指定个数的元素，如果不指定个数，默认移除1个元素

SPOP hallo 2


有序集合（根据第二个参数（分数）来排序）（ZADD时间复杂度为O(logN)）

ZADD hallo 0 abc

ZADD hallo 1 xyz

删除指定元素（可多个）
ZREM hallo abc

获取abc的分数
ZSCORE hallo abc

增加或者减少abc的分数，这里是加1（负数为减）
ZINCRBY hallo 1 abc

获取hallo的元素个数
ZCARD hallo

获取abc在hallo中的排名（从0开始，正序，从小到大的排名）
ZRANK hallo abc

逆序排序（从大到小，例子是表示逆序输出得到为0到5之间的集合元素）

ZRANGEBYSCORE hallo 0 5

逆序排序输出集合的全部元素

ZREVRANGEBYSCORE hallo +inf -inf

正序输出分数在0到10之间的元素

ZRANGEBYSCORE hallo 0 10 WITHSCORES

有序输出集合的全部元素，WITHSCORES参数为输出时携带得分

ZRANGE hallo 0 -1 WITHSCORES

有序输出集合的索引为0到5之间的元素（第一个到第六个元素，-1表示最后一个元素，-2是最后面的第2个元素，以此类推）

ZRANGE hallo 0 5 WITHSCORES

获取hallo集合中分数0到100的元素有多少个

ZCOUNT hallo 0 100

将分数排名第一个的元素到第三个元素移除
ZREMRANGEBYRANK hallo 0 2

将分数100到300之间的元素移除
ZREMRANGEBYSCORE hallo 100 300

获取abc在hallo中的排名（逆序，从大到小的排名，排位0的值的分数最大）
ZREVRANK hallo abc

获取hallo中第一个元素到第三个元素的值（带分数），先根据分数从大到小排序，再获取
ZREVRANGE hallo 0 2 WITHSCORES

获取指定有序集的交集（这里3表示有序集中存在4个值，就是每个有序集提供4个值，这里指定了2个有序集，就是4+4，hallo交集中就有8个值），并且存储到hallo交集中
ZINTERSTORE hallo 3 xyz hhh

获取指定有序集的并集（其中WEIGHTS后面的参数分别表示对应有序集的乘法因子，例如xyz中第一个元素得分为2，那么就是2x2，就是4，而这个4被做为该值的得分传入到新的hallo并集中，并集将集合合并在一起，AGGREGATE后面的参数表示集合的聚合方法，默认为SUM（就是将所有指定集合中的某个元素的分数的和作为并集中该成员的分数，简单来说就是将集合中相同元素的分数相加，得到的和作为并集，也就是新创建的集合中作为该元素的分数），除了SUM外还有MIN（获取全部集合中相同元素的得分取自这些相同元素中最小的得分作为新集的该元素的得分），MAX（获取全部集合中相同元素的得分取自这些相同元素中最大的得分作为新集的该元素的得分））
ZUNIONSTORE hallo 3 xyz hhh WEIGHTS 2 3 AGGREGATE MIN




---



慢查询（特点：先进先出队列，固定长度，保存于内存中）

慢查询就是寻找长时间执行的命令，并且将该命令存储在日志中，慢查询只能记录命令执行的时间

慢查询利用2个参数来实现的，这2个参数分别为slowlog-max-len和slowlog-log-slower-than

其中slowlog-log-slower-than是记录执行超过指定时间的命令，单位为微秒，默认值为10000

slowlog-max-len是限制慢查询日志的条数（慢查询日志的数据结构为列表，默认值为128，因为该慢查询日志是存储在内存中，重启redis会丢失日志的记录）

当慢查询日志达到阈值时，如果还继续添加新的慢查询进来，将会将当前日志中最早的慢查询删除（先进先出）

通过SOLWLOG GET命令查看慢查询日志

慢查询日志的结构有6个值来表示1个慢查询，分别为慢查询的唯一ID，该命令执行的时间戳，慢查询的命令和参数，客户端地址和端口，客户端名称（通过client setname设置）


查看存在多少条慢查询

SOLWLOG LEN


获取指定多少条慢查询

SOLWLOG GET 1

清除慢查询的全部记录

SOLWLOG RESET

---


pipeline流水线机制

redis的命令执行由发送命令，再到命令进行排队（无法确定下个执行的命令是哪个），命令执行，最后返回结果

这个命令的过程又叫Round trip time，RTT，往返时间

pipeline流水线机制就是减少RTT的

pipeline就是将命令一次性通过网络获取多条命令，然后再执行多条命令（打包组装）

注意：原生批命令(mset, mget)是原子性，而pipeline是非原子性，而且应该也必须限制pipeline的执行命令的个数


---

发布订阅


发布1到hallo频道中去
PUBLISH hallo 1

订阅hallo频道
SUBSCRIBE hallo

取消订阅hallo频道
UNSUBSCRIBE hallo

订阅全部以h开头的频道（定义模式）
PSUBSCRIBE h*

取消订阅全部以h开头的定义
PUNSUBSCRIBE  h*

查看至少有一个订阅的频道并且要以h*开头（也就是活跃的频道）
PUBSUB CHANNELS h*

输出指定频道的订阅数
PUBSUB NUMSUB hallo

输出当前订阅模式的数量
PUBSUB NUMPAT

---


Bitmap位图（位图就是位的映射，使用一个bit位来表示某个元素的值）

位图的每个值只占用一个bit位，位图本质上还是字符串类型

1个字节由8个二进制位表示，例如字符串a的二进制为01100001

创建hallo位图
SET hallo a

查询b字符的八位二进制码（得到01100010）
BIN(ORD('b'))

将a值改为b，只需要将最后一位改为0，倒数第二位改为1，例如：

给hallo位图的最后一位设置0值
SETBIT hallo  7 0

给hallo位图的倒数第二位设置1值
SETBIT hallo  6 1

通过执行GET hallo命令，可以看到已经变成了b


获取hallo位图中第一位的值
GETBIT hallo 0

获取hallo位图中第一位到第八位之间值为1的个数（如果不指定就是获取整个位图中值为1的个数）
BITCOUNT hallo 1 8

将abc位图和xyz位图交集，并且将得到的新集存储在hallo位图中（and交集，or并集，not非集，xor异或集）
BITOP AND hallo abc xyz

获取hallo位图中中第一位到第八位之间第一个值为1位置（偏移量）
BITPOS hallo 1 1 8








---

hyperloglog


hyperloglog（使用极小的空间来完成计数，本质还是字符串，特点就是输入的数量很多或者值有多大，占据的空间都是固定的（一个HyperLogLog键只使用12KB内存））

向hyperloglog添加值
PFADD hallo a b c

查看hyperloglog中存在多少个值（注意：返回的是基数，就是hyperloglog中存在相同的值时，会被忽略）
PFCOUNT hallo

合并2个（也可多个）hyperloglog到hallo
PFMERGE hallo abc xyz

hyperloglog的缺点就是错误率高（官方提供的错误率值是0.81%），无法获取单个数据






---



geo


GEO（地理信息定位），用于存储经纬度，计算两地距离，范围计算等等

添加经纬度到hallo（guangzho是标识）
GEOADD hallo 113.27 23.13 guangzho
GEOADD hallo 114.07 22.62 shenzheng

从GEO获取经纬度
GEOPOS hallo guangzho

获取两个经纬度之间的距离（m米，km千米，mi英里，ft尺）
GEODIST hallo shenzheng guangzho

获取guangzho附近1000千米的标识信息
GEORADIUS hallo guangzho 1000km





---



持久化（redis数据存储于内存中，持久化功能可将数据的更新异步存储在硬盘中）

RDB（快照），AOF（日志）

持久化的RDB和AOF


RDB（redis database）：Redis默认持久化策略，通过内存快照来实现持久化

RDB有3种模式，一个是通过sava命令来同步触发redis进行RDB文件（RDB文件是二进制文件）生成，另一种就是通过bgsave命令来异步进行持久化文件RDB生成（该命令会让redis server fork一个子进程来进行该操作，这样主进程可以继续处理请求，不会阻塞），最后一个就是通过redis.conf配置文件来配置RDB自动生成，例如save 900 1，这个命令表示900秒内执行了一次set命令就生成一次RDB持久化文件（自动触发）

注意：如果之前存在RDB文件，执行sava命令，会生成一个新的RDB文件去覆盖这个老的RDB文件

触发第一种模式很简单，就执行sava命令（时间复杂度为O(n)），redis接收该命令，就会立刻去创建生成RDB文件


触发第二种模式，也很简单，也是就执行bgsave命令（时间复杂度为O(n)），不过redis接收该命令后会利用Linux的fork()函数来生成一个子进程，让这个子进程来处理生成RDB文件

save和bgsave的区别就是，save会阻塞命令的执行，bgsave需要另一个进程来处理（消耗内存）

第三种模式通过配置文件来自动触发，当满足条件时，创建RDB文件是通过bgsave命令完成的

配置文件参数还有dbfilename（来表示rdb文件名，必须是.rdb后缀），以及上面提到的dir（来表示rdb持久化文件放到哪个文件夹中），stop-writes-on-bgsave-error（如果bgsave命令发生错误是否停止写入，yes或者no），rdbcompression（rdb文件是否使用压缩模式），rdbchecksum（是否对rdb文件进行检验操作）

恢复数据也很简单，将ADB文件放到redis安装目录下，然后重启（redis会在启动的时候根据RDB文件来恢复数据），可执行CONFIG GET dir来得到redis安装目录地址



RDB的缺点就是耗时（生成RDB文件慢，因为需要复制全部数据，时间复杂度是O(n)），内存消耗大（fork进程），硬盘资源消耗大（当数据很庞大时），而且不可控制（无法指定什么时间备份），存在丢失数据的风险（当自动备份没有触发，而且也没有手动触发时宕机，就会导致数据丢失了）


---




AOF（每执行一次写命令（例如：SET，HMSET，SADD），就会将该命令存储在AOF文件中，类似于MySQL的sql备份）


通过redis.conf配置文件来配置AOF，appendonly（是否启动AOF持久化模式），appendfilename（AOF持久化文件名称，必须是.aof后缀），appendfsync（AOF的三种持久化策略：always（每次执行写命令时间都会将该命令同步到AOF可持久化文件中），everysec（每秒将缓存区中的命令同步到AOF可持久化文件中），no（不需要将缓存区的命令同步到AOF可持久化文件中，让操作系统来决定什么时候同步，Linux系统默认30秒将缓存区数据写入）），no-appendfsync-on-rewrite（表示是否在同步期间对新写命令不进行同步，先将其存储在缓存区，等同步完成在写入，如果no的话，会阻塞appendfsync，如果设置yes可能存在会丢失数据的风险（新写命令没有持久化，Linux默认每隔30秒执行一次缓存区同步到硬盘的操作）），auto-aof-rewrite-percentage（因为AOF是使用追加的方式的，因此AOF可持久化文件会越来越大，因此存在AOF重写机制（简化AOF文件里面命令，例如去除过期命令，将多条命令合并成1条命令等等，得到新的AOF后，将老的删除，写入新的），当满足某个阀值时，会启动AOF重写，该参数是表示当前AOF文件大小是上次的大小的多少倍触发（单位为百分比，100就是指一倍），auto-aof-rewrite-min-size（当AOF大于该阀值时触发，例如64mb），aof-load-truncated（因为AOF文件可能是不完整的（宕机或者异常导致的不完整），表示是否在恢复数据时自动进行AOF修复操作，no表示不，需要手动redis-check-aof修复，yes表示是的），dir参数和RDB介绍一样的作用（AOF文件存储在哪个目录）

手动修复命令redis-check-aof --fix

手动AOF重写命令bgrewriteaof（执行到该命令会让redis服务器fork一个子进程来进行AOF重写操作，不会阻塞命令执行，而且只有当完成之后创建一个新的AOF文件来覆盖老的AOF文件（不存在bgrewriteaof命令执行失败后，导致数据丢失的情况，必须时执行成功后再覆盖））

注意：auto-aof-rewrite-percentage和auto-aof-rewrite-min-size，必须都满足这2个设置的阀值才会重写（避免AOF文件很小也导致触发）

注意：每次执行写命令时间并不会立刻同步到AOF可持久化文件中去，而是先写入缓存区，再根据策略来同步到AOF可持久化文件，每个策略都有自己的缺点和优点，always不怕丢数据，但是IO消耗大，everysec对硬盘友好，但是存在丢失那1秒数据的风险，no不可控制什么时候同步，但是性能高


AOF恢复数据和RDB一样，将AOF文件放到Redis安装目录下





---






优化fork

fork()操作是同步的，内存越大耗时越慢，可通过latest_fork_usec命令查看最近一次fork操作的耗时（单位微秒）来进行排查，例如：redis-cli  info latest_fork_usec

maxmemory配置Redis实例最大可用内存，例如：CONFIG SET maxmemory 100MB或者修改redis.conf的maxmemory参数

通过配置Linux内存分配策略，例如：vm.overcommit_memory = 1（1表示内核允许超量使用内存，直到用完，0表示内核会检查是否还有足够的内存，如果有则对内存申请通过，否则失败，2表示内核不会超量使用内存，整个内存空间不会超过swap+50%的RAM，课外小知识：Linux内核会批准大部分的申请内存的请求，但是分配到了内存后，不能使用内存，只有当内存赋值时才分配内存，这个技术叫overcommit，如果策略允许超量使用时，也就是允许overcommit，当内存分配不足时，会导致OOM killer，通过杀死用户态进程来释放内存）

通过提高AOF或者RDB自动触发生成的阀值来降低fork的频率，非不必要时不进行自动触发


优化子进程


Copy on write机制：fork会创建一个除了pid不同外和父进程完全相同的子进程，但是这些数据是对子进程无用的（因为子进程使用exec()来实现自己的功能时，会清空这些数据），Copy On Write可以让子进程和父进程共享内存空间（直接引用父进程的内存空间，这样子进程更轻量化了，实质上在没有exec()之前，子进程和父进程使用的是同一块物理内存，只是虚拟内存不同，当exec之后，才会给子进程分配一个独立的物理内存）

该机制的优点就是减少不必要的内存分配，缺点是当fork之后父进程或者子进程进行写操作，会导致页异常中断（page-fault），因为这个时候子进程和父进程都还是同一块物理内存，最后因为复制数据导致资源浪费


AOF追加阻塞（everysec策略，每1秒进行一次同步操作）


因为主线程会对比AOF的同步时间，如果上一次同步时间在2秒，则正常，超过2秒后，主线程将会导致阻塞，一直到同步操作完成，因此everysec策略最多可以丢2秒的数据





---





主从复制（提供数据副本（数据灾备），扩展读性能（读写分离））

从节点复制主节点的数据（一从一主或者一主多从）


主从复制配置（有2种）

slaveof命令（在从节点中运行该命令，例如slaveof 192.168.1.2 6379，就能复制主节点192.168.1.2:6379的数据了，而且主节点的数据更新都能同步到从节点中）

将该节点取消从节点
slaveof no one

配置（redis.conf）

配置主节点
slaveof 192.168.1.2 6379

如果需要设置从连接主的密码，需要设置masterauth参数，例如masterauth "123456"或者执行命令config set masterauth "123456"

如果需要设置密码，可以设置requirepass参数，例如requirepass "123456"或者执行命令config set requirepass "123456"

如果想设置从节点只读，可以设置slave-read-only yes，如果设置no，写入的数据也会在主从同步完成后被删除





全量复制和部分复制


全量复制就是将主节点的全部数据一次性发送给从节点复制

部分复制就是主从复制过程导致数据丢失了，从节点在连接到主节点后，主节点可以将丢失的数据发送给从节点，从而避免全量复制的过高的资源消耗


runid（redis服务器的标识符（用于区分redis服务器），一重启redis服务器后runid都会发生改变）

可通过info server | grep run命令来查看

复制偏移量就是命令的字节长度，可以用于判断主从节点数据是否一致，可使用info replication命令查看


主节点会记录自身的复制偏移量（master_repl_offset，可通过info replication查看），从节点也会每秒将自身的复制偏移量发送给主节点（slave的offset），这样主节点就可以记录从节点的复制偏移量








---





Sentinel架构（不可存储数据，对redis节点的故障判断，故障master转移处理以及通知客户端该节点状态）


故障转移：当多个sentinel发现master存在故障，会通过选举算法选举出一个sentinel，然后该sentinel会选择一个slave从节点来当master主节点，最后通知其他slave从节点成为该新的master的从节点并且通知客户端主从节点的变化（如果出现故障的master恢复后会成为新的master的从节点）


安装Sentinel和配置

开启主从节点，然后开启Sentinel监控主节点


Sentinel配置文件（sentinel.conf，redis配置文件的参数可用于Sentinel配置文件（例如port，dir都是可以的，Sentinel就是特殊的redis））


    sentinel monitor hallomaster 127.0.0.1 6379 2
    sentinel down-after-milliseconds hallomaster 30000
    sentinel parallel-syncs hallomaster 1
    sentinel failover-timeout hallomaster 180000


第一行是该Sentinel监控的master节点的信息，hallomaster是master节点名称，master节点IP，master节点的端口，以及当多少个Sentinel认定该master节点存在故障时进行故障转移

第二行Sentinel监控master节点故障多少毫秒后认定故障

第三行表示发送故障转移时最多能有几个从节点对新的master进行同步（越少，新的master压力越小）

第四行表示故障转移时最多允许多少毫秒，当超过时，认定故障转移失败


启动Sentinel监控节点

    redis-sentinel redis-sentinel.conf


注意：因为选举算法的原因，Sentinel监控节点最少要有3个，如果只有2个Sentinel监控节点，发生故障，因为投票机制问题（要选一个Sentinel监控节点来当Sentinel监控主节点，让这个Sentinel监控节点来确定哪个从节点来当主节点），只有当Sentinel监控集群超过一步的节点检测失效才会生效，而且Sentinel监控节点也发生故障了，导致故障转移无法生效，而且Sentinel监控节点数最后是奇数，避免投票偶数出现相同票数的情况



Sentinel监控节点定时任务

Sentinel监控节点每10秒对master和slave执行info（用来发现slave以及主从关系的确认），每2秒对master节点的channel进行交换信息（订阅发布，频道名为__sentinel__:hello），每1秒对其他Sentinel以及redis主从节点执行ping


客观下线和主观下线，以及领导者选举

客观下线

sentinel monitor hallomaster 127.0.0.1 6379 2



主观下线

sentinel down-after-milliseconds hallomaster 30000

当主观下线条件满足后，会执行sentinel is-master-down-by-addr命令来询问其他Sentinel监控节点是否确定master已经下线了

sentinel is-master-down-by-addr 127.0.0.1 6379 0 *

每个做主观下线的Sentinel监控节点向其他Sentinel监控节点发送该命令，并且要求设置该为领导者

如果接收该命令的Sentinel监控节点没有同意过其他Sentinel监控节点的请求，那么将同意，否则拒绝（在周期内）


如果Sentinel监控节点发现自己的票超过了Sentinel集合的半数，并且也是超过了客观下线的条件时，该监控节点成为领导者

如果有多个Sentinel监控节点成为领导者，将会在一段时间后重新选举

领导者节点会从slave节点中选择一个来当master节点，对该slave节点执行slaveof no one命令来让它成为新的master节点，然后对其他slave节点发送命令，来让其成为新的master节点的slave节点

然后对故障的master节点配置为slave节点，并且进行监控，当其恢复则发送命令让其去复制新的master节点


领导者节点选择slave节点当master节点的机制

选择slave-priority（slave节点优先级）最高的slave节点，存在返回，不存在继续选择

选择复制偏移量最大的slave节点（复制master节点最完成的），存在返回，不存在继续选择

选择runID最小的slave节点（也就是最早的slave节点）



手动故障转移master节点（当需要对master节点进行下线之前的操作）

sentinel failover hallomaster






---



redis cluster




Cluster（集群，解决QPS（或者OPS）无法满足业务需求，硬件无法满足业务需求）


数据分区（常用的有顺序分区，哈希分区）

顺序分区：按照顺序（id或者时间等等顺序）进行划分Redis实例，缺点是数据分布不均匀

哈希分区：具有随机性，数据分布均匀，缺点是无法进行顺序访问


哈希分区有节点取余分区，一致性哈希分区，虚拟槽分区

节点取余分区，一致性哈希分区都是客户端分片，在客户端进行计算


节点取余分区：hash(key)%nodes，优点：简单，缺点：增加或者减少节点需要重新计算来重新映射（推荐采用翻倍扩容来降低影响），重新计算将导致重新计算数据偏移量，节点数越多，花费的时间越长


一致性哈希分区：hash(key)%2^32，将哈希构造成一个0~2^32的token环形结构，节点的范围在该环形结构内，因为该数值足够大，增加或者减少节点不会导致重新计算哈希的问题

数据哈希化后，会根据顺时针方向来寻找自己的node节点（简单来说就是将全部数据构造成一个环形，分区就跟切蛋糕一样，该蛋糕是该node节点的，那个蛋糕是那个node节点的）

增加或者减少节点，将导致数据范围减少或者增加，但是不会像节点取余分区一样，需要重新计算，重新分配数据，数据的偏移量和节点数有关，节点数越多，数据的偏移量越少（10个节点，数据的偏移量只有10%，100个节点，数据的偏移量只有1%），只会影响邻近节点（也可使用翻倍伸缩）


虚拟槽分区（客户端计算hash，在服务端进行管理节点，槽以及数据）：Redis内置了16384个槽，每个槽都可映射成单个节点（当然节点不可能用这么多，一般都是节点使用多个槽，将16384个槽平均分配给每个节点，CRC16(key)&16383，每个节点都知道自己负责哪个槽，其他节点负责哪个槽（共享节点信息））



搭建集群


集群的部署有原生命令部署和官方工具（redis-trib.rb）部署这2种

原生命令安装

配置redis文件（redis-192.168.1.2.conf）

    cluster-enabled yes
    cluster-config-file nodes-192.168.1.2.conf
    cluster-node-timeout 15000
    cluster-require-full-coverage no


cluster-enabled yes表示该redis节点是集群的节点，cluster-config-file为集群运行时，记录该节点的配置以及其他节点的信息状态等等，cluster-node-timeout表示集群中节点失联的最大时间（单位为毫秒），当超过该时间会认定其故障，cluster-require-full-coverage表示是否需要集群的全部节点服务（也就是说为yes时如果只有一个节点故障了，那么这个集群就不可用了）


开启redis

    redis-server redis-192.168.1.2.conf

添加集群节点（只需要在一个节点上配置，因为节点之间存在通信交互，当a节点和b节点连接后，a又和c节点连接，b节点知道c节点的存在后彼此感知着对方）

    cluster meet 192.168.1.2 6379


分配槽（必须将0到16383的16384个槽全部分配完才能处于上线工作状态，这里设置了3个集群主服务）

192.168.1.2
    cluster addslots {0...5461}

192.168.1.3
    cluster addslots {5462...10922}

192.168.1.4
    cluster addslots {10923...16383}


设置主从关系（故障转移，集群复制，这里是3个主节点和3个从节点来保证高可用）

192.168.1.5
    cluster replicate 主节点的node-id

192.168.1.6
    cluster replicate 主节点的node-id

192.168.1.7
    cluster replicate 主节点的node-id


node-id通过cluster nodes命令获取


查看集群节点的状态

    cluster info

查看集群节点的槽分配情况

    cluster slots

测试集群上线情况（-c是以集群模式连接）

    redis-cli -c -p 6379
    set hallo word

返回ok则认为集群已上线



redis-trib.rb是redis官方推出的redis集群管理工具（因为是用ruby编写成的工具，需要安装ruby和rubygem redis客户端）


集群伸缩


故障发现，故障恢复




---



集群完整性

带宽消耗

pubsub广播

集群倾斜和数据倾斜，以及请求倾斜

读写分离

数据迁移

集群和单机


---

缓存更新策略


缓存粒度，穿透，无底洞问题


















