---
title: nodejs模块crypto
date: 2019-01-20 13:25:51
categories: nodejs
tags: nodejs
---

## crypto 提供通用的加密和哈希算法(用 C/C++实现的算法)

### 哈希算法：MD5 和 SHA1

用于给任意数据一个“签名”

```js
"use strict";

var crypto = require("crypto");

// md5加密--32 个字符的字符串
const hash = crypto.createHash("md5");
// sha1加密--40 个字符的字符串
// const hash = crypto.createHash("sha1");
// sha256加密--64 个字符的字符串
// const hash = crypto.createHash("sha256");
// sha512加密--128 个字符的字符串
// const hash = crypto.createHash("sha512");

hash.update("hello world");
// hash.update("hello nodejs");

console.log(hash.digest("hex"));
```

### 随机数“增强”的哈希算法：Hmac

比哈希算法多了一个密钥字符串，这个字符串是自己定义的

```js
"use strict";

var crypto = require("crypto");

// md5加密--32 个字符的字符串
const hmac = crypto.createHash("md5", "nodejs");
// sha1加密--40 个字符的字符串
// const hmac = crypto.createHash("sha1", "nodejs");
// sha256加密--64 个字符的字符串
// const hmac = crypto.createHash("sha256", "nodejs");
// sha512加密--128 个字符的字符串
// const hmac = crypto.createHash("sha512", "nodejs");

hmac.update("hello world");
// hash.update("hello nodejs");

console.log(hmac.digest("hex"));
```

### 对称加密算法：AES

加解密都用同一个密钥，crypto 模块提供了 AES 支持，但是需要自己封装好函数，便于使用

```js
"use strict";

var crypto = require("crypto");

function aesEncrpt(data, key) {
  const cipher = crypto.createCipher("aes192", key);
  var crypted = cipher.update(data, "utf8", "hex");
  crypted += cipher.final("hex");
  return crypted;
}

function aesDecrypt(encrypted, key) {
  const decipher = crypto.createDecipher("aes192", key);
  var decrypted = decipher.update(encrypted, "hex", "utf8");
  decrypted += decipher.final("utf8");
  return decrypted;
}

var data = "hello, this is a secrete message";
var key = "Password!";
var encryptd = aesEncrpt(data, key);
var decryptd = aesDecrypt(encryptd, key);

console.log("Plain text: " + data);
console.log("encryptd text: " + encryptd);
console.log("decryptd text: " + decryptd);

// 执行结果：
// Plain text: hello, this is a secrete message
// encryptd text: 1d19941e8612ca5d3b08c1d47db31ce7669c40e22c345804b3034ed233d91d73dd5221be4d3d37ebbcc8d18416c340cd
// decryptd text: hello, this is a secrete message
```

### Diffie-Hellman, DH 算法是一种密钥交换协议.它可以让双方在不泄漏密钥的情况下协商出一个密钥来。

### RSA 非对称加密算法

由一个私钥和一个公钥构成的密钥对，通过私钥加密，公钥解密，或者通过公钥加密，私钥解密。其中，公钥可以公开，私钥必须保密。
