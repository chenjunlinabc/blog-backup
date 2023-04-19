---
title: "简单使用Nginx HTTP服务器软件"
categories: [ "技术" ]
tags: [ "nginx" ]
draft: false
slug: "85"
date: "2021-08-18 12:50:00"
---

Nginx是目前web服务器占比第一（在https://w3techs.com 中可以看到Nginx占比33.1%）


Nginx支持静态资源提供服务，支持高并发，热部署，反向代理，缓存，负载均衡等功能，Nginx使用BSD许可证开源（允许修改Nginx源码来重新发布一个商业用途的（例如Tengine））

Nginx由Nginx二进制可执行文件，Nginx.conf，access.log，error.log组成


Nginx和Nginx plus的区别：Nginx开源，免费，Nginx plus闭源，不免费


Nginx编译安装

Mainline version版本是提供最新的功能，Stable version版本是目前的稳定版本，Legacy versions版本是过去的版本

下载Nginx

wget http://nginx.org/download/nginx-1.22.0.tar.gz


解压Nginx压缩包

tar -xzf nginx-1.22.0.tar.gz

其中auto目录有4个子目录（分别为cc（用于编译），lib（lib库），os（操作系统的判断），types（类型判断）），conf目录是Nginx配置目录，html目录是Nginx默认静态文件

conf目录是Nginx配置文件目录（例如nginx.conf），src目录是Nginx源代码目录

进入nginx目录然后进行编译

查看编译时支持的参数

./configure --help | more

使用默认参数编译

./configure --prefix=/home/nginx


编译完成的中间文件存会放在objs文件夹下

ngx_module.c是要编译进Nginx的模块，如果需要安装第三方模块需要在其修改


make编译

make install安装


进入/home/nginx，可以看到已经安装完成


---



安装

编译安装

apt install -y gcc gcc-c++ pcre pcre-devel openssl openssl-devel zlib zlib-devel

wget http://nginx.org/download/nginx-1.22.0.tar.gz

tar -zxvf nginx-1.18.0.tar.gz

cd nginx-1.18.0

make &&make install

一键安装（不推荐）

apt install nginx



检查是否安装完毕

nginx -v


