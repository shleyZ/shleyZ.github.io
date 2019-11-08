---
title: 将callback转换为Promise
date: 2019-11-08 20:02:02
tags: javascript
---

与回调函数相比，使用 Promises（或 Async / await）更容易。 在基于 Node 的环境中工作时尤其如此。 不幸的是，大多数 Node API 都是使用回调编写的。

下面，我们来看看如何将回调转换成 Promise 。

在这之前我们需要详细了解 [Promise]()

## 将 Node 样式的回调转换为 Promise

Node API 的回调具有相同的模式。 它们作为最后一个参数传递给函数。

这是 fs.readFile 的示例。

```js
const fs = require('fs');
fs.readFile(filePath, options, callback);
```

另外，每个回调至少包含两个参数。 第一个参数必须是一个错误对象 err。

```js
fs.readFile('some-file', (err, data) => {
  if (err) {
    // Handle Error
  } else {
    // Handle Data
  }
});
```

如果遇到这种模式的回调，则可以使用 Node 的 util.promisify 将其转换为 Promise。

```js
const fs = require('fs');
const util = require('util');

const readFilePromise = util.promisify(fs.readFile);
```

将回调转换为 Promise 后，便可以像使用其他 Promise 一样使用它:

```js
readFilePromise(filePath, options)
  .then(data => {
    // Handle Data
  })
  .catch(err => {
    // Handle Error
  });
```

有时，您可能会遇到不符合 Node 的错误优先回调格式的 API。 对于这些情况，您不能使用 util.promisify。 您需要写下自己的 Promise。

### 来写自己的 Promise:

要将回调转换为 Promise，需要返回 Promise。

```js
const readFilePromise = () => {
  return new Promise((resolve, reject) => {
    // ...
  });
};
```

你可以在 promise 中使用回调运行代码。

```js
const readFilePromise = () => {
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, options, (err, data) => {
      // ...
    });
  });
};
```

如果有错误，reject。 这使用户可以处理 catch 中的错误。

如果没有错误，resolve。 这使用户可以决定接下来要做什么。

```js
const readFilePromise = () => {
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, options, (err, data) => {
      if (err) {
        return reject(err);
      }
      resolve(data);
    });
  });
};
```

接下来，您需要为 promise 中的代码提供诸如 filePath 之类的参数和选项。

为此，您可以使用 rest 和 spread 运算符。

```js
const readFilePromise = (...args) => {
  return new Promise((resolve, reject) => {
    fs.readFile(...args, (err, data) => {
      if (err) {
        return reject(err);
      }
      resolve(data);
    });
  });
};
```

然后，你就可以将 readFilePromise 用作 Promise。

```js
readFilePromise(filePath, options)
  .then(data => {})
  .catch(err => {});
```

## 将非 Node 样式的回调转换为 Promise

一旦你知道如何构造 Promise，就可以轻松地将非 Node 样式的回调转换为 Promise。 它们遵循相同的步骤：

如果有错误则 reject
否则 resolve

假设您有一个 API，该 API 返回数据作为第一个参数，而 err 作为第二个参数。 我们是这样做的：

```js
const shootPeasPromise = (...args) => {
  return new Promise((resolve, reject) => {
    // 这不是一个 Node 样式的回调（），这里回调的第一个参数不是 err
    shootPeas(...args, (data, err) => {
      if (err) {
        reject(err);
      }
      resolve(data);
    });
  });
};
```

## 多个参数的回调

假设你有一个包含三个参数的回调：

错误对象 err
一些数据 location
另一条数据 size

```js
grewTrees(oprions, (err, location, size) => {});
```

我们不能这样写：

```js
// 这样做是不对的, 因为 Promise 只返回一个参数
const growTreesPromise = (...args) => {
  return new Promise((resolve, reject) => {
    growTrees(...args, (err, location, size) => {
      if (err) {
        reject(err);
      }
      resolve(location, size);
    });
  });
};
```

如果想要 Promise 返回多个参数，可以返回一个数组或对象

```js
// 返回数组
resolve([location, size]);

// 返回对象
resolve({ location, size });
```

然后，您可以在 then 调用中解构数组或对象。

```js
growTreesPromise(options)
  .then(([location, size]) => {
    // 解构处理数组
  })
  .catch(err => {});

growTreesPromise(options)
  .then(({ location, size }) => {
    // 解构处理对象
  })
  .catch(err => {});
```
