---
title: "MySQL学习笔记"
categories: [ "学习" ]
tags: [ "mysql" ]
draft: false
slug: "20"
date: "2021-06-16 09:37:00"
---

推荐使用mariaDB

操作数据库

创建数据库

CREATE DATABASE xxx;

删除数据库

DROP DATABASE xxx;

查看所有数据库

SHOW DATABASES;

查看指定数据库

SHOW xxx;

打开指定数据库

USE xxx;

查看指定数据库的详细信息

SHOW CREATE DATABASE xxx;

修改指定数据库的字符集

ALTER DATABASE xxx CHARACTER SET gbk COLLATE gbk_bin;

数据表的校对规则，与于指定数据集如何排序

COLLATE=utf8_bin

指定字符集

CHARSET=utf8

查看当前mysql所支持的字符集

SHOW CHARACTER SET;


新建数据表

    CREATE TABLE xxx(
        id int(4),
        email char(20),
        status int(2),
        code varchar(10)
​    );

添加数据

    INSERT INTO xxx VALUES(
        1,
        "hallo",
        "1",
        "xxx"
    );

修改数据(根据条件)

UPDATE xxx SET status=1 WHERE id = 4;


删除数据(根据条件)

DELETE FROM xxx WHERE id = 5;


修改字段类型

ALTER TABLE 表名 MODIFY COLUMN 字段名 新数据类型;

---


查询数据

查询数据表的表结构（字段）

SHOW COLUMNS FROM 表名

查询数据表的数据

SELECT * FROM 表名;


根据条件查询
SELECT * FROM 表名1,表名2;

SELECT * FROM 表名1.id aid,表名2.id bid;  // 一次查询多个表，结果是一个二维表，因为表名1和表名2，都是查询id，避免区分不了，使用查询，设置列的别名，便于区分

SELECT * FROM 表名 WHERE id >= 1;

SELECT * FROM 表名 WHERE id IN (1,3,5); 

SELECT * FROM 表名 WHERE id BETWEEN 2 AND 7;

SELECT * FROM 表名 WHERE id >= 1 AND  name = "xxx";

SELECT * FROM 表名 WHERE id >= 1 OR name = "xxx";

SELECT * FROM 表名 WHERE id >= 1 NOT  name = "xxx";

SELECT id name FROM 表名 WHERE name LIKE"a%";

SELECT 字段 FROM 表名 GROUP BY 字段;  // 附属功能和DISTINCT一样，有去重功能

SELECT DISTINCT 字段 FROM 表名;  // 列出不同的值，功能和查询数据，并且数据不重复

根据要求返回数据

SELECT id FROM 表名;


通过SELECT语句修改列名

SELECT id abc FROM 表名;


通过SELECT语句接WHRE条件

SELECT id FROM 表名 WHERE name = "yes";

转义和匹配字符串

百分号%   匹配任意长度的字符串，包括空字符串

下划线_   匹配单个字符串，可以使用多个下划线来指定匹配多长的字符串

转义符\   用于输出或者匹配特殊字符，例如\%和\_


排列

SELECT name FROM 表名 ORDER BY id;  // 按照id从低到高

SELECT name FROM 表名 ORDER BY id DESC;  // 使用DESC倒序


SELECT name FROM 表名 ORDER BY id DESC,gender; // 先按照id倒序，如果有相同的值再按gender排序

ORDER BY  排序（升序（asc）降序（desc））

SELECT 字段 FROM 表名 ORDER BY 字段 （排序方式）;


默认的排序是ASC，可以忽略，如果有WHRE语句，那么ORDER BY要放在WHRE后面


SELECT 字段1 , 字段2 , 字段3 FROM 表名;  // 返回指定字段下的数据，而不是整个表的数据

SELECT 字段1 , 字段2  别名1, 字段3 FROM 表名;  // 使用SELECT查询，并将列名重命名，把字段2改为别名1

SELECT * FROM 表名 ORDER BY  DESC LIMIT 5 （OFFSET 1）  // 分页返回，每页5条记录，获取第2条的记录（sql的索引从0开始）

SELECT COUNT(*) FROM 表名;   // 查询该表有多少条记录

SELECT COUNT(*) 别名 FROM 表名 WHERE 字段 = 'xxx';   // 查询该表有多少条记录符合要求


SELECT 表名1.字段1 别名1, 表名1.字段2,表名2.字段1 别名2,表名2.字段2 FROM 表名1, 表名2;  // 查询多个表的数据

SELECT 别名1.id sid,别名1.name,别名2.id cid, 别名2.name names FROM 表名1 别名1,表名2 别名2;  // 效果和上面一样，只是简洁一点

SELECT 别名1.id sid,别名1.name,别名2.id cid, 别名2.name names FROM 表名1 别名1,表名2 别名2 WHERE 别名1.id<10 AND name ="hallo";   // 支持WHERE条件



---

##SQL约束

PRIMARY KEY    约束唯一标识数据库表中的每条记录，有助于快速查找表中的一个特定的记录。 主键必须包含唯一的值，主键列不能包含 NULL 值，每个表只能有一个主键

例如

CREATE TABLE abc(id int NOT NULL,PRIMARY KEY(id));

NOT NULL   约束某列不能存储NULL值，约束字段要包含值，如果不为字段添加值，则无法更新或者插入新的值

例如：

CREATE TABLE abc(id int NOT NULL);


UNIQUE       约束唯一标识数据库表中的每条记录

例如：
CREATE TABLE abc(id int NOT NULL,UNIQUE(id));



FOREIGN KEY    约束一个表的值指向另一个表的唯一标识，防止非法数据插入，必须是其指向的表中的值

