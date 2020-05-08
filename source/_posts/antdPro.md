---
title: antdPro
date: 2020-05-08 10:21:02
categories: antdPro
tags: antdPro
---

作者是这样描述的：dva 是 react 和 redux 的最佳实践。

而 antd-pro 就是作者把 UI 工具 antd，打包工具 roadhog，路由 react-router，状态管理器 dva 整合在一起

## dva 数据流

- service：请求定义，并且按照 model 的维度进行拆分

- utils/request.ts：统一的请求发送和错误处理

- model: 处理数据逻辑部分，包含 state 初始值 和 reducers（相当于 redux 里面的 reducer，根据 action 来更新 state）

#### 创建一个页面，除了需要配置相应的路由和权限，还需要两个部分，一个是 ui 组件，另一个就是 model 部分。
#### 在组件内部调用 dispatch 方法，根据传的 type 来执行 model 里面的对应的命名空间的 effects，进而可以修改 state 值。
#### 在 UI 组件内部使用高阶组件 connect 包裹组件，可以在组件内部使用 model 里面的 state。


## 1. 组件内部调用 dispatch 一个 action，需要指定命名空间的 model 里面的 action：

![dispatch](https://user-images.githubusercontent.com/11802509/81363902-fbead280-9116-11ea-80fc-172b86c984f1.png)

## 2. model 部分

![dispatch](https://user-images.githubusercontent.com/11802509/81364365-0a85b980-9118-11ea-886a-3d73f3b95c8f.png)

## 3. services 里面进行 请求的定义

![dispatch](https://user-images.githubusercontent.com/11802509/81364456-43259300-9118-11ea-908d-4536a8f756a3.png)


### 一个 model 里面会有这些属性:
- state：model 的状态数据，一般是不可变的，只有 action 才能修改 state 值

- action：改变 state 的唯一途径，需要 dispatch 来触发 action，需要传一个 type 值，来指定 state 的修改。 dispatch 一般在组件 connect 以后从 props 里面可以获取到

- dispatch：触发 action 的函数，在组件 connect 以后从 props 里面可以获取到 dispatch

- reducers：是一个纯函数，接收 state 和 action，返回新的 state , 同步修改状态

- effect：多用于异步修改状态，将异步操作转换成同步的写法，来实现纯函数
- subscription：订阅一个数据源，根据条件来 dispacth 一个 action，一般用来监听路由变化

此外每个 model 还需要定义一个 namespace 命名空间，也是全局 state 里面的属性，

例如 connect 指定的 state 时会用到，dispatch 指定的命名空间的 effects 也会用到

### model 里面的 effect 的使用:
- call 一般用来调用 api 接口，并可以获取到返回值
- put 大致与 dispatch 相同，可让 effects 里的方法调用修改 state 的方法
- select 用于获取 state 变量
此外 antd-pro 引入了 typescript，对 js 代码进行的类型检测，配合 tslint，能够严格约束代码的编写，虽然有些时候遇到的一些类型警报，可以用 any 类型解决，但是非常不建议这样做。