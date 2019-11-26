---
title: 'display:flex和position:absolute/fixed一起用产生的问题'
date: 2019-11-26 13:40:09
tags:
---

在开发中遇到这样的问题

```css
.layout {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
}

.layout .block1 {
  width: 12em;
}

.layout .block2 {
  flex: 1;
}
```

发现 layout 下面的 block2 并不是占用 block1 剩余的空间的，flex 布局没有生效

所以，被绝对定位与固定定位的盒子不参与 flex 布局。

解决办法，可以在要 flex 布局的盒子外面套一层盒子：

```css
.layout {
  position: absolute;
  width: 100%;
  height: 100%;
}

.layout .container {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100%;
}

.layout .container .block1 {
  width: 12em;
}

.layout .container .block2 {
  flex: 1;
}
```

完美解决问题
