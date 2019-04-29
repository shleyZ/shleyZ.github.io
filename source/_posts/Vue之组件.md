---
title: Vue之组件
date: 2017-09-19 15:13:02
categories: Vue
tags: Vue
---

1.注册全局组件：

	Vue.component(tagName, options)

使用：

	<tagName></tagName>

*要确保在初始化根实例之前注册了组件*

    
``` html

	<div id="test">
        <my-component></my-component>
    </div>
    <script>
        Vue.component('my-component',{
            template:"<p>组件渲染的一个段落</p>"
        });
        var vm = new Vue({
            el:'#test'
        })
    </script>

```

2.局部注册组件：
使组件仅在另一个实例/组件的作用域中可用

``` html

	<div id="test">
        <my-component></my-component>
        <my-component></my-component>
    </div>

    <script>
        var Child={
            template:'<p>这是child组件渲染的部分</p>'
        };
        var vm = new Vue({
            el:'#test',
            components:{
                'my-component':Child
            }
        })
    </script>

```

3. data必须是函数

4. 组合组件

父子组件的关系可以总结为 props down, events up。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。

### props

组件实例的作用域是孤立的，这意味着不能在子组件的模版内直接引用父组件的数据。如果要访问父组件的数据，子组件要显式的用props选项声明它期待获取的数据。

``` html

	<div id="test">
        <child message="hello"></child>
    </div>

    <script>
        Vue.component('child',{
            props:['message'],
            template: '<p>{{message}}</p>',
        });

        var vm = new Vue({
            el:'#test',
        })
    </script>

```

动态props：

``` html

	<div id="test">
        <todo-item :my-message="todo"></todo-item>
    </div>

    <script>
        Vue.component('todo-item',{
            props:['myMessage'],
            template: '<p>{{myMessage.text + " " +  myMessage.isComplete}}</p>',
        });

        var vm = new Vue({
            el:'#test',
            data:{
                todo:{
                    text:'Learn Vue',
                    isComplete:false
                }
            }
        })
    </script>    

```

*单向数据流：prop是单向绑定的，当父组件的属性改变时，子组件的所有prop属性都会更新。但子组件不会影响父组件。不可以在子组件中更改prop属性，否则会警告*

如果需要使用prop属性，可以定义一个局部变量活着一个计算属性：

``` html

	<div id="test">
        <todo-item :my-message="todo"></todo-item>
    </div>
    
    <script>
        Vue.component('todo-item',{
            props:['myMessage'],
            template: '<p>{{myMessage.text + " " +  myMessage.isComplete + " 大写表示:" + content}}</p>',
            computed:{
                content:function () {
                    return this.myMessage.text.toUpperCase() + this.myMessage.isComplete;
                }
            }
        });
    
        var vm = new Vue({
            el:'#test',
            data:{
                todo:{
                    text:'Learn Vue',
                    isComplete:1
                }
            }
        })
    </script>

```

*注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。*

prop验证

props 会在组件实例创建之前进行校验，所以在 default 或 validator 函数里，诸如 data、computed 或 methods 等实例属性还无法使用。

``` js

    Vue.component('example', {
        props: {
            // 基础类型检测 (`null` 意思是任何类型都可以)
            propA: Number,
            // 多种类型
            propB: [String, Number],
            // 必传且是字符串
            propC: {
              type: String,
              required: true
            },
            // 数字，有默认值
            propD: {
              type: Number,
              default: 100
            },
            // 数组/对象的默认值应当由一个工厂函数返回
            propE: {
              type: Object,
              default: function () {
                return { message: 'hello' }
              }
            },
            // 自定义验证函数
            propF: {
              validator: function (value) {
                return value > 10
              }
            }
        }
    })

```

type 可以是下面原生构造器：

String
Number
Boolean
Function
Object
Array
Symbo    
