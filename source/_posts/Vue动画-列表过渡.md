---
title: Vue动画--列表过渡
date: 2017-09-22 16:48:21
categories: Vue
tags: Vue
---

列表过渡要使用：`<transition-group>`组件:

不同于 `<transition>`，它会以一个真实元素呈现：默认为一个 `<span>`。  
你也可以通过 tag 特性更换为其他元素。  
内部元素 总是需要 提供唯一的 key 属性值

1.列表的进入／离开过渡
	
``` html

	<style>
        .list-item{
            display: inline-block;
            margin-right: 10px;
        }
        .list-enter-active,.list-leave-active{
            transition: all 1s;
        }
        .list-enter,.list-leave-to{
            opacity: 0;
            transform: transilateY(30px);
        }
    </style>
	<div id="list-demo">
        <button v-on:click="add">添加</button>
        <button v-on:click="remove">移除</button>
        <transition-group name="list" tag="p">
            <span v-for="item in items" v-bind:key="item" class="list-item">{{item}}</span>
        </transition-group>
    </div>
    <script>
        var vm = new Vue({
            el:'#list-demo',
            data:{
                items: [1,2,3,4,5,6,7,8,9],
                nextNum: 10,
            },
            methods:{
                randomIndex:function () {
                    return Math.floor(Math.random()*this.items.length);
                },
                add:function () {
                    this.items.splice(this.randomIndex(),0,this.nextNum++);
                },
                remove:function () {
                    this.items.splice(this.randomIndex(),1);
                }
            }
        })
    </script>

```

2.列表的排序过渡

改变定位：使用v-move特性
根据这一特性我们可以实现下面的单列列表过渡：

``` html

	<div id="flip-list-demo" class="demo">
        <button @click="shuffle">列表随机排序</button>
        <button @click="add">添加</button>
        <button @click="remove">移除</button>
        <transition-group name="list" tag="p">
            <span v-for="item in items" v-bind:key="item" class="list-item">{{item}}</span>
        </transition-group>
    </div>
    <script>
        var vm = new Vue({
            el:'#flip-list-demo',
            data:{
                items: [1,2,3,4,5,6,7,8,9],
                nextNum: 10,
            },
            methods:{
                shuffle:function () {
                    this.items = _.shuffle(this.items)
                },
                randomIndex:function () {
                    return Math.floor(Math.random()*this.items.length);
                },
                add:function () {
                    this.items.splice(this.randomIndex(),0,this.nextNum++);
                },
                remove:function () {
                    this.items.splice(this.randomIndex(),1);
                }
            }
        })
    </script>
<a href="http://owpwda5op.bkt.clouddn.com/list.html">查看运行结果</a>

```

同样可以实现多维网格的过渡。

``` html

	<!doctype html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <meta name="viewport"
	          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
	    <meta http-equiv="X-UA-Compatible" content="ie=edge">
	    <title>Vue多维网格动画</title>
	    <script src="../node_modules/vue/dist/vue.js"></script>
	    <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>
	    <style>
	        .demo{
	            width: 300px;
	            margin:100px auto;
	        }
	        button{
	            float:right;
	            width:100px;
	            height:30px;
	            border-radius: 5px;
	            outline: none;
	        }
	        .container{
	            margin-bottom: 30px;
	            width:270px;
	        }
	        .cell{
	            display: inline-block;
	            width:30px;
	            height:30px;
	            outline: 1px solid #aaa;
	            line-height: 30px;
	            text-align: center;
	        }
	        .cell-move{
	            transition: 1s;
	        }
	    </style>
	</head>
	<body>
	    <div id="cell-demo" class="demo">
	        <transition-group name="cell" tag="div" class="container">
	            <span v-for="cell in cells" :key="cell.id" class="cell">{{cell.number}}</span>
	        </transition-group>
	        <button @click="shuffle">随机排序</button>
	    </div>
	    <script>
	        var vm = new Vue({
	            el:'#cell-demo',
	            data:{
	                cells:Array.apply(null,{length:81}).map(function (_,index) {
	                    return{
	                        id: index,
	                        number: index % 9 + 1
	                    }
	                })
	            },
	            methods:{
	                shuffle:function () {
	                    this.cells = _.shuffle(this.cells)
	                },

	            }
	        })
	    </script>
	</body>

<a href="http://owpwda5op.bkt.clouddn.com/grid.html">查看运行结果</a>

```

3.可复用的过渡

过渡可以通过 Vue 的组件系统实现复用。要创建一个可复用过渡组件，你需要做的就是将 `<transition>` 或者 `<transition-group> `作为根组件，然后将任何子组件放置在其中就可以了。
下面是一个简单的例子：

``` js

	Vue.component('my-special-transition', {
	  template: '\
	    <transition\
	      name="very-special-transition"\
	      mode="out-in"\
	      v-on:before-enter="beforeEnter"\
	      v-on:after-enter="afterEnter"\
	    >\
	      <slot></slot>\
	    </transition>\
	  ',
	  methods: {
	    beforeEnter: function (el) {
	      // ...
	    },
	    afterEnter: function (el) {
	      // ...
	    }
	  }
	})

```

动态过渡：使用v-bind：name=""来绑定不同过渡的name值，再进行切换。
当你想用 Vue 的过渡系统来定义的 CSS 过渡/动画 在不同过渡间切换会非常有用。
