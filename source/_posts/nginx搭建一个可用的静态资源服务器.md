---
title: nginx搭建一个可用的静态资源服务器
date: 2018-11-22 10:52:24
categories: nginx
---


    nginx配置文件nginx.conf


    http {
        include       mime.types;
        default_type  application/octet-stream;

        #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
        #                  '$status $body_bytes_sent "$http_referer" '
        #                  '"$http_user_agent" "$http_x_forwarded_for"';

        #access_log  logs/access.log  main;

        sendfile        on;
        tcp_nopush     on;

        #keepalive_timeout  0;
        keepalive_timeout  120;

        #gzip  on;
        #gzip_min_length 1;

        server {
            listen       port;   --- 端口号
            server_name  localhost;

            #charset koi8-r;

            #access_log  logs/host.access.log  main;

            location / {
                alias   www_home/;
                index  index.html index.htm;
            }

        }

配置好以后，nginx可以访问时

#### Nginx开启Gzip压缩大幅提高页面加载速度

    gzip  on;     // gzip开关开启
    gzip_min_length 1;  // 小于一字节就不再压缩
    gzip_comp_level 2;  // 压缩级别
    gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/peg image/gif image/png;


#### Nginx打开目录浏览功能--autoindex   

Nginx默认是不允许列出整个目录的。如需此功能，打开nginx.conf文件或你要启用目录浏览虚拟主机的配置文件，在server或location 段里添加上autoindex on;来启用目录流量

    location / {
        alias   www_home/;
        autoindex on;
        index  index.html index.htm;
    }

#### 限制访问速度--limit_rate    

    location / {
        alias   www_home/;
        autoindex on;
        set $limit_rate 1k;   // 每秒1k，设置这个以后就会访问很慢
        index  index.html index.htm;
    }

    