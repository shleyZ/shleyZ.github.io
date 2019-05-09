---
title: 布局方式static/relative/absolute/fixed
date: 2018-05-09 09:01:35
categories: css
---

css中position的布局方式有：static | relative | absolute | fixed


1. static: 默认值，制定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时top | bottom | left | right | z-index 属性都不生效

2. relative: 根据自身的位置来进行定位， 不脱离文档流

3. absolute: 脱离文档流，根据上级中最近的非static元素来定位

4. fixed: 脱离文档流，相对于屏幕视口进行定位
