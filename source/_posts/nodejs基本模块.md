---
title: nodejs基本模块
date: 2019-01-13 09:36:56
categories: nodejs
tags: nodejs
---

nodejs 中少数几个同步 I/O 操作：require

## 基本模块

### 1.global: 在 Node.js 环境中，唯一的全局对象

### 2.process: 也是 Node.js 提供的一个对象，它代表当前 Node.js 进程。通过 process 对象可以拿到许多有用信息：

```bash
> process === global.process;
true
> process.version;
'v8.11.4'
> process.platform;
'darwin'
> process.arch;
'x64'
> process.cwd(); //返回当前工作目录
'/Users/michael'
> process.chdir('/private/tmp'); // 切换当前工作目录
undefined
> process.cwd();
'/private/tmp'
```

JavaScript 和 Node.js 是由事件驱动执行的单线程模型，Node.js 不断执行响应事件的 JavaScript 函数，直到没有任何响应事件的函数可以执行时，Node.js 就退出了。

#### process.nextTick()

如果想要在下一次事件响应中执行代码，可以调用 process.nextTick()：

```js
process.nextTick(() => {
  console.log("nextTick callback!");
});

console.log("nextTick was set!");
```

输出是：

```js
nextTick was set!
nextTick callback!
```

这说明传入 process.nextTick()的函数不是立刻执行，而是要等到下一次事件循环。

#### process 的 exit 事件

在程序即将退出时执行某个回调函数

```js
process.on("exit", code => {
  console.log("about to exit with code: " + code);
});
```

## 常用内置模块

### fs 文件系统模块,负责读写文件

和所有其它 JavaScript 模块不同的是，fs 模块同时提供了异步和同步的方法。

#### 异步读取文件:

```js
fs.readFile("base_module.js", "utf-8", (err, data) => {
  if (err) {
    console.log(err);
  } else {
    console.log(data);
  }
});
fs.readFile("1.jpeg", (err, data) => {
  if (err) {
    console.log(err);
  } else {
    console.log(data);
    console.log(data.length + " bytes");
  }
});
```

```js
// Buffer -> String
var text = data.toString("utf-8");
console.log(text);
```

```js
// String -> Buffer
var buf = Buffer.from(text, "utf-8");
console.log(buf);
```

#### 同步读文件

```js
const data = fs.readFileSync("1.jpeg");
const data1 = fs.readFileSync("base_module.js", "utf-8");
console.log(data); // 读取图片
console.log(data1); // 读取js文件
```

如果同步读取文件发生错误，需要用 try...catch 来捕获

```js
try {
  const data = fs.readFileSync("1.jpeg");
  const data1 = fs.readFileSync("base_module.js", "utf-8");
  console.log(data);
  console.log(data1);
} catch (err) {
  console.log(err);
}
```

#### 写文件 fs.writeFile()

```js
"use strict";

var fs = require("fs");
var data = "console.log('aaa')";
// 异步写入
fs.writeFile("writeInFile.js", data, err => {
  console.log(err);
});
// 同步写入
fs.writeFileSync("writeInFileAsync.js", data);
```

#### stat 获取文件大小，创建时间等信息

```js
"use strict";

var fs = require("fs");
fs.stat("base_module.js", (err, stat) => {
  if (err) {
    console.log(err);
  } else {
    console.log("isFile:" + stat.isFile());
    console.log("isDir:" + stat.isDirectory());
    if (stat.isFile) {
      console.log("File size:" + stat.size);
      console.log("create time:" + stat.birthtime);
      console.log("modify time:" + stat.mtime);
    }
  }
});

// isFile:true
// isDir:false
// File size:465
// create time:Thu Jun 20 2019 09:52:21 GMT+0800 (CST)
// modify time:Thu Jun 20 2019 10:13:35 GMT+0800 (CST)
```

### stream 支持“流”数据结, 仅在服务端可用的模块

流的特点是数据是有序的，而且必须依次读取，或者依次写入，不能像 Array 那样随机定位。

在 Node.js 中，流也是一个对象，我们只需要响应流的事件就可以了：data 事件表示流的数据已经可以读取了，end 事件表示这个流已经到末尾了，没有数据可以读取了，error 事件表示出错了。

#### 从文件流读取文本内容：

```js
"use strict";

var fs = require("fs");
var rs = fs.createReadStream("aaa.text", "utf-8");

rs.on("data", chunk => {
  console.log("DATA:");
  console.log(chunk);
});

rs.on("end", chunk => {
  console.log("END");
});

rs.on("error", err => {
  console.log("ERROR:" + err);
});
```

#### 以流的形式写入文件:

```js
"use strict";

var fs = require("fs");
var ws = fs.createWriteStream("in.text", "utf-8");

ws.write("增加段落1\n");
ws.write("增加段落2\n");
ws.write("增加段落3\n");
ws.end();

var ws1 = fs.createWriteStream("in1.text", "utf-8");

ws1.write(new Buffer("使用Stream写入二进制数据...\n"), "utf-8");
ws1.write(new Buffer("END \n"), "utf-8");
ws1.end();
```

#### pipe 两个流串联起来

一个 Readable 流和一个 Writable 流串起来后，所有的数据自动从 Readable 流进入 Writable 流，这种操作叫 pipe。

在 Node.js 中，Readable 流有一个 pipe()方法，就是用来干这件事的。

```js
"use strict";

var fs = require("fs");
var rs = fs.createReadStream("in.text");
var ws = fs.createWriteStream("copied.text");

rs.pipe(ws);
```

### http

要开发 HTTP 服务器程序，直接操作 http 模块提供的 request 和 response 对象。

request 对象封装了 HTTP 请求，我们调用 request 对象的属性和方法就可以拿到所有 HTTP 请求的信息；

response 对象封装了 HTTP 响应，我们操作 response 对象的方法，就可以把 HTTP 响应返回给浏览器。

#### 一个简单的 HTTP 服务器：

```js
"use strict";

var http = require("http");
const server = http.createServer((req, res) => {
  console.log(req.method + ":" + req.url);
  res.writeHead(200, { "Content-type": "text/html" });
  res.end("<h1>hell nodejs</h1>");
});

server.listen(9090);
console.log("server is runnig at http://127.0.0.1:9090/");
```

#### 一个简单的 文件 服务器：

```js
"use strict";

var fs = require("fs");
var url = require("url");
var path = require("path");
var http = require("http");

// 从命令行参数获取root目录，默认是当前目录:
var root = path.resolve(process.argv[2] || ".");
console.log("Static root dir: " + root);

var server = http.createServer((req, res) => {
  // 获得URL的path，类似 '/css/bootstrap.css':
  var pathname = url.parse(req.url).pathname;

  // 获得对应的本地文件路径，类似 '/srv/www/css/bootstrap.css':
  var filePath = path.join(root, pathname);

  // 获取文件状态:
  fs.stat(filePath, (err, stat) => {
    if (!err && stat.isFile()) {
      // 没有出错并且文件存在:
      console.log("200" + req.url);
      // 发送200响应:
      res.writeHead(200);
      // 将文件流导向response:
      fs.createReadStream(filePath).pipe(res);
    } else {
      // 出错了或者文件不存在:
      console.log("404" + req.url);
      // 发送404响应:
      res.writeHead(404);
      res.end("404 not found");
    }
  });
});

server.listen(9090);
console.log("server is running at http://127.0.0.1:9090/");
```
