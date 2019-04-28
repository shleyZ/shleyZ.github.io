---
title: 阿里云服务器nginx安装
date: 2018-04-20 13:11:12
categories: Linux
---

## 阿里云服务器Linux远程ssh连接

	
可以在创建 ECS 实例时分配密钥对，也可以为实例绑定密钥对。

一台 ECS 实例只能绑定一个 SSH 密钥对。

### 创建密钥对

登录 ECS 控制台。

在左侧导航栏中，单击 密钥对。

在 密钥对 页面上，选择所需地域后，单击 创建密钥对。

在 创建密钥对 页面，设置密钥对名称，并选择 自动新建密钥对。

单击 确定 创建密钥对。

### 绑定密钥对

登录 ECS 控制台。

在左侧导航栏中，选择 网络与安全 > 密钥对。
	
选择一个地域。
	
找到需要操作的密钥对，在 操作 列中，单击 绑定密钥对。

在 绑定密钥对 对话框里，在 选择 ECS 实例 栏中，选中需要绑定该密钥对的 ECS 实例名称，单击 >，移入 已选择 栏中。

单击 确定 绑定密钥对。

### 使用 SSH 密钥对连接 Linux 实例（本地为 Windows 环境）

#####前提条件

已经下载并安装了 PuTTY 和 PuTTYgen。

PuTTY 和 PuTTYgen 的下载地址：

PuTTY：

https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe

PuTTYgen：

https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe

已经拥有一个分配了密钥对的 Linux 实例。可以在创建 ECS 实例时分配密钥对，也可以为实例绑定密钥对。

实例所在的安全组必须添加以下安全组规则（具体操作，请参考 https://help.aliyun.com/document_detail/51798.html?spm=a2c4g.11186623.4.2.4RBql5#windows）



#####操作步骤

1. （可选）如果您正在使用阿里云生成的 .pem 私钥文件，应先按以下步骤转为 .ppk 私钥文件。如果您使用的私钥文件本身已经是 .ppk 文件，可以略过这一步。
 
	启动 PuTTYgen。本示例中的 PuTTYgen 版本为 0.68。

	在 Parameters > Type of key to generate 中，选中 RSA。Number of bits in a generated key 的值不需要设置，软件会根据导入的私钥信息自动更新。

	单击 Load。PuTTYgen 默认仅显示扩展名为 .ppk 的文件。要找到您的 .pem 文件，请选择显示所有类型的文件。

	选择您从阿里云下载的“.pem”格式的私钥文件，然后单击 打开。

	单击 OK（确定）关闭确认对话框。

	单击 Save private key。PuTTYgen 会显示一条关于在没有口令的情况下保存密钥的警告，单击 是(Y)。

	指定与密钥对相同的私钥名称，保存。PuTTY 会自动为文件添加 .ppk 扩展名。
2. 启动 PuTTY。
3. 单击 Connection > SSH > Auth，再单击 Browse…，选择前面所生成的 .ppk 文件。
4. 单击 Session，

	在 Host Name (or IP address) 里输入账号和需要连接的实例公网 IP 地址，格式为 “root@IP 地址”。
	在 Port 里输入端口号 22；
	Connection type 选择 SSH。
	单击 Open，开始连接您的 Linux 实例。	

## Ubuntu下搭建和配置Nginx Web服务器

### 安装nginx

	apt-get update

	apt-get install nginx

### 启动nginx

	systemctl start nginx
	systemctl status nginx -l
	systemctl enable nginx

### 停止nginx

	systemctl stop nginx

### 测试：

	lsof -i:80


	COMMAND     PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
	AliYunDun   898     root   18u  IPv4  12980      0t0  TCP qyweb01:33654->140.205.140.205:http (CLOSE_WAIT)
	AliYunDun   934     root   18u  IPv4  12980      0t0  TCP qyweb01:33654->140.205.140.205:http (CLOSE_WAIT)
	AliYunDun   934     root   21u  IPv4  13271      0t0  TCP qyweb01:48738->106.11.68.13:http (ESTABLISHED)
	nginx     16177     root    6u  IPv4  33583      0t0  TCP *:http (LISTEN)
	nginx     16177     root    7u  IPv6  33584      0t0  TCP *:http (LISTEN)
	nginx     16178 www-data    6u  IPv4  33583      0t0  TCP *:http (LISTEN)
	nginx     16178 www-data    7u  IPv6  33584      0t0  TCP *:http (LISTEN)
	nginx     16179 www-data    6u  IPv4  33583      0t0  TCP *:http (LISTEN)
	nginx     16179 www-data    7u  IPv6  33584      0t0  TCP *:http (LISTEN)


看到如上的信息，说明nginx启动成功。

nginx配置文件全放在/etc/nginx 下面，用 ll 名令查看如下：

	root@qyweb01:/etc/nginx# ll
	total 64
	drwxr-xr-x  6 root root 4096 Apr 26 12:47 ./
	drwxr-xr-x 90 root root 4096 Apr 26 12:47 ../
	drwxr-xr-x  2 root root 4096 Jul 12  2017 conf.d/
	-rw-r--r--  1 root root 1077 Feb 12  2017 fastcgi.conf
	-rw-r--r--  1 root root 1007 Feb 12  2017 fastcgi_params
	-rw-r--r--  1 root root 2837 Feb 12  2017 koi-utf
	-rw-r--r--  1 root root 2223 Feb 12  2017 koi-win
	-rw-r--r--  1 root root 3957 Feb 12  2017 mime.types
	-rw-r--r--  1 root root 1462 Feb 12  2017 nginx.conf
	-rw-r--r--  1 root root  180 Feb 12  2017 proxy_params
	-rw-r--r--  1 root root  636 Feb 12  2017 scgi_params
	drwxr-xr-x  2 root root 4096 Apr 26 12:47 sites-available/
	drwxr-xr-x  2 root root 4096 Apr 26 12:47 sites-enabled/
	drwxr-xr-x  2 root root 4096 Apr 26 12:47 snippets/
	-rw-r--r--  1 root root  664 Feb 12  2017 uwsgi_params
	-rw-r--r--  1 root root 3071 Feb 12  2017 win-utf

nginx.conf是主配置文件

查看nginx进程是否启动

	 ps -ef|grep nginx

### 配置nginx Web服务器

主要配置文件nginx.conf

sudo vim /etc/nginx/nginx.conf

虚拟主机配置文件

sudo vim /etc/nginx/sites-enabled/default

## Ubuntu下安装node

	sudo apt-get install nodejs-legacy nodejs 
	sudo apt-get install npm

## Windows上传文件到Ubuntu服务器

使用WinSCP工具，传输rar压缩文件

## Linux解压文件

	apt install rar unrar
	
	unrar e build.rar





