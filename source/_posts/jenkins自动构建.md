---
title: gitlab-jenkins自动构建
date: 2019-01-17 13:49:25
categories: javascript
---


在部门的测试环境中，开发人员一旦向gitlab仓库提交成功代码，gitlab就会自动触发jenkins构建项目。当然在构建后还可以添加项目部署或者自动化测试的脚本。这里只针对测试环境。

jenkins 依赖java,首先安装java sdk

安装jenkins:

    brew install jenkins

启动 

    jenkins

开机自启动：

    ln -sfv /usr/local/opt/jenkins/*.plist ~/Library/LaunchAgents

登录： 

    http://localhost:8080

安装插件

安装Gitlab Hook Plugin插件：

    Gitlab Hook Plugin
    Build Authorization Token Root Plugin

创建用户登陆进行构建