nginx配置文件nginx.conf解读，一般在/etc/nginx下

    user root; # nginx运行用户
    worker_processes auto; # nginx进程数，一般会设置和CPU核数一致
    error_log  /www/wwwlogs/nginx_error.log  crit; # 错误日志存储位置
    pid        /www/server/nginx/logs/nginx.pid; # 进程PID存储位置
    events{
            worker_connections 51200; # 单个后台进程的最大并发数
            multi_accept on; # 一个进程可以同时接受所有的新连接，关闭（off）的话一个进程只能接收一个连接，默认值为off关闭
        }
    http{
            include       mime.types; # 文件扩展和类型的映射表
            default_type  application/octet-stream; # 默认文件的类型
            server_names_hash_bucket_size 512;
            client_header_buffer_size 32k;
            large_client_header_buffers 4 32k;
            client_max_body_size 50m;
            gzip on; # 开启gzip压缩
            gzip_min_length  1k; # 设置允许压缩的最小的字节数，这里设置1k，就是只有超过1k的文件才会被压缩
            gzip_buffers     4 16k; # 以16k为单位，以16k的4倍申请存储gzip压缩的数据流内存
            gzip_http_version 1.1; # 只有当http1.1协议版本可以执行gzip压缩
            gzip_comp_level 2; # 压缩比
            gzip_types     text/plain application/javascript application/x-javascript text/javascript text/css application/xml; # 设置匹配MIME类型进行压缩，默认text/html是始终被压缩的
            gzip_vary on;
            gzip_proxied   expired no-cache no-store private auth; # 当nginx是反向代理时数据的压缩，expired表示启动压缩，如果header头中包含 "Expires" 头信息，，只有off是关闭数据压缩，any是无条件启动压缩
            server_tokens off;  # 隐藏版本号，避免404暴露nginx的版本
            access_log off; # nginx访问日志存储位置（off为关闭访问日志）
        
    server
        {
            listen 80; # 监听端口
            server_name localhost; # 配置域名
            index index.html index.htm index.php;  # 默认页面
            root  /www/server/phpmyadmin; # 服务默认启动目录
            error_page   404   /404.html; # 404配置页面
            error_page   500 502 503 504  /50x.html # 50x服务错误配置页面
            include fastcgi.conf; # 使用fastCGI解析php

            location /
            {
                root /www/wwwroot; 
                index index.html index.htm index.php;
            }

            access_log  /www/wwwlogs/access.log; # nginx访问日志存储位置（off为关闭访问日志）
        }


可以看到网站根目录在/www/wwwroot


强制停止nginx服务 nginx -s stop

非强制停止（等待进程完成任务后停止） nginx -s quit

重载配置文件 nginx -s reload


限制访问权限

    location /
    {
        root /www/wwwroot; 
        index index.html index.htm index.php;
        allow 192.168.1.128; # 限制只有192.168.1.128可访问
        deny all # 禁止访问
    }


nginx虚拟主机（可基于端口，域名（ip），别名设置虚拟主机）

    server{
        listen 80; 
        server_name localhost;
        index index.html index.htm index.php; 
    }
    server{
        listen 80;
        server_name abc.xiaochenabc123.test.com;
        index index.html index.htm index.php; 
    }
    server{
        listen 80; # 监听端口
        listen 443 ssl http2; # 设置https ssl
        server_name xiaochenabc123.test.com; # 配置域名
        index  index.php index.html index.htm default.php default.htm default.html;
    }

在上面例子中，可以看到端口都是80，但是域名不同，也就是说这里设置了3个虚拟主机，虚拟主机配置除了设置在/nginx/nginx.conf中，还可以设置在子配置文件（nginx/conf.d/default.conf）中，不同的虚拟主机都可设置在不同的子配置文件中




反向代理

    server{
        listen 80;
        server_name abc.xiaochenabc123.test.com;
        location / {
               proxy_pass http://xiaochenabc123.test.com;
               index  index.html;
        }
    }

在该配置中，访问abc.xiaochenabc123.test.com会反向代理到http://xiaochenabc123.test.com中







---







Nginx命令（例如nginx -s quit）

查看帮助

nginx -?或者nginx -h

使用指定配置文件

nginx -c nginx-01.conf

指定配置指令（覆盖nginx.conf内的指令）

nginx -g "user root;"

立即关闭nginx服务

nginx -s stop

处理完毕全部请求后再停止服务

nginx -s quit

重载配置文件

nginx -s reload

重新记录日志

nginx -s reopen

查看配置文件是否出错（大小写都可以）

nginx -t

查看nginx信息（大V是编译信息小写v是版本信息）

nginx -v

指定nginx运行目录

nginx -p /nginx/


---



开启缓存

proxy_cache_path /data/cache levels=1:2 keys_zone=oncache:10m max_size=10g inactive=60m;


/data/cache是缓存目录，levels是缓存目录的优先级，例如1:2（二级目录），表示/data是一级目录，/cache是二级目录，keys_zone是定义共享内存区的名称和大小（oncache是名称，10m是内存区的大小），max_size是缓存的最大大小，inactive表示该缓存不在该指定时间内被访问，将从缓存中删除

proxy_cache_path的上下文是http段

---


Goaccess（可视化监控access日志）


---



SSL证书


申请人通过登记机构（具备验证申请人的身份）发送证书签名申请（CSR）到CA机构，CA机构再将公钥私钥发送给登记机构，登记机构再给申请人


当访问服务端时，会发送申请证书的请求，服务端接收到请求时会将公钥发送给客户端，客户端会验证证书的有效性，CA会将过期证书发送给CRL服务器（浏览器验证时发送请求给CRL服务器）（性能很差），一般通过OCSP（在线证书状态协议）验证，OCSP是服务端主动获取的结果，然后随着握手协商时发送给客户端，不需要客户端去验证


开启OCSP Stapling


    server {
        listen 443 ssl;
        server_name test.xiaochenabc123.test.com;
        ssl_stapling on;
        ssl_stapling_verify on;
        resolver 8.8.8.8 8.8.4.4 valid=300s; #OCSP请求的DNS服务器
        resolver_timeout 5s;
        ssl_trusted_certificate nginx.pem; #证书公钥（pem格式）
    }

查看是否开启OCSP Stapling

openssl.exe s_client -connect https://test.xiaochenabc123.test.com:443 -status

没有开启会返回OCSP response: no response sent




---






nginx负载均衡（将请求转发给多个服务端去执行，每个服务端的任务一般是一样的）

nginx配置

    upstream test_server{
        server 198.168.1.2:81 weight=10 max_conns=3000 fail_timeout=10s max_fails=3; #权重为10（默认为1，指定该上游服务端的最大并发连接数为1000，在10秒内判断2次上游服务端不可用时，将在这10秒内不进行请求转发
        server 198.168.1.3:82 weight=1 max_conns=500 fail_timeout=10s max_fails=2;
        server 198.168.1.4:83 weight=5 max_conns=1000 fail_timeout=10s max_fails=2 backup; # backup，备用服务端标记，只有当其他服务端不可用时才进行请求转发，down，标记是离线服务端，不会进行请求转发
        server 198.168.1.5:80 weight=5 max_conns=1000 fail_timeout=10s max_fails=2;
        server 198.168.1.6:8080 weight=10 max_conns=2000 fail_timeout=10s max_fails=2;
        server 198.168.1.7:85 weight=5 max_conns=1000 fail_timeout=10s max_fails=2 down;
        server test1.xiaochenabc123.test.com:5000 weight=20 max_conns=5000 fail_timeout=10s max_fails=5;
        keepalive 32; # worker子进程与上游服务端空闲的长连接数为32
        keepalive _requests 50; # 每个长连接最多只能处理50个HTTP请求（默认值为100）
        keepalive _timeout 30s; #nginx与上游服务端空闲长连接的最长生存时间，默认为60秒
    }
    server{
        listen 80;
        server_name test.xiaochenabc123.test.com
        location /test/ {
            proxy_pass http://test_server;
        }
    }


测试

curl test.xiaochenabc123.test.com



负载均衡算法


哈希算法（根据某些参数来决定，只有参数不变时将一直使用该代理服务）

    upstream test_server1{
        hash $request_uri; # 当uri不发送改变，那么负责处理该请求的上游服务端将会一直不变，也可以是其他参数
        server 198.168.1.2:80;
        server 198.168.1.3:80;
    }


ip_hash算法


    upstream test_server1{
        ip_hash; # 根据ip来决定，如果ip不发生改变，将一直使用该代理服务
        server 198.168.1.2:80;
        server 198.168.1.3:80;
    }


最少连接数算法


    upstream test_server1{
        zone testserver 10M; # 创建共享内存，通过共享内存来让全部的worker子进程对负载均衡策略生效，上游服务的状态数据都存放在该功能内存中
        least_conn; # 选择连接数最少的服务端，并且对该服务端进行加权，让它能够获取更多连接
        server 198.168.1.2:80;
        server 198.168.1.3:80;
    }



---



性能优化

worker子进程数，worker_processes 8;

worker子进程与CPU核心绑定，如果是4个CPU核心，worker_cpu_affinity 0001 0010 0100 1000;

worker子进程与CPU核心绑定的好处：更好的利用CPU核心内部的一级缓存，二级缓存（增加缓存命中率）

位数表示使用多少个核心，个数为多少个进程，如果是0101 1010，将表示4个核心，使用2个进程

设置worker子进程的优先级，worker_priority -20;

提高worker子进程的优先级，可以分配更大的CPU时间片，让worker子进程更优先的进行工作

优先级为-20到19，值越小优先级越高

延迟处理新连接的请求，listen 80 deferred;


deferred表示在3次握手中，检测到客户端进行http请求数据时，tcp状态才设置连接成功（tcp连接成功会唤醒nginx进程，如果没有请求数据，只进行连接TCP，完全没有必要nginx进程），否则丢弃





---




优化TCP连接

当nginx作为中间件时，向上游服务获取数据时，需要进行TCP连接，对下游客户端也是需要进行TCP连接

以下配置都是要用到/etc/sysctl.conf的系统控制文件，这个文件是配置内核参数等信息的


net.ipv4.tcp_syn_retries = 6，这个设置决定了上游（上游一直没有返回SYN+ACK包）在TCP连接中超时，最多重发多少次SYN包，在Linux下默认为6，并且TCP连接超时为127秒，这个值越小，认定TCP连接超时的时间越短，认定TCP连接超时会关闭连接（占用TCP连接会消耗CPU和内存）

net.ipv4.tcp_synack_retries = 5，这个设置决定了SYN+ACK确认包的重发次数，默认为5，这个值越小，认定TCP连接超时的时间越短

net.ipv4.tcp_syncookies = 1，1为开启tcp_syncookies，在进行TCP第一次握手时，如果syn_backlog队列满了，会丢弃SYN包，开启tcp_syncookies将不会丢弃，而是使用syncookie进行握手

net.core.netdev_max_backlog = 262144，决定能接收数据包的最大长度队列

net.core.somaxconn = 128，决定同时发起的TCP连接数，默认为128，可以调高该值，避免连接超时或者重传

net.ipv4.tcp_max_orphans = 16384，决定孤立连接（主动断开方关闭进程，表示进程与这个连接没有关系了，就成了孤立连接，也就是FIN_WAIT1状态）的最大数量，当孤立连接超过该值，后续新增的孤立连接将不会走4次挥手流程，而是直接通过发送RST报文来强制关闭连接

net.ipv4.tcp_max_syn_backlog = 128，决定处于SYN_RECV状态的TCP连接数（第二次握手，服务端发送SYN确认包后），超过设置数会丢弃第三次的SYN报文

net.ipv4.tcp_abort_on_overflow = 1，决定了TCP连接建立完成后，如果backlog队列满了，TCP连接将回退到SYN+ACK状态，开启该设置，会发送RST包给客户端，进行终止该词连接，如果没有必要请勿开启

net.ipv4.ip_local_port_range = 32768 61000，限制服务端对外和本地的端口范围（每个TCP连接都需要占用一个本地的端口，如果本地端口满了，将无法创建新的TCP连接）

net.ipv4.tcp_tw_reuse = 1，复用TIME_WAIT状态的连接，开启这个可以快速复用TIME_WAIT状态的连接

net.ipv4.tcp_fastopen = 1，TFO功能，开启该功能会在三次握手时，发送syn包给上游，上游根据ip生成cookie，并且和syn+ack一起发送给下游（下游缓存这个cookie），下游与上游进行第二连接时，发送syn包给上游会携带这个cookie，上游验证后发送SYN-ACK包，就可以发送数据（数据在SYN包中），省去第三次握手，下游发送ACK确认上游的数据，如果cookie无效，将会丢弃SYN包中的数据，发送正常的SYN-ACK包，并且完成第三次握手，进行TFO功能的TCP连接依赖于这个cookie

/etc/sysctl.conf配置生效可以使用sysctl -p命令