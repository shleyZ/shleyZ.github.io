---
title: '@bigtest/convergence'
date: 2020-01-8 11:21:48
categories: bigtest
tags: bigtest
---

[Convergence](https://bigtestjs.io/docs/convergence/) 是强大,不可变，可重用和可组合的断言，使您可以立即知道何时达到期望的状态。
简而言之：它每 10 毫秒检查一次 DOM（默认情况下为 2 秒），以查看您要检查的状态是否存在。

Convergence 是 BigTest 中所有事物的基础。这就是使 interactors 可靠的原因。
CLI 中甚至将 Convergence 用于等待浏览器连接状态。

```js
import Convergence from '@bigtest/convergence';

setTimeout(() => (foo = 'bar'), 100);
await new Convergence().when(() => foo === 'bar');
console.log(foo); // => "bar"
```

默认情况下，Convergence 将在 2000ms 之前或之后 converge，具体取决于定义的断言类型。这可以通过在初始化 convergence 时提供超时或使用 #timeout() 方法来配置。

```js
new Convergence(100);
new Convergence().timeout(5000);
```

使用 #when()，断言将运行多次直到通过。同样，#always() 确保断言在一段时间内保持通过状态。

```js
// converges when `foo` is equal to `'bar'` within 100ms
new Convergence(100).when(() => foo === 'bar');
// converges after `foo` is equal to `'bar'` for at least 100ms
new Convergence(100).always(() => foo === 'bar');
```

convergence 是不可变的，因此，它的方法将返回新实例。 这使您可以组合多个 convergence，并使用各自的 #run() 方法分别启动每个 convergence。

```js
let converge = new Convergence(300);
let convergeFoo = converge.when(() => foo === 'foo');
let convergeFooBar = convergeFoo.when(() => foo === 'bar');
let convergeFooBarBaz = convergeFooBar.when(() => foo === 'baz');

setTimeout(() => (foo = 'foo'), 100);
setTimeout(() => (foo = 'bar'), 200);
setTimeout(() => (foo = 'baz'), 150);

// resolves after 100ms
convergeFoo.run();
// resolves after 200ms
convergeFooBar.run();
// rejects after 300ms since it wasn't `baz` _after_ `bar`
convergeFooBarBaz.run();
```

Convergences 会立即调用#run（）。这使它们能够在大多数情况下可以使用的地方使用。

```js
async function onceBarAlwaysBar() {
  await new Convergence().when(() => foo === 'bar').always(() => foo === 'bar');
}

Promise.race([onceBarAlwaysBar(), new Convergence().when(() => foo === 'baz')]);
```

## Methods

### isConvergence(obj) 返回布尔值

如果对象具有正确类型的 convergence 属性，则返回 true。

```js
let result = maybeConvergence();

if (isConvergence(result)) {
  await result.do(something).timeout(100);
} else {
  something(result);
}
```

### always(assertion, timeout)

传入的参数：

- assertion： Function 需要 converge 的断言
- timeout：Number 使用的超时时间，以剩余的超时时间为上限。

返回一个新的 convergence

返回带有附加断言的新 convergence 实例。重复运行此断言以确保它在整个超时时间内通过。如果在超时期间的任何时候断言失败，那么 convergence 将失败。

```js
// would converge after `foo` remains `'foo'` for at least 100ms
new Convergence(100).always(() => foo === 'foo');
```

当 convergence 结束时遇到 always 断言时，超时默认为当前正在运行的实例的剩余时间。最少 20ms。如果不在 convergence 范围内，则默认为总超时时间的十分之一。

```js
let convergeFooThenBar = new Convergence(1000)
  .always(() => foo === 'foo') // would continue after `foo` remains `'foo'` for at least 100ms
  .when(() => foo === 'bar'); // then have any time remaining to converge on `foo` being `'bar'`
```

给定一个超时时间，则将其限制为当前运行实例的剩余超时时间。

```js
let convergeFooThenBar = new Convergence(100)
  .always(() => foo === 'foo', 50) // 当在50ms内foo持续等于'foo'，则继续
  .when(() => foo === 'bar') // 然后还剩下50ms
  .always(() => foo === 'bar', 100);
```
