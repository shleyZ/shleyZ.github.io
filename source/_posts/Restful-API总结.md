---
title: Restful API总结
date: 2019-07-22 10:54:40
categories: RestfulAPI
tags: RestfulAPI
---

Restful 是目前最流行的 API 设计规范， 用于 web 接口设计。

## 1 URL 设计

Restful 的核心就是客户端发出的数据操作指令都是 "动词 + 宾语" 的形式

### 1.1 动词通常是五种 http 方法，对应 curd 操作

- GET：读取（Read）

- POST：新建（Create）

- PUT：更新（Update）

- PATCH：更新（Update），通常是部分更新

- DELETE：删除（Delete）

### 1.2 动词的覆盖

有些客户端只能使用 GET 和 POST 这两种方法，服务器必须接受 POST 模拟其他三个方法（PUT、PATCH、DELETE）。

这时，客户端发出的 HTTP 请求，要加上 X-HTTP-Method-Override 属性，告诉服务器应该使用哪一个动词，覆盖 POST 方法。

```js
POST /api/Person/4 HTTP/1.1
X-HTTP-Method-Override: PUT
```

上面代码中，X-HTTP-Method-Override 指定本次请求的方法是 PUT，而不是 POST。

### 1.3 宾语必须是名词

宾语就是 API 的 URL，是 HTTP 动词作用的对象。

### 1.4 复数 URL

建议都使用复数 url

### 1.5 避免多级 url

比如获取某个作者的某一类文章： GET /authors/12/categories/2

这种 URL 不利于扩展，语义也不明确，往往要想一会，才能明白含义。

更好的做法是: 除了第一级，其他级别都用查询字符串表达。

GET /authors/12?categories=2

## 2 状态码，状态码必须精确

HTTP 状态码就是一个三位数，分成五个类别：

- 1xx：相关信息
- 2xx：操作成功
- 3xx：重定向
- 4xx：客户端错误
- 5xx：服务器错误

### 2.1 API 不需要 1xx 状态码

### 2.2 2XX 状态码

GET 200 表示操作成功
POST 201 表示生成了新的资源
DELETE 204 表示资源已经不存在

### 2.3 3XX 状态码，重定向

API 用不到的（浏览器会自动跳转）：

- 301 永久重定向
- 302 暂时重定向， 用于 GET 请求

API 用得到的：

- 303 表示参考另一个 URL， 主要用于 POST、PUT 和 DELETE 请求， 浏览器不会自动跳转

### 2.4 4XX 状态码, 表示客户端错误

- 400 Bad Request: 服务器不理解客户端的请求，未做任何处理.

- 401 Unauthorized: 用户未提供身份验证凭据，或者没有通过身份验证.

- 403 Forbidden: 用户通过了身份验证，但是不具有访问资源所需的权限.

- 404 Not Found: 所请求的资源不存在，或不可用.

- 405 Method Not Allowed: 用户已经通过身份验证，但是所用的 HTTP 方法不在他的权限之内.

等等

### 2.5 5XX 状态码, 表示服务端错误。一般来说，API 不会向用户透露服务器的详细信息

- 500 Internal Server Error：客户端请求有效，服务器处理时发生了意外

- 503 Service Unavailable：服务器无法处理请求，一般用于网站维护状态

## 3. 服务器回应

### 3.1 返回对象应该是 json 对象

### 3.2 发生错误时，不要返回 200 状态码

### 3.3 提供链接
