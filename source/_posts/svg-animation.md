---
title: svg animation
date: 2018-09-04 06:11:12
categories: css
---

## 基础知识：

### stroke

用于绘制形状的轮廓颜色的表示属性


作为表示属性，它可以应用于任何元素，但它仅对以下十一个元素产生影响：

    <altGlyph>, <circle>, <ellipse>, <path>, <polygon>, <polyline>, <rect>, <text>, <textPath>, <tref>, <tspan>


### strokeWidth

定义要应用于形状的笔划宽度的表示属性，默认1px

作为表示属性，它可以应用于任何元素，但它仅对以下十一个元素产生影响：

    <altGlyph>, <circle>, <ellipse>, <path>, <polygon>, <polyline>, <rect>, <text>, <textPath>, <tref>, <tspan>

### fill

fill属性有两个不同的含义：对于形状和文本，它是一个表示属性，它允许定义用于绘制元素的颜色；对于动画，它允许定义动画的最终状态。

作为表示属性，它可以应用于任何元素，但它仅对以下十一个元素产生影响：

    <altGlyph>, <circle>, <ellipse>, <path>, <polygon>, <polyline>, <rect>, <text>, <textPath>, <tref>, <tspan>

例如：

``` html
<svg viewBox="0 0 400 100" xmlns="http://www.w3.org/2000/svg">
  <circle cx="150" cy="50" r="40" fill="url(#myGradient)" />
</svg> 
```

对于动画，五个元素使用此属性:

    <animate>, <animateColor>, <animateMotion>, <animateTransform>, <set>

例如：

``` html
<svg viewBox="0 0 400 100" xmlns="http://www.w3.org/2000/svg">
  <circle cx="250" cy="50" r="20">
    <animate 
      attributeType="XML"
      attributeName="r"
      from="0" to="40" dur="5s"
      fill="freeze" />
  </circle> 
</svg>   
```

作为表示属性，fill可以用作CSS属性。


### fill-opacity

定义应用于形状的不透明度的表示属性。

例如：

``` html
<svg viewBox="0 0 400 100" xmlns="http://www.w3.org/2000/svg">
  <circle 
    cx="250" 
    cy="50" 
    r="40"
    fill-opacity="50%" />
</svg>  
```

### stroke-dasharray

stroke-dasharray属性用来设置实线和虚线的宽度.如果提供奇数个，则会自动复制该值成偶数

比如： 

stroke-dasharray: 50 20;

50和20分别对应了实线和虚线的长度


### stroke-dashoffset

stroke-dashoffset：定义虚线描边的偏移量（在路径开始的前面，看不到）


## 动画

svg 描边动画能使用css3 animation 实现，无需任何JavaScript

``` html
<svg>
  <path
    d="......"
    stroke="#000"
    stroke-width=2
    fill="#fff"
    fill-opacity=0
  />
</svg>
```

CSS:

``` css
path {
  stroke-dasharray: 1000;
  stroke-dashoffset: 1000;
  animation: dash 5s linear infinite;
}

@keyframes dash {
  to {
    stroke-dashoffset: 0;
  }
}
```

上面的1000应该是大于等于路径的长度


### 路径长度的计算

``` js
var path = document.querySelector('path');
var length = path.getTotalLength();
```    