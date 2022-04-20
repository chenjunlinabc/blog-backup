---
title: "简单使用Nginx"
categories: [ "默认" ]
tags: [ "nginx" ]
draft: false
slug: "85"
date: "2021-08-18 12:50:00"
---

Nginx是目前web服务器占比第一（在https://w3techs.com 中可以看到Nginx占比33.1%）


安装

编译安装

apt install -y gcc gcc-c++ pcre pcre-devel openssl openssl-devel zlib zlib-devel

wget http://nginx.org/download/nginx-1.18.0.tar.gz

tar -zxvf nginx-1.18.0.tar.gz

cd nginx-1.18.0

make &&make install

一键安装（不推荐）

apt install nginx



检查是否安装完毕

nginx -v


nginx配置文件nginx.conf解读，一般在/etc/nginx下

    user root; // nginx运行用户
    worker_processes auto; // nginx进程数，一般会设置和CPU核数一致
    error_log  /www/wwwlogs/nginx_error.log  crit; // 错误日志存储位置
    pid        /www/server/nginx/logs/nginx.pid; // 进程PID存储位置
    events{
            worker_connections 51200; // 单个后台进程的最大并发数
            multi_accept on; // 一个进程可以同时接受所有的新连接，关闭（off）的话一个进程只能接收一个连接，默认值为off关闭
        }
    http{
            include       mime.types; // 文件扩展和类型的映射表
            default_type  application/octet-stream; // 默认文件的类型
            server_names_hash_bucket_size 512;
            client_header_buffer_size 32k;
            large_client_header_buffers 4 32k;
            client_max_body_size 50m;
            gzip on; // 开启gzip压缩
            gzip_min_length  1k; // 设置允许压缩的最小的字节数，这里设置1k，就是只有超过1k的文件才会被压缩
            gzip_buffers     4 16k; // 以16k为单位，以16k的4倍申请存储gzip压缩的数据流内存
            gzip_http_version 1.1; // 只有当http1.1协议版本可以执行gzip压缩
            gzip_comp_level 2; // 压缩比
            gzip_types     text/plain application/javascript application/x-javascript text/javascript text/css application/xml; // 设置匹配MIME类型进行压缩，默认text/html是始终被压缩的
            gzip_vary on;
            gzip_proxied   expired no-cache no-store private auth; // 当nginx是反向代理时数据的压缩，expired表示启动压缩，如果header头中包含 "Expires" 头信息，，只有off是关闭数据压缩，any是无条件启动压缩
            server_tokens off;  // 隐藏版本号，避免404暴露nginx的版本
            access_log off; // nginx访问日志存储位置（off为关闭访问日志）
        
    server
        {
            listen 80; // 监听端口
            server_name localhost; // 配置域名
            index index.html index.htm index.php;  // 默认页面
            root  /www/server/phpmyadmin; // 服务默认启动目录
            error_page   404   /404.html; // 404配置页面
            error_page   500 502 503 504  /50x.html // 50x服务错误配置页面
            include fastcgi.conf; // 使用fastCGI解析php

            location /
            {
                root /www/wwwroot; 
                index index.html index.htm index.php;
            }

            access_log  /www/wwwlogs/access.log; // nginx访问日志存储位置（off为关闭访问日志）
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
        allow 192.168.1.128; // 限制只有192.168.1.128可访问
        deny all // 禁止访问
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