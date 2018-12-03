---
title: react性能优化
date: 2018-12-03 14:33:10
categories: react
---


### shouldComponentUpdate 组件是否需要被更新

shouldComponentUpdate函数是重渲染时render()函数调用前被调用的函数，它接受两个参数：nextProps和nextState，分别表示下一个props和下一个state的值。并且，当函数返回false时候，阻止接下来的render()函数的调用，阻止组件重渲染，而返回true时，组件照常重渲染。


### PureComponent

纯组件，自动有一个shouldComponentUpdate，如果你只是单纯地想要避免state和props不变下的冗余的重渲染，那么react的pureComponent可以非常方便地实现这一点

### 避免在render里面使用箭头函数和bind

    <button onClick={() => this.handleClick()} style={this.state}>
    确定
    </button>

    handleClick(){
      ......
    }

  浏览器每执行一次 =>，就需要创建一个 新的函数对象，这其实是一个比较耗费性能的操作，

  如果你在 render 中使用箭头函数，那么每次调用 render 时都会去创建一个新的函数对象，此时，即使使用 PureComponent 和 shouldComponentUpdate 也起不到优化作用

  所以我们应该避免在 render 中使用箭头函数和绑定。否则会打破 shouldComponentUpdate 和 PureComponent 的性能优化。


  应该这样：


    <button onClick={this.handleClick} style={this.state}>
    确定
    </button>

    handleClick = () => {
      ......
    }

    不使用箭头函数，和bind，改为对handleClick函数的引用


### 尽量避免使用array的索引值作为其key值

react的diff算法是以key来唯一标识组件的，当发现update前和update后key值没有变化，react就会认为update前后组件是同一个，如果以数组索引为key值，可能引发删除一个数据时，导致渲染出错。

应该将key设置为一个特殊字段，保证其唯一性