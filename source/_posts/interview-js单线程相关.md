---
title: js单线程和事件循环
date: 2020-04-02 14:06:14
categories: javascript
tags: js基础
---

## js单线程

javascript 作为一个浏览器端语言，最主要任务是处理用户的交互，而用户的交互主要就是对 dom 的增删改查。如果设计成多线程，一个线程在某个DOM节点上面新增内容，另一个线程删除了这个节点。那浏览器要怎么显示，以哪个为准。

单线程意味着，所有的任务都要排队，在一个队列里面。前一个任务结束，后一个任务才能开始。如果前一个任务耗时很长，后面的任务就得一直等待，例如一个接口请求等待读取数据时。


## 事件循环

后面的任务完全可以在前一个任务没有完成前执行，也就是把等待中的任务暂时挂起，先运行排在后面的任务，等挂起的任务完成或有了返回，再继续执行挂起的任务。

因此有了两种任务：

- 同步任务： 立即执行的任务，同步任务一般会直接进入到主线程中执行。
- 异步任务： 例如 Ajax回调函数、DOM 事件回调函数、setTimeout/setInterval回调函数等, 通过任务队列来进行协调。


同步任务和异步任务分别进入不同的执行环境:
  - 同步任务进入主线程，也就是主执行栈。
  - 异步任务进入 Event Quene。

主执行栈内的任务执行完毕，会去 Event Quene 读取对应的任务，推入主程执行。上面的过程重复执行就是 Event Loop **事件循环**。

在事件循环中，每进行一次循环成为一次 tick。每一次 tick 的任务处理模型都是很复杂的：

- 在此次 tick 中选择最先进入队列的任务（oldest）。如果有则执行。
- 检查是否存在 microtasks（微任务），如果存在则不停的执行，直至清空 microtasks 队列。
- 更新 Render
- 主线程重复执行上述步骤

宏任务：setTimeout、setInterval, Ajax，DOM事件
微任务：Promise async/await

## 例子1 简单：

```js
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');
```

分析上面代码：

1. 整体 script 作为第一个宏任务进入主线程，遇到 console.log('script start'), 输出 script start；
2. 遇到 setTimeout， 其回调函数被分发到宏任务的 Event Quene中；
3. 遇到 Promise, 其 then 函数被分到微任务 Event Quene 中，记为 then1, 之后又遇到了 then 函数，将其分到微任务 Event Quene 中，记为 then2；
4. 遇到 console.log('script end'), 输出 script end；

现在 Event Quene 中存在三个任务：

| 宏任务      | 微任务   | 
| --------   | -----:  | 
| setTimeout | then1   | 
| ---        | then2   | 
然后：

5. 执行微任务， 首先执行 then1, 输出 promise1， 然后执行 then2, 输出 promise2；此时清空了所有的微任务；
6. 执行 setTimeout，输出 setTimeout。

所以执行完输出顺序：

```js
// script start
// script end
// promise1
// promise2
// setTimeout
```

## 例子2 复杂：

```js
console.log('script start');

setTimeout(function() {
  console.log('timeout1');
}, 10);

new Promise(resolve => {
    console.log('promise1');
    resolve();
    setTimeout(() => console.log('timeout2'), 10);
}).then(function() {
    console.log('then1')
})

console.log('script end');
```

分析：

1. 整体 script 作为第一个宏任务进入主线程，遇到 console.log('script start'), 输出 script start；
2. 遇到 setTimeout， 其回调函数被分发到宏任务的 Event Quene中， 记为 setTimeout1；
3. 遇到 Promise，new Promise 中代码立即执行， 输出 promise1 
4. 将 Promise 的 then 放到微任务中去，记为 then1
5. 遇到 console.log('script end')， 输出 script end

现在 Event Quene 中存在三个任务：

| 宏任务      | 微任务   | 
| --------   | -----:  | 
| setTimeout1| then1   | 
| setTimeout2| -----   | 

6. 依次执行微任务，直至清空微任务，输出： then1
7. 依次执行宏任务，输出： timeout1， timeout2

所以执行完输出顺序：

```js
// script start
// promise1
// script end
// then1
// timeout1
// timeout2
```

## 总结：

微任务优先于宏任务，所以有需要优先执行的逻辑，放入微任务队列会更早的执行。

最后：JS是单线程语言，所有的任务都放在事件循环队列里面，等待主执行栈来执行，并没有专门的异步线程。