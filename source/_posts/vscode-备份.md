---
title: vscode 备份
date: 2019-04-11 15:14:21
categories: vscode
tags: vscode
---
使用Visual Studio  Code需要安装一些常用插件及根据个人的喜好进行一些设置,但如果换电脑,或者重装了系统,那么又要重新安装及设置,所以最好还是备份一下

使用Setting Sync插件将目前配置保存到GitHub上，以后只需要从GitHub上获取，就可以一次性安装插件配置信息

### 1.首先在VSCode里面搜索Setting Sync插件，安装好后重新加载激活

    l Upload Key : Shift + Alt + U 快捷键备份(上传)

    l Download Key : Shift + Alt + D快捷键恢复(下载)


### 2.打开编辑器按下快捷键 Shift + Alt + U快捷键备份(上传)

这一步要有文件打开时才有用

### 3. 跳转到github, Generate new tooken(要先登录哦)

token description: vscode-async备注一下

select scopes: 选择gist---create gists

点击Generate token 生成一串key

将生成的key输入vscode命令框里 回车

弹出控制台提示备份(上传)插件成功

上传完成后会生成一个ID, 

    setting->用户设置->'sync.gist':"一串字符串组成的ID"

### 4.key和ID都要记下来 下载(恢复)插件需要用到

### 5.下载(恢复)插件

Shift + Alt + D  输入ID  即可开始同步配置

将上传(备份)好插件的key和ID输入vscode命令框里 回车



备份一下我自己的

    github key: 3036e7cdffd1ff62b80e69be10a9753797a54cda
    vscode ID: 3b3adac00ea2bb6100d53c932c15fa18
