---
title: js实现图片懒加载
date: 2020-04-02 13:17:48
categories: javascript
tags: js基础
---

主要原理是判断图片是否在可视区域内部，图片移动到可视区域内部，进行加载。

有三种判断方法：

- 1. offsetTop < clientHeight + scrollTop
- 2. element.getBoundingClientRect().top < clientHeight. 缺点是，由于 scroll 事件密集发生，计算量很大，容易造成性能问题。
- 3. IntersectionObserver, 使用方法：

```js
var io = new IntersectionObserver(callback, option);

```
IntersectionObserver 是浏览器原生提供的构造函数，接受两个参数：callback 是可见性变化时的回调函数，option 是配置对象（该参数可选）。



使用方法三实现懒加载的例子：

```js
const query = (selector) => {
  return Array.from(document.querySelectorAll(selector));
}

const observer = new IntersectionObserver((changes) => {
  changes.forEach((change) => {
    const container = change.target;
    const content = container.querySelector('template').content;
    container.appendChild(content);
    observer.unobserve(container)
  })
})

query('.lazy-load').forEach((item) => {
  observer.observer(item)
})

```

上面代码中，只有目标区域可见时，才会将模板内容插入真实 DOM，从而引发静态资源的加载。


注： 
**clientHeight**  
  表示页面可见区域的高度, 只包括 padding, 不包括 border
**offsetHeight**  
  表示页面可见区域的高度，包括 border
**scrollTop**   
  表示网页被卷去的高，就是滚动条上滚的页面高度
**element.getBoundingClientRect()**  
  表示获取某个元素 element 相对于视窗的位置集合。
**element.getBoundingClientRect().top**  
  表示获取某个元素 element 相对于视窗的顶部的距离。
**window.screen.height**    
  表示屏幕分辨率高