---
title: CSS之盒子模型
date: 2017-07-10 14:22:32
categories: CSS
---
#### 1.盒子的内部结构：  

在一个文档中，每个元素都被表示为一个矩形的盒子。确定这些盒子的尺寸, 属性像它的颜色，背景，边框方面和位置是渲染引擎的目标。 

模型是对某种事物本质特性的抽象。  

一个独立的盒子模型由：content（内容），padding（内边距），border（边框）和margin（外边距）组成。  

一个盒子的宽度由“content + 2 x（padding+border+margin）”组成。  

#### 2.内容区域content

包含元素的真实内容，它通常包含背景，颜色和图片等，它的宽度为content-box的宽度。

如果 box-sizing 为默认值， width, min-width, max-width, height, min-height 与 max-height 控制内容大小。

#### 3.内边距区域padding

作用域content的背景、颜色等会延伸到padding上。即元素的背景颜色等属性的作用范围是content+padding。

他的宽是padding-box的宽度

#### 4.边框border

包含了边框的区域，他的宽是border-box的宽度

#### 5 外边距margin

用空白区域扩展元素，以分开相邻的元素。

它的大小为  margin-box 的高宽。

#### 6.外边距塌陷

块的顶部外边距和底部外边距有时被组合(折叠)为单个外边距，其大小是组合到其中的最大外边距，这种行为称为外边距塌陷(margin collapsing)，有的地方翻译为外边距合并。

发生外边距塌陷的三种情况

1.相邻的兄弟元素

``` html

	<p style="margin-bottom: 30px;">这个段落的下外边距被合并...</p>
	<p style="margin-top: 20px;">...这个段落的上外边距被合并。</p>

```

2.块级元素与它的第一个或最后一个子元素（前提是父元素与子元素中间没有border、padding、inline content、height、min-height、 max-height来分隔时）
3.一个空的块级元素，其 border、padding、inline content、height、min-height 都不存在。那么此时它的上下边距中间将没有任何阻隔，此时它的上下外边距将会合并。

``` html

	<p style="margin-bottom: 0px;">这个段落的和下面段落的距离将为20px</p>
	<div style="margin-top: 20px; margin-bottom: 20px;"></div>
	<p style="margin-top: 0px;">这个段落的和上面段落的距离将为20px</p>

```

当有负边距存在时，合并后的外边距将是最大正边距加上最小负边距（即负边距中绝对值最大的一个）。


#### 7.box-sizing用于更改用于计算元素宽度和高度的默认CSS盒子模型。

box-sizing默认值：content-box。

默认是当设置元素的宽度width为100px时，实际是元素的content宽度是100px。

box-sizing设置为border-box时：

当设置元素的宽度width为100px时，其实是congtent+padding+border的宽度是100px。

box-sizing设置为padding-box时：(只有Firefox实现了这个值，它在Firefox 50中被删除。)

当设置元素的宽度width为100px时，其实是congtent+padding的宽度是100px。

因此box-sizing只有默认值content-box和border-box

实现兼容各个浏览器：

``` css

	Element {
	     -moz-box-sizing: content-box;  
	     -webkit-box-sizing: content-box; 
	     -o-box-sizing: content-box; 
	     -ms-box-sizing: content-box; 
	     box-sizing: content-box; 
	  }
	        
	  
	  Element {
	     -moz-box-sizing: border-box;  
	     -webkit-box-sizing: border-box; 
	     -o-box-sizing: border-box; 
	     -ms-box-sizing: border-box; 
	     box-sizing: border-box; 
	  }

```

#### margin越界

当父元素没有边框border时，设置第一个子元素的margin-top值的时候，会出现margin-top值加在父元素上的现象，解决方法有四个：

（1）给父元素加边框border （副作用）

（2）给父元素设置padding值  （副作用）

（3）父元素添加 overflow：hidden （副作用）

（4）父元素加前置内容生成。（推荐）

第四种方法的实现：

``` css

	.parent{
	    width: 300px;
	    height: 300px;
	    background-color: cadetblue;
	}
	.parent:before {
	    content : "";
	    display : table;
	}
	.child{
	    width: 50px;
	    height: 50px;
	    margin: 20px;
	    background-color: #9ea040;
	}

```

#### 绘制三角形

``` css
	.child{
	    width: 0;
	    height: 0;
	    border: 100px solid transparent;
	    border-right: 100px solid yellow;
	}

```

#### 子元素浮动后父元素高度为0（父元素不设置高度）

``` css

	.parent{
	    width: 300px;
	    background-color: cadetblue;
	}
	.child{
	    width: 50px;
	    height: 50px;
	    float:left;
	    background-color: #4932ff;
	}

```

css如上,这时parent高度为0，不显示。

要解决这个问题

（1）overflow：hidden，但是这样有副作用。	
（2）为父元素设置后置内容生成：

``` css

	.parent{
	    width: 300px;
	    background-color: cadetblue;
	}
	.parent:after{
	    content:' ';
	    display: table;
	    clear: both;
	}
	.child{
	    width: 50px;
	    height: 50px;
	    float:left;
	    background-color: #4932ff;
	}

```



