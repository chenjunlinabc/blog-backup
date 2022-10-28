---
title: "简单使用Nginx"
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
        server_name abc.cjlio.com;
        index index.html index.htm index.php; 
    }
    server{
        listen 80; # 监听端口
        listen 443 ssl http2; # 设置https ssl
        server_name cjlio.com; # 配置域名
        index  index.php index.html index.htm default.php default.htm default.html;
    }

在上面例子中，可以看到端口都是80，但是域名不同，也就是说这里设置了3个虚拟主机，虚拟主机配置除了设置在/nginx/nginx.conf中，还可以设置在子配置文件（nginx/conf.d/default.conf）中，不同的虚拟主机都可设置在不同的子配置文件中




反向代理

    server{
        listen 80;
        server_name abc.cjlio.com;
        location / {
               proxy_pass http://cjlio.com;
               index  index.html;
        }
    }

在该配置中，访问abc.cjlio.com会反向代理到http://cjlio.com中







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


---


Goaccess（可视化监控access日志）


---



SSL证书


申请人通过登记机构（具备验证申请人的身份）发送证书签名申请（CSR）到CA机构，CA机构再将公钥私钥发送给登记机构，登记机构再给申请人


当访问服务端时，会发送申请证书的请求，服务端接收到请求时会将公钥发送给客户端，客户端会验证证书的有效性，CA会将过期证书发送给CRL服务器（浏览器验证时发送请求给CRL服务器）（性能很差），一般通过OCSP（在线证书状态协议）验证，OCSP是服务端主动获取的结果，然后随着握手协商时发送给客户端，不需要客户端去验证


开启OCSP Stapling


    server {
        listen 443 ssl;
        server_name test.cjlio.com;
        ssl_stapling on;
        ssl_stapling_verify on;
        resolver 8.8.8.8 8.8.4.4 valid=300s; #OCSP请求的DNS服务器
        resolver_timeout 5s;
        ssl_trusted_certificate nginx.pem; #证书公钥（pem格式）
    }

查看是否开启OCSP Stapling

openssl.exe s_client -connect https://test.cjlio.com:443 -status

没有开启会返回OCSP response: no response sent









