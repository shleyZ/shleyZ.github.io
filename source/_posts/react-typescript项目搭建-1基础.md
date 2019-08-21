---
title: react typescript项目搭建-1基础
date: 2019-07-20 11:19:10
categories: react
tags: typescript
---

最近公司有了新的项目，所以为开发做准备，并且提供一个普适的模版，自己搭建一个 react+ typescript 项目

## 起步安装

```js
// 全局安装 typescript
npm install -g typescript

// 创建项目文件夹
mkdir iot-back && cd iot-back

// 项目初始化，生成package.json和tsconfig.json
npm init -y && tsc --init

// 安装webpack
npm install -D webpack webpack-cli webpack-dev-server

// 安装react相关和ts包
npm install -S react react-dom npm install -D @types/react @types/react-dom

// 安装ts-loader
npm install -D awesome-typescript-loader
```

这时候，起步安装基本上就完成了。

## 项目配置

> 项目根目录创建 build 文件夹，在 build 下创建 webpack.config.js

> 项目根目录创建 src 文件夹，在 src 下创建 index.js 作为项目入口

> 编写 webpack 配置文件 webpack.config.js，添加 entry 和 output

```js
const path = require("path");

module.exports = {
  entry: {
    app: path.join(__dirname, "./../", "src/index.tsx")
  },
  output: {
    path: path.join(__dirname, "./../", "dist"),
    filename: "[name].js"
  }
};
```

> webpack.config.js 内继续编写 awesome-typescript-loader 的配置，解析.tsx 文件,也就是 module 模块

```js
module: {
  rules: [
    {
      // 匹配要解析的文件
      test: /\.ts(x?)$/,
      use: [
        {
          loader: "awesome-typescript-loader",
          options: {}
        }
      ]
    }
  ];
}
```

> webpack.config.js 配置导入文件类型,这样以后引入文件就不需要带扩展名

```js
resolve: {
  extensions: [".ts", ".tsx", ".js", ".jsx"]
},
```

> 在 tsconfig 里面没有指定 JSX 的版本

compilerOptions 中添加"jsx": "react"

> src/index.tsx 中编辑入口文件

```js
import * as React from "react";
import * as ReactDOM from "react-dom";

const render = () => {
  ReactDOM.render(<div>123</div>, document.querySelector("#app"));
};
render();
```

> 添加页面模板

根目录 > build > tpl > index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

> 将页面模板和打包出来的 js 文件关联起来, 因为考虑到以后打包出来的 js 的文件不会是一个固定的名称，所以这里需要使用一个 webpack 的插件 html-webpack-plugin

```bash
npm install -D html-webpack-plugin
```

然后在 webpack 的 plugins 配置项下进行一些简单配置

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
plugins: [
  new HtmlWebpackPlugin({
    template: "build/tpl/index.html"
  })
];
```

配置完成后就可以启动项目了

> package.json 中添加启动命令

```js
"dev": "webpack-dev-server --config build/webpack.config.js --mode development"
```

启动命令： npm run dev
