---
title: Nodejs与浏览器
date: 2020-04-20 10:03:35
tags: Nodjs
---

### 在 node 里运行的 js 与在 Chrome 里运行的 js 有什么不同？

其实在 Node 里面写 js 和在 Chrome 写 js 基本上没有什么不同。
不一样的地方就是： Node 没有浏览器的 API,例如 document，window 等等，但是加了许多 Nodejs 的 API.

Nodejs 不只控制浏览器，而是控制整个计算机
Chome 里面的 js 只是控制浏览器

总结：
**Nodejs 是一个基于 Chrome V8 引擎的 javascript 运行环境。**
**Nodejs 使用了一个事件驱动，非阻塞 I/O 模型。**


### 为什么使用 Nodejs 呢

**搜索引擎SEO优化、首屏加载速度优化**
**服务端渲染，前后端同构**

### BFF 层：backend for frontend。也就是属于浏览器与后台服务的中间渲染层，为了前端服务的后台服务。

对用户提供 http 服务
使用后端 RPC 服务

### Nodejs 模块规范 CommonJS：

Chrome 浏览器、VS Code、Nodejs