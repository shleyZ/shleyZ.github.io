---
title: react typescript项目搭建-2优化开发体验
date: 2019-07-21 10:20:14
categories: react
tags: typescript
---

## 集成 antd

npm install -S antd

> 修改 antd 的主题配色

antd 的样式使用 less 进行编写, 首先要安装 less 和 less-loader

注意 less 版本要小于 3.0，否则会报错

1. npm install -D less less-loader

2. 在根目录下添加一个 theme.js 文件

```js
module.exports = {
  "primary-color": "red",
  "border-radius-base": "20px"
};
```

3. 在 build/rules/styleRules 中添加针对 less 文件的 loader

```js
const { resolve } = require("../utils");
const theme = require("./../../theme");
module.exports = [
  {
    test: /\.less$/,
    include: [resolve("node_modules")],
    use: [
      "style-loader",
      "css-loader",
      {
        loader: "less-loader",
        options: {
          // 禁用内联js代码
          javascriptEnable: false,
          // 根据antd官网进行主题修改
          modifyVars: theme
        }
      }
    ]
  }
];
```

4. 全局引入 antd 的样式文件

在 src/index.tsx 中

import "antd/dist/antd.less";

## antd 按需加载

import { Button } from 'antd';

这样的引入方式存在一个很大的弊端，就是在引入其中某个组件的同时会把整个 antd 文件都引入进来，影响构建速度，而且打包后体积会变大， 因此需要做 antd 的按需加载:

```js
//tsconfig.json
{
  ...
  "module": "ESNext",
  ...
}
```

```bash
npm install -D ts-import-plugin
```

修改 build/rules/jsRule.js

```js
const { resolve } = require("../utils");
const tsImportPluginFactory = require("ts-import-plugin");

module.exports = [
  {
    // 匹配要解析的文件
    test: /\.ts(x?)$/,
    use: [
      {
        loader: "awesome-typescript-loader",
        options: {
          transpileOnly: true,
          useCache: true,
          cacheDirectory: resolve(".cache-loader"),
          getCustomTransformers: () => ({
            before: [
              tsImportPluginFactory({
                libraryName: "antd",
                libraryDirectory: "lib",
                style: true // 填写 true 使用组件的 less 文件， 填写 css 使用css文件但是同时不能定制主题
              })
            ]
          })
        }
      }
    ]
  }
];
```

## 热加载

npm install -D react-hot-loader

## 集成 mobx

npm install -S mobx

npm install -S mobx-react
