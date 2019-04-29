---
title: Vue之表单输入
date: 2017-09-17 16:56:47
categories: Vue
tags: Vue
---
v-model指令在表单控件元素上创建双向数据绑定。它本质上来说是一种语法糖（没有改变什么功能，只是对于程序员来说更容易理解），负责监听用户的输入事件以更新数据。*它会根据控件类型自动选取正确的方法来更新元素。*

注意：
	1.v-model会忽略掉所有表单元素的value、checked、selected特性的初始值。因为*它选择的是Vue的实力数据作为具体的值*。所以应该在vue的data中声明初始值。
	2.对于要求IME(input method如中文、日语、韩语等),v-model 不会在 ime 输入中得到更新。如果你也想实现更新，请使用 input 事件。

### 基础用法(v-model 绑定的 value 通常是静态字符串)

1. 文本:

``` html

	<div id="test">
        <input v-model="message" placeholder="edit me">
        <p>Message is: {{ message }}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{message:""}
        })
    </script>	

```

 2. 多行文本

注意：在文本区域插值 `(<textarea></textarea>)` 并不会生效，应用 v-model 来代替。

``` html

 	<div id="test">
        <p>多行文本：</p>
        <p style="white-space: pre-line;">{{message}}</p>
        <textarea v-model="message" placeholder="请输入多行文本"></textarea>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{message:""}
        })
    </script>

```

3.复选框

``` html

	<div id="test">
        <p>复选框：</p>
        <input type="checkbox" value="Joe" id="Joe" v-model="checkedNames">
        <label for="Joe">Joe</label>
        <input type="checkbox" value="LiLei" id="LiLei" v-model="checkedNames">
        <label for="LiLei">LiLei</label>
        <input type="checkbox" value="HanMeiMei" id="HanMeiMei" v-model="checkedNames">
        <label for="HanMeiMei">HanMeiMei</label>
        <p>{{checkedNames}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{checkedNames:[]}
        })
    </script>

```

for 属性规定 label 与哪个表单元素(id)绑定。

复选框v-model关联：*数据为数组形式时，关联的是选中的复选框的value值。是字符串形式时，关联的则是选中的状态true／false*

4.单选按钮

``` html

	<div id="test">
        <p>单选框：</p>
        <input type="radio" value="Joe" id="Joe" v-model="checkedNames">
        <label for="Joe">Joe</label>
        <input type="radio" value="LiLei" id="LiLei" v-model="checkedNames">
        <label for="LiLei">LiLei</label>
        <input type="radio" value="HanMeiMei" id="HanMeiMei" v-model="checkedNames">
        <label for="HanMeiMei">HanMeiMei</label>
        <p>{{checkedNames}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{checkedNames:''}
        })
    </script>

```

单选框v-model关联的只是单选选中的value值。    

5.下拉框
单选：

``` html

	<div id="test">
        <p>下拉框：</p>
        <select name="" id="" v-model="selected">
            <option value="请选择" disabled>请选择</option>
            <option value="A">A</option>
            <option value="B">B</option>
            <option value="C">C</option>
        </select>
        <p>选择的是：{{selected}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{selected:''}
        })
    </script>

```

多选下拉框只需要在select标签里面增加multiple

用v-for动态渲染：

``` html

	<div id="test">
        <p>v-for实现下拉框：</p>
        <select v-model="selected">
            <option v-for="option in options" v-bind:value="option.value">{{option.text}}</option>
        </select>
        <p>选择的是：{{selected}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{
                selected:'A',
                options:[
                    {text:'one',value:'A'},
                    {text:'two',value:'B'},
                    {text:'three',value:'C'}
                ]
            }
        })
    </script>  

```

### 值绑定:(绑定 value 到 Vue 实例的一个动态属性上)

1.复选框：
`v-bind:true-value="a" v-bind:false-value="b"`

``` html

	<div id="test">
        <p>复选框：</p>
        <input type="checkbox" v-model="checkedNames" v-bind:true-value="a" v-bind:false-value="b">
        <p>{{checkedNames}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{checkedNames:'',
                a:'yes',
                b:'no'
            },
        })
    </script>

```

2.单选按钮
`<input type="radio" v-model="pick" v-bind:value="a">`

``` html

	<div id="test">
        <p>单选按钮：</p>
        <input type="radio" v-model="select" v-bind:value="a">
        <p>{{select}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{select:'',
                a:'selected'
            },
        })
    </script>

``` 

3.下拉列表：

``` html

	<div id="test">
        <p>v-for实现下拉框：</p>
        <select v-model="selected">
            <option v-bind:value="{number:123}">123</option>
            <option v-bind:value="{number:456}">456</option>
            <option v-bind:value="{number:789}">789</option>
        </select>
        <p>选择的是：{{selected}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{
                selected:'',
            }
        })
    </script>

```

### v-model的修饰符

.lazy   转变为在 change 事件中同步
.number   自动将用户的输入值转为 Number 类型(如果原值的转换结果为 NaN 则返回原值)
.trim    自动过滤用户输入的首尾空格