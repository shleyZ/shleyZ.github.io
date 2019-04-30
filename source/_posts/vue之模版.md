---
title: vue之模版
date: 2017-09-09 15:29:19
categories: Vue
---
### 模版语法:

1.使用Mustache语法(双大括号)的文本插值(将msg值解读为文本),只要绑定的对象上的msg值发生变化，插值处的内容都会变化。
但是当使用v-once指令时，可以一次性使用插值，以后msg值发生变化，插值的内容也不会变化。

    <span>Message:{{msg}}</span>
    <span v-once>这里面的值不会变化{{msg}}</span>

2.如果想解读为html代码的话，使用v-html指令：
    
    <div v-html = "rawHtml"></div>

这样这个div的内容就会被rawHtml的值给替换掉。（这样很容易导致xss攻击，最好不要对用户提供的内容进行插值）	

4.使用v-bind指令给元素绑定属性。(id/class/.../自定义属性等)

    <div v-bind:id="dynamicId"></div>

5.模版的双大括号中使用表达式(只能是单个表达式，不能是语句声明，不能是多个表达式，可以是三元表达式):

    {{ok?'YES':'NO'}}
    <div v-bind:id="'list'+id"></div>

6.vue的指令(v-bind,v-once,v-html,v-if)的职责是:当表达式的值改变时，其产生的连带影响，响应式的作用到dom.
指令的值一般为单个表达式，除v-for之外.  
一个指令只能接受一个参数。

    <p v-if="seen">如果seen值为true，你就能看到我</p>
    <p v-on:click="dosomething">点我有惊喜哦</p>

7.缩写（Vue.js 为 v-bind 和 v-on 这两个最常用的指令，提供了特定简写):

    <div v-bind:id="dynamicId"></div> 
    <div :id="dynamicId"></div> 
    <p v-on:click="dosomething">点我有惊喜哦</p> 
    <p @click="dosomething">点我有惊喜哦</p>  

