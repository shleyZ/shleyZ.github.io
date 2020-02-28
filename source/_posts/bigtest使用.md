---
title: '@bigtest使用'
date: 2020-01-07 15:44:08
categories: bigtest
tags: bigtest
---

假设您的应用程序正在使用以下技术：

- React
- Mocha
- Webpack or Parcel

## Installing Dependencies

首先，安装 BigTest 所需的软件包：

```bash
yarn add --dev @bigtest/cli @bigtest/interactor @bigtest/react
```

- @bigtest/cli 将使你能够访问文章内使用的 bigtest 命令。
- @bigtest/interactor 允许您的测试以类似人与应用交互的方式与应用进行交互。 他们将等待元素出现，然后再与他们进行交互，这意味着您不必担心正确地安排测试时间以与任何运行循环同步。 （谁还有时间这样做？）
- @bigtest/react：React DOM helper，用于设置您的应用程序以进行验收测试。

## 初始化 BigTest

现在已经安装了所有依赖项，然后转到项目根目录并运行

```bash
npx bigtest init
```

这将创建一个新的 bigtest 目录：

bigtest/
├── bigtest.opts
├── index.js
├── helpers/
│ └── setup-app.js
├── interactors/
│ └── app.js
└── tests/
└── app-test.js

## 导入你的应用

您需要将应用程序根组件导入到 bigtest/helpers/setup-app.js 文件中。
导入根组件：

```js
import { setupAppForTesting } from '@bigtest/react';
import YourApp from '../../src/YourApp';

export async function setupApplicationForTesting() {
  await setupAppForTesting(YourApp, () => {
    mountId: 'bigtesting-container';
  });
}
```

## 启动和服务

BigTest 通过将您的应用程序与您编写的测试文件捆绑在一起来工作。
创建的所有测试都需要导入到 bigtest/index.js。
您需要通过将捆绑器的入口点更改为 bigtest/index.js 来告诉捆绑器如何将测试与应用捆绑在一起。

bigtest CLI 会设置 NODE_ENV 为您测试。以 Webpack 为例，您可以检查 NODE_ENV 是否等于 test 并根据需要更改入口点：

```js
// in your webpack.config.js
let isTesting = process.env.NODE_ENV === 'test';
module.exports = {
  entry: isTesting ? '.bigtest/index.js' : './src/index.js'
};
```

### bigtest run

bigtest run 处理启动不同的浏览器并实际运行测试的过程。将脚本添加到 package.json：

```json
"scripts": {
  "test": "bigtest run"
}
```

因此，我们使用的是 package.json 中指定的@bigtest/cli 项目版本，而不是全局安装。

### bigtest.opts

bigtest.opts 文件使启动器（bigtest 运行）知道：

- 如何启动您的应用服务器（--serve）
- 应用服务的位置（--serve-url）
- 您正在使用什么测试框架（--adapter），目前最大的启动器仅适用于 mocha，但我们正在努力添加更多的适配器。

bigtest.opts 文件是一个配置文件，其中包含可以作为标志传递给 BigTest CLI 的各种选项。从 CLI 传递的参数优先于 opts 文件中指定的参数。

bigtest.opts 应该看起来像这样：

```bash
--serve "yarn start"
--serve-url "http://localhost:3000"
--adapter mocha
```

## 运行测试

现在，所有内容都已设置好，可以通过运行我们之前设置的测试命令进行检查：

```bash
yarn test
```