例如：
CREATE TABLE abc(id int NOT NULL,home int,PRIMARY KEY (Id),FOREIGN KEY (home) REFERENCES Persons(home));

ALTER TABLE 子表名 ADD CONSTRAINT 约束名 FOREIGN KEY(子表名字段) REFERENCES 父表名(字段);

DEFAULT      约束向列中插入默认值，确保列中始终有值，而不是NULL值

例如：
CREATE TABLE abc(id int DEFAULT '0');



CHECK    约束限制列中的值的范围，当向单个列约束时，那么该列只允许特定的值，当向一个表约束时，那么该约束只会在特定的列中对值进行限制


例如：

CREATE TABLE abc(id int NOT NULL,CHECK(id >0));


---

添加表级约束
例如：

ALTER TABLE 表名 add  UNIQUE(home);

添加表，列级约束

ALTER TABLE 表名 modify id int NOT NULL



移除外建约束

ALTER TABLE 表名 DROP FOREIGN KEY 约束名;

---

mysql 函数

使用方法，例如：

SELECT 函数(字段) FROM 数据表;



COUNT() 计数

SUM()  求和

AVG()  平均

MAX()  最大

MIN()  最小

COUNT()  返回表的记录数


创建用户，删除用户，修改用户

---

创建用户


GRANT ALL PRIVILEGES ON *.* TO 'a1'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION; 

GRANT ALL PRIVILEGES ON *.* TO 'a1'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION; 

INSERT INTO mysql.user(Host,User,Password,ssl_cipher,x509_issuer,x509_subject,authentication_string) VALUES("%","a3",password("123456"),'y','y','y','y');


---

删除用户

DROP USER 'a1'@'%';

DELETE FROM user WHERE user='a2' AND host='%';

---

修改用户密码

管理员修改

UPDATE mysql.user SET password=password('123') WHERE user='a3' AND host='%';

SET password FOR 'a1'@'%'=password("123");


root修改

GRANT ALL ON *.* TO 'a3'@'%' IDENTIFIED BY '123';

UPDATE mysql.user SET authentication_string=password('123') WHERE user ='a1';

普通用户修改（普通用户只能修改自己的密码）

SET password=password('123');


---

连接

当想要输出一段数据，但是里面的数据来自不同的表


内连接

SELECT 别名1.id,别名1.name,别名1.id_name别名2.score FROM 表名1 别名1 INNER JOIN 表名2 别名2 ON  别名1.id_name = 别名2.id;


外连接

SELECT 别名1.id,别名1.name,别名1.id_name别名2.score FROM 表名1 别名1 RIGHT OUTER JOIN 表名2 别名2 ON  别名1.id_name = 别名2.id;

内连接和外连接的区别是内连接只返回两个表同时存在的数据，外连接（RIGHT OUTER JOIN）只返回右表都存在的行，外连接（LEFT OUTER JOIN）只返回左表都存在的行


INNER JOIN是选出两张表都存在的记录，LEFT OUTER JOIN是选出左表存在的记录，RIGHT OUTER JOIN是选出右表存在的记录，FULL OUTER JOIN则是选出左右表都存在的记录




视图


CREATE VIEW 视图名(字段1,字段2,字段3) AS SELECT 字段1,字段2,字段3 FROM 表名;

SELECT * FROM 视图名; // 输出视图的数据

可以理解为将表的指定字段来插入到一个“虚拟的表”，视图的字段名和表的字段名可以不一样，用户就查询不到原来的表的结构和接触不了实质上的表

查询视图

DESCRIBE 视图名；


在多表上创建视图

CREATE VIEW 视图名  AS SELECT 字段1,字段2,字段3 FROM 表名1 别名1,表名2 别名2 WHERE 别名1.字段1 = 别名2.字段1 AND 别名2.字段2 = 别名1.字段2 ;



---


查看当前mysql版本

SELECT VERSION();

查看当前数据库的用户

SELECT USER();

查看数据库的存储路径

SELECT @@DATADIR;

查看mysql的安装路径

SELECT @@BASEDIR

查看mysql的安装操作系统

SELECT @@VERSION_COMPILE_OS

初始数据库默认有4个，其中information_schema是保存着mysql服务器所维护的其他数据库的信息，例如数据库表，数据库名，表的数据类型，以及数据库的权限等等

其中information_schema有73个表，其中最重要的有3个，分别是SCHEMATA和TABLES，以及COLUMNS

SCHEMATA表存储了当前mysql中存储的数据库的信息，指令SHOW DATABASES;的结果来自于该表

TABLES表存储了数据库中的表的信息（视图的信息也存储在该表）

COLUMNS表存储了表的列信息

通过information_schema表来查询

查询有什么数据库

SELECT * FROM information_schema.SCHEMATA;

查询hallo数据库有什么数据表

SELECT * FROM information_schema.TABLES WHERE TABLE_SCHEMA="hallo";

查询hallo数据表有哪些列

SELECT * FROM information_schema.COLUMNS WHERE TABLE_NAME="hallo";

mysql数据库是mysql的核心，其中存储了数据库的用户，权限设置，关键字等等所需要使用和管理的数据

performance_schema数据库存储了mysql服务器运行期间的状态信息，例如执行了那些语句，内存使用了多少等等

sys数据库主要是通过视图的形式将数据库的数据汇总，可用来性能调优和诊断


UNION操作符可以将多个（2个以上）SELECT语句合并在一起，如果出现重复的数据会被忽略（唯一性）

例如：

SELECT id FROM test
UNION
SELECT id FROM test1
ORDER BY id;

如果需要输出重复的数据，可以使用UNION ALL