---
title: rn调试中遇到的问题
date: 2019-06-27 15:30:06
categories: react-native
tags: react-native
---

## 1.Could not connect to development server（Android）解决方法

![Could not connect to development server](http://ww1.sinaimg.cn/large/8c85763dgy1g4fr0vai8pj20u01hck59.jpg)

解决办法：

```bash
adb reverse tcp:8081 tcp:8081
```

如果提示 adb 找不到什么的，需要在环境变量中添加以下并重新 source ：

- 打开文件：vi ~/.bash_profile

- 添加：

```bash
export PATH=${PATH}:/Users/******/Library/Android/sdk/platform-tools
```

- 保存以后：source ~/.bash_profile

执行完以上后重新 react-native run-android

## 2. android 不用摇晃手机，即可调出 Reload 等选项

```bash
adb shell input keyevent 82
```

## 3.
