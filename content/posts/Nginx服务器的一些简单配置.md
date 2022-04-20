---
title: "Nginx服务器的一些简单配置"
categories: [ "默认" ]
tags: [ "server" ]
draft: false
slug: "34"
date: "2021-06-16 15:52:00"
---

配置ssl证书

前提是已经申请到ssl证书,并且开放443端口

编辑nginx.conf

将443那几行的注释去掉（#）,并且修改

ssl_certificate "crt证书的绝对路径";
ssl_certificate_key"key证书的绝对路径";


如果想访问网站就301重定向到https，那么添加这几行

    server{
        listen 80;
        server_name cjlio.com;
        rewrite ^(.*) https://$host$1 permanent;
    }

\# 当使用80端口访问网站时，将301永久重定向到https://cjlio.com，达到全站https的效果

然后刷新一下配置nginx -s



---

nginx反向代理配置

将客户端请求转发给内部网络的其他目标服务端，并且将从其他服务端的结果返回到客户端，代理服务端和目标服务端，在外部看起来像是一个整体，只是将请求转发给其他服务端处理，从而达到减轻目标服务端的压力的效果

配置nginx.conf

    location / {
        proxy_pass https://test.cjlio.com;   # 反向代理服务器地址
        proxy_connect_timeout 200;  # 设置连接超时
        proxy_read_timeout 200;  #  设置读响应超时
    }

重启Nginx服务：service nginx restart


请求当前服务器时，当前服务器将请求转发给地址为 https://test.cjlio.com 的服务器处理


---

gzip压缩

    gzip on;
    gzip_comp_level 1; 
    gzip_min_length 10;
    gzip_http_version 1.1;
    gzip_types text/html text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;


解释：

gzip on // gzip开启，关闭是off

gzip_buffers 16 8k // 设置处理请求压缩的缓冲区数量和大小，一般不会设置，默认

gzip_comp_level 1 // 设置压缩比，数值1至9，压缩文件越小越吃CPU，最性价比的是1

gzip_min_length 10 // 当文件超过这个值才启用压缩，单位k，值为0时表示全站压缩

gzip_http_version 1.1 // 这个1.1是指http版本，只有http/1.1以上才启用gzip，例如http/1.0就不启用

gzip_types text/html // 指定要压缩的MIME类型，请求不在这个指定内范围的文件不进行压缩