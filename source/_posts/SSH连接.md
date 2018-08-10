---
title: SSH连接
date: 2018-04-20 13:11:12
categories: Linx
---

## 服务器端安装SSH

安装SSH：

	yum install openssh-server

启动SSH：

	sevice sshd start

设置开机运行：

	chkconfig sshd on 或

	systemctl enable sshd.service

## 客户端SSH连接服务器端SSH

本地命令行l：

	ssh root@server_ip

回车输入密码连接。

## SSH config

- config方便批量管理多个ssh

- config存放在 ~/.ssh/config

- config语法

	cd ~/.ssh/
	
	vim config

编辑：

	host "zhu"
	    HostName 47.96.140.**
	    User root
	    Port 22

同样可以配置多个host


## SSH免密码登录ssh key

- ssh key使用非对称加密方式生成公钥和私钥
- 私钥存放在本地~/.ssh目录
- 公钥可以对外开放，放在服务器的~/.ssh/authorized_keys

linux生成秘钥

	cd ~/.ssh/
	ssh-keygen 

然后输入文件名字
输入密码
确认密码

生成成功

	Your identification has been saved in file_sra.
	Your public key has been saved in file_sra.pub.
	The key fingerprint is:
	SHA256:NrBxS72quC*************Mf54sqrl8tPwraeHYdKzg root@server
	The key's randomart image is:
	+---[RSA 2048]----+
	|                 |
	|         .       |
	|      o o .      |
	|       * . .     |
	|   .  . S .      |
	|  . +  o.o       |
	|   *.+oo+o.      |
	|  ==E==+==       |
	|  =@OO=*=oo.     |
	+----[SHA256]-----+


	[root@qyweb01 .ssh]# ls -al
	total 24
	drwx------  2 root root 4096 Apr 27 12:14 .
	dr-xr-x---. 5 root root 4096 Apr 27 12:02 ..
	-rw-------  1 root root    0 Apr 27 10:20 authorized_keys
	-rw-r--r--  1 root root   67 Apr 27 11:52 config
	-rw-r--r--  1 root root  174 Apr 27 11:58 known_hosts
	-rw-------  1 root root 1679 Apr 27 12:14 zxl_sra
	-rw-r--r--  1 root root  394 Apr 27 12:14 zxl_sra.pub

把公钥 .pub 里面的内容复制到服务器 ~/.ssh/authorized_keys

这样就可以免密连接服务器

## SSH设置安全端口

ssh默认端口22

防止不法分子利用，最好修改默认端口

修改配置文件

	/etc/ssh/sshd_config

的port部分，

然后重启服务

	systemctl restart sshd.service

最好不要修改默认端口，而是使用复杂的密码，或者使用密钥登陆

