---
title: nginx编译
date: 2018-11-19 10:10:50
categories: Linux
tags: nginx
---

安装nginx有两种方法，一种是直接通过yarn/apt-get直接来安装，一种就是编译

通过yarn/apt-get直接来安装，有一个问题，就是nginx的二进制文件会把模块直接编译进去，nignx的官方模块并不是每一个都会开启，如果想要添加第三方的nginx模块，必须通过编译的方式进行添加。

推荐编译安装

#### 下载nginx

打开nginx.org

找到stable版本(当前stable版本为1.14.1)，复制下载地址，进入linux你想要安装的目录

    wget http://nginx.org/download/nginx-1.14.1.tar.gz

    tar -xzf nginx-1.14.1.tar.gz 进行解压

    cd nginx-1.14.1 进入解压后的源码目录,查看目录如下：

    总用量 748
    drwxr-xr-x 6 mysql mysql   4096 11月 19 10:31 auto
    -rw-r--r-- 1 mysql mysql 287441 11月  6 21:52 CHANGES
    -rw-r--r-- 1 mysql mysql 438114 11月  6 21:52 CHANGES.ru
    drwxr-xr-x 2 mysql mysql   4096 11月 19 10:31 conf
    -rwxr-xr-x 1 mysql mysql   2502 11月  6 21:52 configure
    drwxr-xr-x 4 mysql mysql   4096 11月 19 10:31 contrib
    drwxr-xr-x 2 mysql mysql   4096 11月 19 10:31 html
    -rw-r--r-- 1 mysql mysql   1397 11月  6 21:52 LICENSE
    drwxr-xr-x 2 mysql mysql   4096 11月 19 10:31 man
    -rw-r--r-- 1 mysql mysql     49 11月  6 21:52 README
    drwxr-xr-x 9 mysql mysql   4096 11月 19 10:31 src


#### nginx源码各个目录

1.auto目录：

    总用量 204
    drwxr-xr-x  2 mysql mysql  4096 11月 19 10:31 cc
    -rw-r--r--  1 mysql mysql   141 11月  6 21:52 define
    -rw-r--r--  1 mysql mysql   889 11月  6 21:52 endianness
    -rw-r--r--  1 mysql mysql  2812 11月  6 21:52 feature
    -rw-r--r--  1 mysql mysql   136 11月  6 21:52 have
    -rw-r--r--  1 mysql mysql   137 11月  6 21:52 have_headers
    -rw-r--r--  1 mysql mysql   411 11月  6 21:52 headers
    -rw-r--r--  1 mysql mysql  1020 11月  6 21:52 include
    -rw-r--r--  1 mysql mysql   745 11月  6 21:52 init
    -rw-r--r--  1 mysql mysql  4836 11月  6 21:52 install
    drwxr-xr-x 11 mysql mysql  4096 11月 19 10:31 lib
    -rw-r--r--  1 mysql mysql 18253 11月  6 21:52 make
    -rw-r--r--  1 mysql mysql  3183 11月  6 21:52 module
    -rw-r--r--  1 mysql mysql 37857 11月  6 21:52 modules
    -rw-r--r--  1 mysql mysql   136 11月  6 21:52 nohave
    -rw-r--r--  1 mysql mysql 24767 11月  6 21:52 options
    drwxr-xr-x  2 mysql mysql  4096 11月 19 10:31 os
    -rw-r--r--  1 mysql mysql  8654 11月  6 21:52 sources
    -rw-r--r--  1 mysql mysql   120 11月  6 21:52 stubs
    -rw-r--r--  1 mysql mysql  2014 11月  6 21:52 summary
    -rw-r--r--  1 mysql mysql   394 11月  6 21:52 threads
    drwxr-xr-x  2 mysql mysql  4096 11月 19 10:31 types
    -rw-r--r--  1 mysql mysql 26859 11月  6 21:52 unix

包含四个子目录： cc--用于编译,lib--lib库,os--判断操作系统,types

其他所有的都是为了配合configure脚本执行的时候去判定nginx支持哪些模块，当前操作系统有什么特性供nginx使用，

2.CHANGES文件：nginx每个版本中的特性和bugfix

3.conf是一个配置示例文件

4.configure脚本是用来生成中间文件，执行编译前的必备动作

5.contrib目录,vim编辑器的nginx语法高亮显示

6.html目录, 里面是500错误，重定向到的页面，还有一个是index默认页面

7.man目录，是linux对nginx的帮助文件

8.src目录, 是nginx的源代码

#### 编译

configure支持哪些参数呢：

    ./configure --help | more 

    结果：

      --help                             print this message

      --prefix=PATH                      set installation prefix
      --sbin-path=PATH                   set nginx binary pathname
      --modules-path=PATH                set modules path
      --conf-path=PATH                   set nginx.conf pathname
      --error-log-path=PATH              set error log pathname
      --pid-path=PATH                    set nginx.pid pathname
      --lock-path=PATH                   set nginx.lock pathname

      --user=USER                        set non-privileged user for
                                        worker processes
      --group=GROUP                      set non-privileged group for
                                        worker processes

      --build=NAME                       set build name
      --builddir=DIR                     set build directory

      --with-select_module               enable select module
      --without-select_module            disable select module
      --with-poll_module                 enable poll module
      --without-poll_module              disable poll module
    --More--

1.开始编译（使用默认参数prefix）：

    ./configure --prefix=/home/zhaoxuelin/nginx (指定了nginx安装目录)

  如果没有报错，nignx就在指定目录安装成功了  

  此过程生成的中间文件放在新增加的objs目录下

    objs:

    总用量 84
    -rw-r--r-- 1 root root 17763 11月 19 10:59 autoconf.err
    -rw-r--r-- 1 root root 39736 11月 19 10:59 Makefile
    -rw-r--r-- 1 root root  6816 11月 19 10:59 ngx_auto_config.h
    -rw-r--r-- 1 root root   657 11月 19 10:59 ngx_auto_headers.h
    -rw-r--r-- 1 root root  5725 11月 19 10:59 ngx_modules.c  (决定了哪些模块被编译进nginx)
    drwxr-xr-x 9 root root  4096 11月 19 10:59 src

2.接下来执行make编译

    make

编译完成以后，就会生成大量中间文件和运行nginx的二进制文件。可以在objs目录中看到。

3.执行make install首次安装可以执行这个命令

    make install

以上就是编译nginx的步骤