---
title: Vue动画--单元素或组件的过渡
date: 2017-09-22 15:35:15
categories: Vue
tags: Vue
---

#### 单元素／组件的过渡

1.Vue提供了transition的封装组件，在以下情境中可以给任何元素和组件添加entering／leaving过渡。

	条件渲染 (使用 v-if)
	条件展示 (使用 v-show)
	动态组件
	组件根节点

``` css 
	<style>
        .fade-enter-active, .fade-leave-active {
            transition: opacity .5s
        }
        .fade-enter, .fade-leave-to /* .fade-leave-active in below version 2.1.8 */ {
            opacity: 0
        }
    </style>
	<div id="demo">
        <button v-on:click="show=!show">Toggle</button>
        <transition name="fade">
            <p v-if="show">hello!</p>
        </transition>
    </div>
    <script>
        new Vue({
            el:'#demo',
            data: {
                show:true
            }
        })
    </script>

```

当插入或删除包含在 transition 组件中的元素时，Vue 将会做以下处理：
1.自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。
2.如果过渡组件提供了 JavaScript 钩子函数，这些钩子函数将在恰当的时机被调用。
3.如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，和 Vue 的 nextTick 概念不同)   


2.在进入／离开的过渡中，有6个类名进行切换：

	1.v-enter：定义进入*过渡的开始状态*。在元素被插入时生效，在下一个帧移除。
	2.v-enter-active：定义*过渡的状态*。在元素整个过渡过程中作用，在元素被插入时生效，在 transition/animation 完成之后移除。这个类可以被用来定义过渡的过程时间，延迟和曲线函数。
	3. v-enter-to: 2.1.8版及以上 定义*进入过渡的结束状态*。在元素被插入一帧后生效 (于此同时 v-enter 被删除)，在 transition/animation 完成之后移除。
	4.v-leave: 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
	5.v-leave-active：定义过渡的状态。在元素整个过渡过程中作用，在离开过渡被触发后立即生效，在 transition/animation 完成之后移除。这个类可以被用来定义过渡的过程时间，延迟和曲线函数。
	6.v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。在离开过渡被触发一帧后生效 (于此同时 v-leave 被删除)，在 transition/animation 完成之后移除。

3.自定义过渡类名：

我们可以通过以下特性来自定义过渡类名，它们的优先级高于普通的类名：

enter-class
enter-active-class
enter-to-class
leave-class
leave-active-class
leave-to-class

例如：
	
``` html

	<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
	<div id="demo">
        <button v-on:click="show=!show">Toggle</button>
        <transition name="custom-classes-transition" enter-active-class="animated data" leave-active-class="animated bounceOutRight">
            <p v-if="show">hello!</p>
        </transition>
    </div>
    <script>
        new Vue({
            el:'#demo',
            data: {
                show:true
            }
        })
    </script>

```

这样方便于结合其他的动画库，比如上面的Animate.css动画库    


4.设置显性的过渡持续时间：

	<transition :duration="1000">...</transition>
	<transition :duration="{ enter: 500, leave: 800 }">...</transition> //定制进入和移出的持续时间

5.多个组件的过渡可以用动态组件来实现：

``` css
	<style>
        .component-fade-enter-active, .component-fade-leave-active {
            transition: opacity .3s ease;
        }
        .component-fade-enter, .component-fade-leave-to
            /* .component-fade-leave-active for below version 2.1.8 */ {
            opacity: 0;
        }
    </style>
	<div id="demo">
        <input type="radio" name="view" id="a" v-on:click="toggle">
        <label for="a">A</label>
        <input type="radio" name="view" id="b" v-on:click="toggle">
        <label for="b">B</label>
        <transition name="component-fade" mode="out-in">
            <component v-bind:is="view"></component>
        </transition>
    </div>
    <script>
        new Vue({
            el:'#demo',
            data: {
                view:'v-a'
            },
            components:{
                'v-a':{
                    template:'<button>componentA</button>'
                },
                'v-b':{
                    template:'<button>componentB</button>'
                }
            },
            methods:{
                toggle:function () {
                    if(this.view == 'v-a'){
                        this.view = 'v-b'
                    }else if(this.view == 'v-b'){
                        this.view = 'v-a'
                    }
                }
            }
        })
    </script>

```

6.Vue的过渡模式

默认是不设置过渡模式的，默认是同时进入喝离开

in-out：新元素先进入过渡，完成之后当前元素过渡离开。
out-in：当前元素先过渡离开，离开后新元素再过渡进入。

	<transition name="fade" mode="out-in"></transition>

	
