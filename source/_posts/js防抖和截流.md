---
title: js防抖和截流
date: 2018-05-08 17:56:08
categories: javascript
tags: 防抖截流
---

在页面操作中，很高频率的对dom的操作会加重浏览器的负担，也会导致一些bug，例如滚动条滚动触发的操作，很快的点击一个按钮，输入字符串搜索等。因此采用防抖和截流进行降低调用频率，但又不影响使用。

### 事件防抖：

拿输入字符串搜索来说，等待用户停止输入，例如过了500ms用户还没有输入，则搜索以输入的字符串，如果用户在等待500ms的时间内又输入了，则在输入后再等待500ms，如果没有继续输入则搜索，如果有就再等待500ms有没有输入。。。。。。

实现防抖的思路：

每次触发事件时都取消之前的延时，出发后发出新的延时

``` js
function debounce(fn, delay) {
  let handle;
  return function(e){
    clearTimeout(handle);
    handle = setTimeout(() => {
      fn(e)
    }, delay)
  }
}
function sayHi(e){
  console.log(e.target.innerWidth, e.target.innerHeight)
}

window.addEventListener('resize', debounce(sayHi, 500))

```    


### 事件节流

事件节流就是 每过500ms进行一次搜索

实现节流的思路：

每次触发事件时都判断当前是否有等待执行的延时函数


``` js
function throttle(fn, delay) {
  let runStatus = false;
  return function(e){
    if(runStatus) {
      return false
    } 
    runStatus = true;
    setTimeout(() => {
      fn(e);
      runStatus = false
    }, delay)
  }
}
function sayHi(e){
  console.log(e.target.innerWidth, e.target.innerHeight)
}

window.addEventListener('resize', debounce(sayHi, 500))

```    
