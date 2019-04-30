---
title: Vue组件--动态组件
date: 2017-09-22 11:14:11
categories: Vue
---
动态组件就是几个组件放在一个挂载点，父组件根据变量来决定显示哪个

`<component v-bind:is="which_to_show"></component>`,根据which_to_show的值来决定显示哪个组件,which_to_show值的改变相对显示的组件也会改变。

``` html

	<div id="app">
        <button @click="toShow">点击切换组件</button>
        <my_component v-bind:is="which_to_show"></my_component>
    </div>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                which_to_show: "first"
            },
            methods: {
                toShow: function () {
                    var myArr=['first','second','third',''];
                    var index = myArr.indexOf(this.which_to_show);
                    if(index < 3){
                        this.which_to_show = myArr[index + 1];
                    }else{
                        this.which_to_show = myArr[0];
                    }
                }
            },
            components:{
                first:{
                    template:'<div>这是first组件</div>'
                },
                second:{
                    template:'<div>这是second组件</div>'
                },
                third:{
                    template:'<div>这是third组件</div>'
                }
            }
        });
    </script>

```

#### keep-alive

假如需要子组件在切换后，依然需要他保留在内存中，避免下次出现的时候重新渲染。那么就应该在component标签中添加keep-alive指令。