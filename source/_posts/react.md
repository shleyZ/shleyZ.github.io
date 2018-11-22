---
title: react
date: 2018-09-10 08:54:48
categories: react
---


### 以组件的方式考虑ui的构建


### 单一职责原则

每个组件只做一件事，如果组件变得复杂，拆分成小组件。复杂度拆分，提高性能（局部刷新state,不会影响其他组件）。

### 数据状态管理： DRY原则

  1. 能计算得到的状态，不要进行存储，在用的时候计算
  2. 组件尽量无状态，所有数据从props获取。纯组件，有更好的性能，更容易被重用。


### 组件的生命周期

Render阶段，纯净没有副作用
Pre-commit 阶段，可以读取dom
Commit 阶段，使用Dom，

生命周期的三个类型

创建时 constructor
更新时 render
卸载时 willUnmount

### 渲染机制

#### 渲染过程

在页面一开始打开的时候，React会调用render函数构建一棵Dom树，在state/props发生改变的时候，render函数会被再次调用渲染出另外一棵树，接着，React会用对两棵树进行对比，找到需要更新的地方批量改动。

#### Diff 算法（将算法复杂度从O(n^3)降低到O(n)）

Diff算法只会对同层的节点进行比较，也就是说如果父节点不同，React将不会在去对比子节点。因为不同的组件DOM结构会不相同，所以就没有必要在去对比子节点了。这也提高了对比的效率。

1. 对于不同的节点类型，react会基于第一条假设，直接删去旧的节点，新建一个新的节点。

2. 当对比相同的节点类型比较简单，react会对比它们的属性，只改变需要改变的属性


比如：

        <div className="before" title="stuff" />

	<div className="after" title="stuff" />

这两个div中，react会只更新className的值

	<div style={{color: 'red', fontWeight: 'bold'}} />

	<div style={{color: 'green', fontWeight: 'bold'}} />

这两个div中，react只会去更新color的值

3. 列表比较

	<div>
	  <A />
	  <B />
	</div>
	// 列表一到列表二
	<div>
	  <A />
	  <C />
	  <B />
	</div>

从列表一到列表二，只是在中间插入了一个C，但是如果没有key的时候，react会把B删去，新建一个C放在B的位置，然后重新建一个节点B放在尾部。

当节点很多的时候，这样做是非常低效的，所以我们需要给每个节点配一个key，让react可以识别出来哪些节点是一样的，不需要重新创建。

配上key之后，列表二的生命周期就如我所愿，只在指定的位置创建C节点插入。
这里要注意的一点是，key值必须是稳定（所以我们不能用Math.random()去创建key），可预测，并且唯一的。


### 
