---
title: 前端安全问题-XSS跨站脚本攻击
date: 2020-02-04 14:37:33
categories: javascript
tags: js基础
---

## XSS（Cross Site Scripting）跨站脚本攻击

攻击者在 WEB 页面插入恶意 script 代码，在用户浏览页面时，被执行，从而达到某些目的。

例如攻击者在输入框内输入

```js
<script>alert('hey, you are attacked!')</script>
```

这样这段代码很可能会在浏览器解析的时候被执行，今天弹出信息框。

在实际情况中，攻击者不会简单的这样操作，恶意植入代码的作用有很多，例如：

- 窃取用户信息，cookie 或者本地 用户认证
- 劫持流量实现恶意跳转等


XSS种类分为： 

- 反射型 XSS： 通过在访问的 url 中加入恶意脚本传递给服务器
- 存储型 XSS： 攻击者将恶意代码提交到服务器中，普通用户访问时服务器将恶意代码返回，浏览器默认执行。

### REACT 本身对 XSS攻 击的处理：

1. **在React中，React 虚拟 DOM 会在渲染所有输入内容之前，进行转义(''，"", &, <, >)，所有内容在渲染之前都变成了字符串，因此恶意代码无法注入，从而有效防止了 XSS 攻击。**

```js
// 一段恶意代码
<img src="empty.png" onerror ="alert('xss')"> 
// 转义后输出到 html 中
&lt;img src=&quot;empty.png&quot; onerror =&quot;alert(&#x27;xss&#x27;)&quot;&gt;
```

2. **React 的 JSX 语法， 在经过babel 编译后变成 React.createElement() 调用：**

```js
// JSX
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
// 通过 babel 编译后的代码
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
// React.createElement() 方法返回的 ReactElement
const element = {
  $$typeof: Symbol('react.element'),
  type: 'h1',
  key: null,
  props: {
    children: 'Hello, world!',
      className: 'greeting'   
  }
  ...
}

```

### REACT 中可能引起漏洞的写法：

1. 使用 dangerouslySetInnerHTML

dangerouslySetInnerHTML 是 React 为浏览器 DOM 提供的直接修改 innerHTML 的替换方案。

使用代码直接设置 HTML 会有风险，如果一定要使用，需要手动设置过滤、转义等。

2. 通过用户设置图片的 src 值，或者链接的 href。

```js
const site = "javascript:alert('xss');";
<a href={site}></a>
```

如果没有对 URL 进行 javascript: 和 data: 过滤， 就可能会造成 XSS 攻击。

### 服务器需要做的对 XSS攻 击的处理：

1. 接收浏览器端数据进行过滤处理，移除特殊的 HTML 标签，和 JS 关键字。
2. 返回数据时针对类型，进行转义