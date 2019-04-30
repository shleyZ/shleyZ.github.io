---
title: Vue组件--内容分发slot
date: 2017-09-22 10:42:35
categories: Vue
---


*使用插槽分发内容*

假如父组件需要在子组件内放一些DOM元素，那么这些DOM元素是显示、不显示、在哪个地方显示、如何显示，就是slot内容分发。Vue实现了一个内容分发API,使用特殊的`<slot>`元素作为原始内容的插槽。

1.单个插槽

除非子组件有至少一个slot插槽，否则父组件的内容会被丢弃。  
如果子组件有一个插槽时，父组件的整个内容片段就会插入到插槽所在的DOM位置，并替换掉插槽标签slot。  
最初在 `<slot> `标签中的任何内容都被视为备用内容。备用内容在子组件的作用域内编译，并且只有在宿主元素为空，且没有要插入的内容时才显示备用内容。

``` html

    <div id="test">
        <h1>我是父组件的标题</h1>
        <my-component>
            <p>这是父初始内容</p>
            <p>这是更多初始内容</p>
        </my-component>
    </div>

    <script>
        Vue.component('my-component', {
            template: '<div>' +
            '<h2>我是子组件的标题</h2>' + '<slot>只有在没有分发内容时我才会显示</slot>' +
            '</div>',
        })
        new Vue({
            el: '#test',
        })
    </script>

```

如果子组件中没有slot插槽，那么父元素的内容（两个p标签）都会被丢弃。


2.具名插槽

`<slot>` 元素可以用一个特殊的属性 name 来配置如何分发内容。多个插槽可以有不同的名字。具名插槽将匹配内容片段中有对应 slot 特性的元素。  
父组件在要分发的标签内添加：  slot = ‘slotName’,  
子组件在对应分发的位置的slot标签内添加：`<slot name = 'slotName'>`.

``` html

    <div id="test">
        <my-component>
            <h1 slot="header">这是slot为header</h1>
            <p>这是主要内容</p>
            <p>这是另一个主要片段</p>
            <p slot="footer">这是slot为footer</p>
        </my-component>
    </div>

    <script>
        Vue.component('my-component', {
            template: '<div class="container">' +
                    '<header><slot name="header"></slot></header>'+
                    '<main><slot></slot></main>' +
                    '<footer><slot name="footer"></slot></footer>' +
            '</div>',
        })
        new Vue({
            el: '#test',

        })
    </script>

```

3.作用域插槽

使用一个 (能够传递数据到) 可重用模板替换已渲染元素。  
在子组件中，只需将数据传递到插槽，就像你将 props 传递给组件一样  
在父级中，具有特殊属性 scope 的 `<template>` 元素必须存在，表示它是作用域插槽的模板。scope 的值对应一个临时变量名，此变量接收从子组件中传递的 props 对象:
 
``` html   

    <div id="test">
        <div class="parent">
            <child>
                <template scope="props">
                    <p>hello from parent</p>
                    <p>{{props.text}}</p>
                </template>
            </child>
        </div>
    </div>

    <script>
        Vue.component('child', {
            template: '<div class="child">' + '<slot text="hello from child"></slot>' +
            '</div>',
        })
        new Vue({
            el: '#test',
        })
    </script>

```

作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项：

``` html

    <div id="test">
        <my-awesome-list :items="items">
            <template slot="item" scope="props">
                <li class="my-fancy-item">{{ props.text }}</li>
            </template>
        </my-awesome-list>
    </div>

    <script>
        Vue.component('my-awesome-list', {
            props:['items'],
            template: '<ul>' + '<slot name="item" v-for="item in items" v-bind:text="item.text">这里是备用内容</slot>' +
            '</ul>',
        })
        new Vue({
            el: '#test',
            data:{
                items:[
                    {text:'apple'},
                    {text:'pear'},
                    {text:'grape'}
                    ]
            },

        })
    </script>

```