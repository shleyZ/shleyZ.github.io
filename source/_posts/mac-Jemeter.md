---
title: mac Jemeter
date: 2018-09-07 07:35:24
categories: automated testing
---

## 第一步
[下载Jemeter](http://jmeter.apache.org/download_jmeter.cgi)

下载Binaries里面的.tgz

## 第二步

解压apache-jmeter-4.0.tgz, 使用命令：

    tar -zxvf apache-jmeter-4.0.tgz

## 第三步

打开刚解压到的文件apache-jmeter-3.3，切换到/bin目录下，启动Jmeter:

    sh jmeter



## 前提是配置好java环境    


[MAC安装JDK及环境变量配置](https://blog.csdn.net/vvv_110/article/details/72897142)



##  mac 配置 jmeter 环境变量

编辑用户目录下.bash_profile文件

    vim .bash_profile

添加

    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-10.0.2.jdk/Contents/Home
    export JMETER_HOME=/Users/******/apache-jmeter-3.2
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JMETER_HOME/lib/ext/ApacheJMeter_core.jar:$JMETER_HOME/lib/jorphan.jar:$JMETER_HOME/lib/logkit-2.0.jar
    export PATH=$JAVA_HOME/bin:$JMETER_HOME/bin:$PATH

上面的 /******/ 填自己的用户名,JAVA_HOME里面的版本号填自己的。


保存退出后

    source ~/.bash_profile

