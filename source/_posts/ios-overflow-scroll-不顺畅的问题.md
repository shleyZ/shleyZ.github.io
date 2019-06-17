---
title: 'ios overflow:scroll 不顺畅的问题'
date: 2019-04-02 17:28:35
categories: javascript
tags: ios卡顿
---

overflow:scroll 在 iOS上滑动会有卡的感觉，不是很顺畅

解决办法：
	
在overflow：scroll处同时添加
``` js
	-webkit-overflow-scrolling: touch;
```
这行代码启用了硬件加速特性，所以滑动很流畅。