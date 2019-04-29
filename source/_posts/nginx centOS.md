---
title: Nginx 安装使用
date: 2018-04-20 13:11:12
categories: Linux
---

## nginx基本操作

安装：

	yum install nginx

启动：

	service nginx start

停止： 
		
	service nginx stop

重载： 
		
	service nginx reload



### 部署NGINX：

	// $ sudo service httpd stop
	$ sudo yum install nginx

查看是否启动：
	
	$ ps -ef | grep nginx
启动：

	$ sudo service nginx start
	$ sudo service nginx reload



	firewall-cmd --zone=public --add-port=80/tcp --permanent
	防火墙开启80端口
	firewall-cmd --reload
	防火墙重载
	yum install iptables-services
	systemctl enable iptables
	systemctl stop firewalld
	systemctl start firewalld

配置/etc/nginx/下的配置文件：

因为在nginx主配置文件中引入了conf.d文件夹中的:
 include /etc/nginx/conf.d/*.conf;
所以在nginx/conf.d/下面添加******.conf文件进行配置.

主配置文件如下：

``` nginx

		# For more information on configuration, see:
		#   * Official English Documentation: http://nginx.org/en/docs/
		#   * Official Russian Documentation: http://nginx.org/ru/docs/

		user root;
		worker_processes auto;
		error_log /var/log/nginx/error.log;
		pid /run/nginx.pid;

		# Load dynamic modules. See /usr/share/nginx/README.dynamic.
		include /usr/share/nginx/modules/*.conf;

		events {
		    worker_connections 1024;
		}

		http {
		    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
	                      '$status $body_bytes_sent "$http_referer" '
	                      '"$http_user_agent" "$http_x_forwarded_for"';

		    access_log  /var/log/nginx/access.log  main;

		    sendfile            on;
		    tcp_nopush          on;
		    tcp_nodelay         on;
		    keepalive_timeout   65;
		    types_hash_max_size 2048;

		    include             /etc/nginx/mime.types;
		    default_type        application/octet-stream;

		    # Load modular configuration files from the /etc/nginx/conf.d directory.
		    # See http://nginx.org/en/docs/ngx_core_module.html#include
		    # for more information.
		    include /etc/nginx/conf.d/*.conf;

		    server {
		        listen       80 default_server;
		        listen       [::]:80 default_server;
		        server_name  _;
		        root         /usr/share/nginx/html;

	          # Load configuration files for the default server block.
	          include /etc/nginx/default.d/*.conf;

	          location / {
 	         }

	          error_page 404 /404.html;
	              location = /40x.html {
	          }

	          error_page 500 502 503 504 /50x.html;
	              location = /50x.html {
	   	       }
	 	     }
	 	 }

```


1. 首先主配置文件，修改用户组

	user root;

2. 然后进行操作：

	cd conf.d
	touch ******.conf  //新建文件
	vim ******.conf  

修改******.conf:

	server {
	        listen       80;
       	 server_name  47.**.***.**;

       	 #charset koi8-r;

       	 #access_log  logs/host.access.log  main;

       	 location / {
       	     root   /home/********/****;
       	     index  index.html index.htm;
		    try_files $uri /index.html;
       	 }
	
       	 #error_page  404              /404.html;
	
       	 # redirect server error pages to the static page /50x.html
       	 #
       	 error_page   500 502 503 504  /50x.html;
       	 location = /50x.html {
       	     root   html;
       	 }
	
    	}	

### react-router在nginx上的应用，参照：

[调整nginx正确服务react-router应用](https://blog.csdn.net/qq_26222859/article/details/54287068)


### 注意在linux进行解压文件时

	unrar e build.rar build

这样会把build.rar里面所有的文件都解压到build文件夹，不保持原有的目录。

	unrar x build.rar build

这样解压会保持原有的目录。
