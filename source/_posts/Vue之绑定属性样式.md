---
title: Vue之绑定属性样式
date: 2017-09-15 16:06:12
categories: Vue
tags: Vue
---
1.绑定Class:

对象语法：

	<div id="app">
        <div class="static" v-bind:class="classObj">aaaa</div>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                classObj: {
                    'active': true,
                    'text-danger': true
                }
            }
        })
    </script>

数组语法：

	<div id="app">
        <div class="static" v-bind:class="[activeClass,errorClass]">aaaa</div>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                activeClass: 'active',
                errorClass: 'text-danger'
            }
        })
    </script>    

绑定到组件上时，这些类将被添加到根元素上面。这个元素上已经存在的类不会被覆盖。  

2.绑定到内联样式：

对象语法：

	<div id="app">
        <div v-bind:style="styleObj">aaa</div>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                styleObj:{
                    width: '100px',
                    height: '100px',
                    color: 'red',
                    backgroundColor: 'orange'
                }
            }
        })
    </script>

数组语法：

	<div id="app">
        <div v-bind:style="[baseStyles, overridingStyles]">aaa</div>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                baseStyles:{
                    width: '200px',
                    height: '200px'
                },
                overridingStyles:{
                    color: 'white',
                    backgroundColor: 'red',
                    transform: 'rotate(7deg)'  //旋转
                }
            }
        })
    </script>    

当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

多重值:

	<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
