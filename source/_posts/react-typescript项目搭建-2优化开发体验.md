---
title: react typescript项目搭建-2优化开发体验
date: 2019-07-20 13:24:34
categories: react
tags: typescript
---

在前面的基础搭建之后，如何提升开发体验呢，例如 sass，css module 等

## 支持 sass

```bash
npm install -D node-sass sass-loader
npm install -D style-loader css-loader
```

scss 代码的编译过程：

- sass-loader 将 sass 代码编译为 css
- css-loader 将编译出来的代码再次编译成为符合 CommonJS 的代码
- style-loader 将第二步编译出来的代码转为 js 代码
  从上面这三个步骤看出： webpack 进行 loader 编译的顺序是从下到上的

进行配置 webpack（在 module.rules 下面加上.scss 文件类型的编译配置）：

```js
{
  test: /\.scss$/,
  include: [path.join(__dirname, "./../", "src")],
  use: ["style-loader", "css-loader", "sass-loader"]
}
```

## 解决模块声明问题

在根目录下创建 typings 文件夹，里面的 .d.ts 文件进行声明

## 配置公共 sass

自定义： 根目录/src/styles/var.scss 内编写公共的 sass
在其他样式文件里面直接导入使用 @import

## 优化路径

```js
import Test from "../../../../components/Test";
```

改成为这样写：

```js
import Test from "@comonents/Test";
```

> 在 tsconfig 中进行配置:

```js
"baseUrl": "./src"
"paths": {
  "@components/*": ["components/*"]
}
```

> 安装 tsconfig-paths-webpack-plugin 将 tsconfig 中对路径的设置映射到 webpack 配置

```bash
npm install -D tsconfig-paths-webpack-plugin
```

> 修改 webpack 配置：

```js
 resolve: {
    extensions: [".ts", ".tsx", ".js", ".jsx"],
    plugins: [
      new TsconfigPathsPlugin({
        configFile: path.join(__dirname, "./../", "tsconfig.json")
      })
    ]
  },
```

## 构建缓存

我们一般会使用 webpack-dev-server 来进行项目开发，当我们运行 webpack-dev-server 的时候它会在内存中进行项目的构建，但是当使用了 babel 之类的代码转换工具后，会对项目构建产生较大的性能影响，这是因为每一次的构建都会对代码进行重新转换。
而构建缓存就是将构建的公用代码缓存在磁盘上，这样做的效果就是第一次构建的时间花销会比不用缓存的构建大，但是在之后每次构建的时间花销都会大大减少。

在设置构建缓存之前我们首先要考虑的是那些地方需要进行设置：

- 对 ts(x)的转换
- babel 转换
- scss 转换

### 1. 对 ts(x)的转换用的是 awesome-typescript-loader，这个库本身自带了开启缓存的选项 useCache，然后我们需要指定一个保存缓存文件的地方 cacheDirectory，所以 webpack 配置改为如下:

```js
{
  // 匹配要解析的文件
  test: /\.ts(x?)$/,
  use: [
    {
      loader: "awesome-typescript-loader",
      options: {
        useCache: true,
        cacheDirectory: path.join(__dirname, "./../", ".cacahe-loader")
      }
    }
  ]
},
```

### 2. scss 转换构建缓存，需要用到 cache-loader

```bash
npm install -D cache-loader
```

然后在对.scss 文件类型的转换配置中使用它，在这里我们主要是针对转换出来的 css 进行缓存，所以需要写在 css-loader 配置的前面:

```js
{
  test: /\.scss$/,
  include: [path.join(__dirname, "./../", "src")],
  use: [
    "style-loader",
    {
      loader: "cache-loader",
      options: {
        cacheDirectory: path.join(__dirname, "./../", ".cache-loader")
      }
    },
    "css-loader",
    "sass-loader"
  ]
}
```

这样就配置好当前的构建缓存了,在进行 npm run dev 的时候会发现根目录多了.cache-loader 文件夹
