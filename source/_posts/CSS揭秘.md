---
title: CSS揭秘
date: 2017-09-25 12:07:53
categories: CSS
tags: CSS
---

#### 1.background-clip:padding-box

background-clip默认为border-box，也就是说border的颜色会把元素背景颜色覆盖掉。
如果设置background-clip:padding-box，背景颜色的范围就是在padding和containt。

#### 2.box-shadow可以进行多重投影

	button{
		background: yellowgreen; 
		box-shadow: 0 0 0 10px #655,
            		0 0 0 15px deeppink,
            		0 2px 5px 15px rgba(0,0,0,.6);
    }

box-shadow语法：
	box-shadow: h-shadow v-shadow blur spread color inset;    

h-shadow水平阴影的位置 
v-shadow垂直阴影的位置
blur模糊距离
spread阴影尺寸
color阴影颜色
inset将外部阴影变为内部阴影

#### 3.使用border+outline实现两层边框

	button{
		background: yellowgreen; 
		border: 10px solid #655; 
		outline: 5px solid deeppink;
    }

不过当outline的内部border设置圆角时，outline边框就不能与border相贴合了。

#### 4.实现边框内圆角

用两个元素实现：

	<div class="example">
        <div class="ex1"></div>
    </div>
    <style>
        .example{
            padding: 0.5em;
            background: #655;
        }
        .ex1{
            background: tan;
            border-radius: .8em;
            padding: 1em;
        }
    </style>    

用一个元素实现：
	
	.ex1{
            background: tan;
            border-radius: .8em;
            padding: 1em;
            box-shadow: 0 0 0 .4em #655;
            outline: .6em solid #655;
        }
原理就是outline和border圆角之间的空白用box-shadow填补上。

#### 5.条纹背景

