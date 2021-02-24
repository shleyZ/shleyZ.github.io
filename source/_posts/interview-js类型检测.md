---
title: js类型检测
date: 2020-02-02 17:23:20
categories: javascript
tags: js基础
---

## typeof 用来判断大部分基础数据类型和Function：

  - Number 
  - String 
  - Boolean 
  - Null     // 使用 typeof 返回的是 "object", 并不能准确分辨
  - Undefined 
  - Symbol (本质上是一种唯一标识符) 

但是对于下面的部分引用类型： 

  - Array
  - Object
  - Function // 使用 typeof 返回的是 "function"， 可以分辨
  - Date
  - RegExp

typeof 返回都是 "object", 无法直接得到类型。。

```js
typeof 123        // "number"
typeof '123'      // "string"
typeof false      // "boolean"
typeof undefined  // "undefined"
typeof Symbol()   // "symbol"
typeof () => {}   // "function"
typeof null       // "object"
typeof []         // "object"
typeof {}         // "object"
typeof /w/g       // "object"
typeof new Date() // "object"
```

因此对于引用类型的数据判断类型的话，可以用 **instanceof**

## instanceof 可以来检测主要的引用类型,即判断一个对象是否在另一个对象的原型链上。

```js
let obj = {}
obj instanceof Object  // true
obj = []
obj instanceof Array  // true
obj = new Date()
obj instanceof Date // true
obj = /w/g
obj instanceof RegExp // true
obj = function() {}
obj instanceof Function // true

```

但是下面这种也都成立：
```js
let obj = {}
obj instanceof Object  // true
obj = []
obj instanceof Object  // true
obj = new Date()
obj instanceof Object // true
obj = /w/g
obj instanceof Object // true
obj = function() {}
obj instanceof Object // true
```

因为所有引用类型其实都是继承自 Object 对象的

下面这个例子：
```js
let str = new String('123')
typeof str  // "object"
obj = []
obj instanceof Object  // true
obj instanceof String  // true
```

这就尴尬了，这到底是什么类型？

## Object.prototype.toString.call() 

直接看例子：

```js
let obj = {}
Object.prototype.toString.call(obj)  // "[object Object]"
obj = []
Object.prototype.toString.call(obj)  // "[object Array]"
obj = new Date()
Object.prototype.toString.call(obj)  // "[object Date]"
obj = /w/g
Object.prototype.toString.call(obj)  // "[object RegExp]"
obj = () => {}
Object.prototype.toString.call(obj)  //"[object Function]"
obj = null
Object.prototype.toString.call(obj)  //"[object Null]"
obj = undefined
Object.prototype.toString.call(obj)  //"[object Undefined]"
obj = 123
Object.prototype.toString.call(obj)  // "[object Number]"
obj = 'abc'
Object.prototype.toString.call(obj)  // "[object String]"
obj = false
Object.prototype.toString.call(obj)  // "[object Boolean]"
obj = Symbol()
Object.prototype.toString.call(obj)  // "[object Symbol]"
```