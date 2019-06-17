---
title: Linux使用
date: 2018-04-20 13:11:12
categories: Linux
tags: linux
---

## 软件操作

软件包管理

	yum

安装

	yum install ...

卸载

	yum remove ...

搜索

	yum serach ...

清理缓存

	yum clean packages

列出已安装

	yum list

软件包信息

	yum info ...

## 硬件资源信息



- 内存

	free -m

- 硬盘

	df -h

- 负载（w或top）

	w

	12:53:49 up  2:33,  3 users,  load average: 0.00, 0.01, 0.05
	USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
	root     tty2                      10:32    1:59m  0.04s  0.04s -bash
	root     pts/0    121.239.246.23   11:34    1:13m  0.01s  0.01s -bash
	root     pts/1    121.239.246.23   12:24    5.00s  0.02s  0.00s w

	load average: 0.00, 0.01, 0.05  分别表示最近1分钟，5分钟，15分钟时候的负载值

	当到达1时说明负载快要超荷。0.6-0.7是健康值。

- cpu个数和核数

	cat /proc/cpuinfo

## 文件操作

####目录结构

根目录/

家目录 /home

临时目录 /tmp

配置目录 /etc

用户程序目录 /usr

####文件基本操作

	touch --新建文件
	rm --删除文件和目录
	cp --复制
	mv --移动
	pwd --显示路径

####vim

	gg--定位到行头
	G--定位到行尾
	dd--删除整行
	u--恢复

####文件权限421

	r--4 可读
	w--2 可写
	x--1 可执行

####文件搜索，查找

	tail--从尾部开始读
	head--从头部开始读
	cat--读取整个文件
	more--分页读取
	less--可控分页
	grep--搜索关键字
	find--查找文件
	wc--统计个数

	find . -name "*.conf" --查找当前路径下以conf后缀的文件
	find . -type  f --查找当前路径下的文件
	find . -type  d --查找当前路径下的文件夹
	find . -ctime -20 --查找20天内更新过的文件

#### 文件压缩和解压tar和untar

把zxl文件压缩为zxl.tar：

	tar -cf zxl.tar zxl

查看zxl.tar里面的内容：

	tar -tf zxl.tar

查看zxl.tar里面的详细内容：

	tar -tvf zxl.tar

把zxl.tar解压：

	tar -xf zxl.tar
 
## 系统用户操作命令

#### 添加用户

	cd /home/
	useradd zhaolin

这时查看home里面会多了一个文件夹zhaolin

给zhaolin用户添加密码：

	passwd zhaolin

输入密码并确认密码。

这样就有了一个新的用户zhaolin，可以用ssh用zhaolin用户连接。

#### 删除用户

	userdel zhaolin  --删除用户zhaolin
	rm -rf zhaolin  --删除用户zhaolin目录
	
	userdel -r zhaolin --彻底删除用户以及用户文件夹


## 防火墙操作

作用：保护服务器安全

安装：

	 yum install firewalld

启动： 

	service firewalld started

启动： 

	service firewalld restarted

列出防火墙服务：
	
	 yum list | grep firewall

查看是否启动防火墙：

	ps -ef|grep firewall

停止：

	service firewalld stop

	service firewalld status

## 提权

root账号中

	visudo

修改：

	## Allows people in group wheel to run all commands
	%wheel  ALL=(ALL)       ALL
	%zhaolin ALL=(ALL)    ALL

在zhaolin用户进行操作以前，前面加上sudo即可

	sudo yum install XXXXX

## 文件上传

#### linux: scp

	scp zhaolin@47.96.140.xx:/tmp/abc.txt ./  
	把服务器47.96.140.xx上tmp里面的文件传到本地当前目录
	
	scp abc.txt  zhaolin@47.96.140.xx:/tmp  
	把本地当前目录的文件上传到服务器47.96.140.xx上tmp

#### windows xshell

- 首先xshell连接linux，
- 然后再linux服务器上安装：

	yum install lrzsz

装好以后执行命令：

	rz

会出现弹框：

选择文件并上传即可


## 文件下载

	wget XXXXX