---
title: js性能优化相关
date: 2020-02-03 18:05:05
categories: javascript
tags: js基础
---


## 性能优化有：

### nginx 开启 gzip 压缩。

### 非核心代码异步加载: 

```js
<script src="script.js"></script>  // 正常加载脚本
<script async src="script.js"></script> // 使用 async 异步加载
<script defer src="myscript.js"></script> // 使用 defer 异步加载
```

async 和 defer 的区别：

两者的读取都是在加载其他后续文档和渲染时一起加载，都是异步加载。
但是执行的时间不同：

- async 是异步加载完成后立即异步执行
- defer 要等其他元素解析完成之后才能执行

### 按需加载

webpack 3/4 自带按需加载

- 对于 import 的内容进行处理，新建一个chunk。
- 当代码执行到 import 才会加载对应的 chunk 内容。
- import 返回的是一个 Promise， 只有加载成功才能获取到加载的内容。

### 提取公共 chunk

对于 webpack 的chunks, 可能出现公用的组件，如果不进行处理，会出现：

- 相同的资源被重复的加载，浪费用户的流量和服务器的成本；
- 每个页面需要加载的资源太大，导致首次加载缓慢，影响用户体验；

这里就需要用到 提取公共 chunks。

**（要知道这样并不会降低页面首次加载的资源大小，反而有可能加大资源的大小。但是对于后面子页面资源的加载性能会有显著性提升。）**

公共组件提取的实现：

- 使用 webpack3 内置的 CommonsChunkPlugin， 需要说明哪些chunk需要单独提取出来，以及拆分出来 chunks 的命名；
- 使用 webpack4 的 optimization.splitChunks: 需要在 缓存组 cacheGroups 里面定义拆分规则：


```js
// webpack.config.js
optimization: {
  minimize: true,
  splitChunks: {
    chunks: 'all',
    minSize: 30000, // 提取出的 chunk 的最小大小
    minChunks: 3,  // 模块被引用 3 次以上的才抽离
    automaticNameDelimiter: '.', 
    cacheGroups: {
      vendor: {
        name: `vendors`, // chunk 名
        test: /[\\/]node_modules[\\/]/, // 提取所有 node_modules 里面的引用
        priority: 10, 
      },
    },
  },
},
```

### 使用 CDN 加载

浏览器第一次打开页面时，缓存是起不了作用的。这个时候，CDN就上场了。


### DNS预解析： http-equiv

```html
<meta http-equiv="x-dns-prefetch-control" content="on"> <!-- 强制打开 DNS 预解析 -->
<link rel="dns-prefetch" href="http://www.smyhvae.com/">  <!-- 对指定的域名（第三方资源）进行DNS预解析 -->
```

需要说明的是在一些高级浏览器里面对于超链接<a>标签， 默认开启了DNS预解析，

但是如果是 https 协议，很多浏览器是关闭了DNS预解析， 如果加上 meta 会强制打开 DNS 预解析。