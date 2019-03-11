---
title: 微信公众号iOS时间显示NAN不兼容问题
date: 2019-03-11 13:29:46
categories: javascript
---

因为在IOS浏览器下，new Date("2019-03-10 23:59:59")会返回invalid data,所以会造成时间显示为

  NAN-NAN-NAN NAN:NAN:NAN

只要避免这种情况就行，在代码中可以使用正则替换

  new Date("2019/03/10 23:59:59")

来解决这种问题  

