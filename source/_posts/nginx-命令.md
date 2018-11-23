---
title: nginx 命令
date: 2018-11-21 15:51:03
categories: nginx
---

#### 格式和linux命令差不多

nginx -s reload (-s是发送信号指令，reload是参数)

帮助： -？或者 -h

使用指定配置文件： -c

指定配置指令：-g

指定运行目录： -p

发送信号： -s

        nginx -s stop   立即停止服务

        nginx -s quit   优雅的停止服务

        nginx -s reload  重载配置文件

        nginx -s reopen  重载开始记录日志文件

测试配置文件是否有语法错误： -t -T

nginx版本信息： -v -V


#### 重载配置文件

在/home/********/nginx/sbin目录下：

        ./nginx -s reload

如果出现错误./nginx可以查看错误，如下：

        nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
        nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
        nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
        nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
        nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
        
此时说明80端口被占用，

查看是哪个进程被占用，如果是nginx，停止nginx服务，或者kill -9 进程号进行强制杀死进程

        netstat -lnp|grep 80命令

再重新reload即可成功

#### 升级nginx版本

首先备份/nginx/sbin/nginx二进制文件

        [root@zhao sbin]# ls
        nginx
        [root@zhao sbin]# mv nginx nginx.bak
        [root@zhao sbin]# ls
        nginx.bak 

然后把已经编译好的新的二进制文件拷贝进sbin目录

        [root@zhao sbin]# cp -r /目录/nginx /home/zh****/nginx/sbin/

        [root@zhao sbin]# ls
        nginx  nginx.bak

然后给正在运行的nginx的master进程发送信号，告诉它我要热部署了，要进行版本升级

        [root@zhao sbin]# ps -ef |grep nginx
        root      8732     1  0 10:09 ?        00:00:00 nginx: master process ./nginx
        nobody    8740  8732  0 10:09 ?        00:00:00 nginx: worker process
        root      9769  9226  0 10:29 pts/1    00:00:00 grep --color=auto nginx
        [root@zhao sbin]# kill -USR2 8732
        [root@zhao sbin]# ps -ef |grep nginx
        root      8732     1  0 10:09 ?        00:00:00 nginx: master process ./nginx
        nobody    8740  8732  0 10:09 ?        00:00:00 nginx: worker process
        root      9787  8732  0 10:29 ?        00:00:00 nginx: master process ./nginx
        nobody    9788  9787  0 10:29 ?        00:00:00 nginx: worker process
        root      9794  9226  0 10:29 pts/1    00:00:00 grep --color=auto nginx
        [root@zhao sbin]# 

发现又新起了一个master进程

发送信号给旧的nginx的master进程，让他优雅的关闭

        [root@zhao sbin]# kill -WINCH 8732
        [root@zhao sbin]# ps -ef |grep nginx
        root      8732     1  0 10:09 ?        00:00:00 nginx: master process ./nginx
        root      9787  8732  0 10:29 ?        00:00:00 nginx: master process ./nginx
        nobody    9788  9787  0 10:29 ?        00:00:00 nginx: worker process
        root      9985  9226  0 10:33 pts/1    00:00:00 grep --color=auto nginx
        [root@zhao sbin]# 

发现旧的master进程还存在，但是他的worker进程已经没有了，说明所有的请求已经新升级到新的worker进程中了。
此时旧的8732可以不要kill掉，因为在需要版本回退时，或者新版本有问题时，只需要重新给8732发送reload命令，把worker进程拉起来，再把新版本关掉


#### 日志分割

        [root@zhao logs]# ll
        总用量 20
        -rw-r--r-- 1 root root 662618 11月 22 10:37 access.log
        -rw-r--r-- 1 root root   6112 11月 22 10:33 error.log
        -rw-r--r-- 1 root root      5 11月 22 10:29 nginx.pid
        -rw-r--r-- 1 root root      5 11月 22 10:09 nginx.pid.oldbin
        [root@zhao logs]# 

当我们发现log文件太大时，需要把以前的日志备份到其他文件，但是nginx还是正常运行，通过reopen命令实现

        [root@zhao logs]# mv access.log bake.log
        [root@zhao logs]# ll
        总用量 20
        -rw-r--r-- 1 root root 662618 11月 22 10:43 bake.log
        -rw-r--r-- 1 root root   6343 11月 22 10:43 error.log
        -rw-r--r-- 1 root root      5 11月 22 10:29 nginx.pid
        -rw-r--r-- 1 root root      5 11月 22 10:09 nginx.pid.oldbin
        [root@zhao logs]# ../sbin/nginx -s reopen
        [root@zhao logs]# ll
        总用量 20
        -rw-r--r-- 1 nobody root      0 11月 22 10:43 access.log
        -rw-r--r-- 1 root   root 662618 11月 22 10:43 bake.log
        -rw-r--r-- 1 nobody root   6404 11月 22 10:43 error.log
        -rw-r--r-- 1 root   root      5 11月 22 10:29 nginx.pid
        -rw-r--r-- 1 root   root      5 11月 22 10:09 nginx.pid.oldbin
        [root@zhao logs]# 

一般最好日志文件要每天备份，所以最好写在bash文件里面自动执行每天备份

        
