---
title: js/css实现持续动画,以及对比
date: 2020-02-03 14:06:23
categories: javascript
tags: js基础
---

## 使用 window.requestAnimationFrame() 

告诉浏览器你希望执行一个动画， 并且要求浏览器在下次重绘之前调用指定的回调函数执行动画。

```js
import { Button } from 'antd';
import React, { useState } from 'react';

const Animate: React.FC<{}> = () => {
  // const ele = document.getElementById('animal');
  const [translateX, settranslateX] = useState(0);
  var start = undefined as any;

  const step = (timestamp: number) => {
    if(start === undefined) {
      start = timestamp;
    }
    const elapsed = timestamp - start;
    settranslateX(Math.min(0.1*elapsed, 200))
    if(elapsed < 2000) {
      window.requestAnimationFrame(step)
    }
  }

  const onBegin = () => {
    window.requestAnimationFrame(step)
  }
  
  return (
    <div style={{width: '100%', textAlign: 'center'}}>
      <div 
        id="animal" 
        style={{
          width: 100,
          height: 100,
          borderRadius: '50%',
          backgroundColor: '#000',
          transform: `translateX(${translateX}px)`
        }} />
      <Button onClick={onBegin}>
        开始
      </Button>
    </div>
  )
};

export default Animate;


```

## 使用css3来实现动画:

```css
.animal{
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background-color: black;
  margin-top: 300px;
  /* position: absolute; */
  animation: horizonMove 5s linear infinite;
  -moz-animation: horizonMove 5s linear infinite;	/* Firefox */
  -webkit-animation: horizonMove 5s linear infinite;	/* Safari 和 Chrome */
  -o-animation: horizonMove 5s linear infinite;
}

@keyframes horizonMove {
  form {
    transform: translateX(0px);
  }
  to {
    transform: translateX(500px);
  }
}

```

```js
<div id="animal" className={style.animal} />
```

## 对比

使用 js 动画：会频繁操作 js，进而频繁对 dom/css 进行操作，浏览器会频繁重绘和重排，这是一个很大的开销，使得 js 的执行效率很低。
使用 css3 进行动画： 不占用主线程， 可以利用硬件加速， 浏览器本身对动画的优化（不可见时不进行动画，减少每秒传输帧数fps）

css3 比较适用于 2D 动画，3D以上的动画的执行效率会比js要低。