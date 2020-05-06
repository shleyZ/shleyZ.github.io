---
title: webpack构建多页面打包
date: 2019-10-17 19:02:14
categories: webpack
---

极客时间版权所有: https://time.geekbang.org/dailylesson/detail/100028427

我们通常说的多页面应用，就是由多个完整页面构成的，它的优势是首屏时间快，SEO 效果好。
多页面应用特别常见，因此针对多页面应用提供一个通用的 webpack 打包方案，就显得很重要了。

实现多页面应用的核心就是 webpack 多个 entry 配置

但是如果我们总是去操作 webpack 来添加删除 entry 就会不是很方便，那么怎样实现不把多个 entry 值暴露给开发者，而是自动配置呢。

那就是设置一个合理的 entry 匹配规则，让 webpack 的 entry 的值自动计算出来。
还有一个关键点就是 html-webpack-plugin 的数量，多个页面就会需要多个数量的 html-webpack-plugin（根据 HTML 模版自动生成 HTML 页面,同时会把 js 和 css 注入进去）

总结起来就是：

> 1. 设置一个合理的 entry 匹配规则
> 2. 自动计算 entry 值
> 3. 根据 entry 的 key 值增加对应的 html-webpack-plugin

首先我们要规范目录结构，可以用脚手架，也可以自定义，如下

.

├── node_modules
├── src
│     ├── pages
│         ├── page1
│         │      ├── index.js
│         │      └── index.html
│         ├── page2
│         │      ├── index.js
│         │      └── index.html
│         │
│         └── page3
│                ├── index.js
│                └── index.html
├── package-lock.json
├── package.json
└── webpack.config.js

这样我们只要找出 src/pages 下面的文件夹