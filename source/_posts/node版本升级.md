---
title: node 版本升级
date: 2018-07-29 14:02:01
categories: node
---


### 首先：查看当前node版本：
    
    node –v

### 安装n模块：

    npm install -g n

### 升级到指定版本/最新版本（该步骤可能需要花费一些时间）升级之前，可以执行n ls （查看可升级的版本）

如：

    n 6.9.1

或者你也可以告诉管理器，安装最新的稳定版本

    n stable

### 安装完成后，查看Node的版本，检查升级是否成功

    node -v
