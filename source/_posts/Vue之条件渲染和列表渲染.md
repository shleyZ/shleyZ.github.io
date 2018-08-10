---
title: Vue之条件渲染和列表渲染
date: 2017-09-16 13:47:58
categories: Vue
tags: Vue
---

## 1.条件渲染

	<h1 v-if="ok">YES</h1>
    <h1 v-else>No</h1>  //v-else来表示else块

#### 在template中配合v-if条件渲染一整组:
 
	<template v-if="ok">
        <p>title</p>
        <h2>第一段落</h2>
        <h2>第二段落</h2>
    </template>
把一个`<template>`元素当做包装元素，并在上面使用 v-if。最终的渲染结果不会包含 `<template>` 元素。
	
v-if／v-else-if／v-else组合使用:

	<div id="isIf">
        <div v-if="ran > 0.5">
            随机数大于0.5时显示
        </div>
        <div v-else-if="ran == 0.5">
            随机数等于0.5时显示
        </div>
        <div v-else>
            随机数小于0.5时显示
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#isIf',
            data:{
                ran: Math.floor(Math.random()*10)/10,  //获取0-1的随机数
            }
        })
    </script>

#### 用key管理可复用的元素：

	<div id="testKey">
	    <template v-if="loginType === 'username'">
	        <label>Username:</label>
	        <input placeholder="Enter your username">
	    </template>
	    <template v-else>
	        <label>Email:</label>
	        <input placeholder="Enter your Email">
	    </template>

        <div>
            <button id="btn">toggle login type</button>
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#testKey',
            data: {
                loginType: 'username',
            }
        })

        document.getElementById('btn').onclick = function () {
            if(vm.loginType == 'username'){
                vm.loginType = 'Email';
            }else{
                vm.loginType = 'username';
            }
        }
    </script>   

 这样执行之后会有一些问题：如果input标签中有内容的话，每次切换，input中的内容都是不变的，不能重新渲染input标签。

 切换 loginType 将*不会清除用户已经输入的内容*。因为两个模板使用了相同的元素，<input> 不会被替换,仅仅是替换了它的 placeholder...

 
 #### 如果要不复用template的元素的话，就需要用key属性(具有唯一值):

	<div id="testKey">
        <template v-if="loginType === 'username'">
            <label>Username:</label>
            <input placeholder="Enter your username" key="input-username">
        </template>
        <template v-else>
            <label>Email:</label>
            <input placeholder="Enter your Email" key="input-Email">
        </template>

        <div>
            <button id="btn">toggle login type</button>
        </div>
    </div>

 现在，每次切换时，*输入框都将被重新渲染*。

#### v-show 也是根据条件展示元素的指令

v-show 只是简单的切换CSS样式的display属性，不支持 `<template>` 语法，也不支持 v-else

v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件不太可能改变，则使用 v-if 较好。

## 2.列表渲染

#### v-for根据一组数组的选项列表进行渲染:

	<div id="testKey">
        <div v-for="item in items">
            {{item.message}}
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#testKey',
            data: {
                items:[
                    {message:'Joe'},
                    {message:'Li'},
                    {message:'Han'}
                    ],
            }
        })
    </script>

以v-for=“item in items”形式的特殊语法

还支持一个可选的第二个参数为当前项的索引：v-for="(item, index) in items"  

还可以用 of 替代 in 作为分隔符 

	<div id="testKey">
        <div v-for="(item,index) in items">
            {{headMessage}}-{{index}}-{{item.message}}
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#testKey',
            data: {
                headMessage: 'Class',
                items:[
                    {message:'Joe'},
                    {message:'Li'},
                    {message:'Han'}
                    ],
            }
        })
    </script>
输出为：

	Class-0-Joe
	Class-1-Li
	Class-2-Han

#### 对象的v-for：

	<div id="testKey">
        <div v-for="(value,key,index) in object">
            {{index}}:{{key}}:{{value}}
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#testKey',
            data: {
                object:{
                    first: 'zhap',
                    second: 'lin',
                    third: 'Joe'
                }
            }
        })
    </script>	

`v-for="(value,key,index) in object" `的三个参数分别对应对象的*值，键名，索引*   

上面的代码输出为：
	0:first:zhap
	1:second:lin
	2:third:Joe

#### key

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用*“就地复用”*策略。

如数据项的顺序被改变,Vue 将不会移动 DOM 元素来匹配数据项的顺序,而是简单复用此处每个元素,并且确保它在特定索引下显示已被渲染过的每个元素.

这个默认的模式是高效的，但是只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。

如果想要重用或者重新排序现有的元素，需要给每项提供唯一的key，*用v-bind来绑定key*。

	<div v-for="item in items" :key="item.id">
  		<!-- 内容 -->
	</div>

#### 数组更新检测

1.变异方法(mutation method)：

push(), 在数组末尾添加
pop(), 删除数组末位
shift(), 删除数组首位
unshift(), 在数组首位添加
splice(), 从数组中添加/删除项目
sort(), 按规则排序
reverse(),按原来顺序的倒序排列
	
这些变异方法会触发视图更新

2.非变异方法(non-mutating method):

filter(),
concat(),
slice(),

这些不会改变原始数组,但总是返回一个新数组.这样不会重新渲染整个列表.Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的、启发式的方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。

3.注意事项

Vue不能检测以下变动的*数组*：
    
    利用索引值来设置某项：vm.items[indexOfitem] = newValue
    直接修改数组的长度：vm.items.length = newLength

想要实现相同效果并且出发更新，解决办法：

    Vue.set(vm.items,indexOfItem,newValue)或者 vm.items.splice(indexOfItem,1,newValue)   
    vm.items.splice(newLength)

#### 对象更改检测注意事项

Vue 不能检测对象属性的添加或删除，可以这样实现:

    Vue.set(vm.userProfile, 'age', 27)

还可以使用 vm.$set 实例方法，它只是全局 Vue.set 的别名:

    this.$set(this.userProfile, 'age', 27)

要添加多个属性，使用assign:

    this.userProfile = Object.assign({}, this.userProfile, {
        age: 27,
        favoriteColor: 'Vue Green'
    })

#### 显示过滤／排序结果

可以用计算属性computed实现，当计算属性不适用时可以用方法methods

    <div id="testKey">
        <div v-for="n in evenNumbers">
            {{n}}
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#testKey',
            data: {
                numbers:[1,2,3,4,5,6,7,8,9,10]
            },
            computed:{
                evenNumbers:function () {
                    return this.numbers.filter(function (number) {
                        return number%2 === 0;
                    })
                }
            }
        })
    </script>

用methods:

    <div id="testKey">
        <div v-for="n in even(numbers)">
            {{n}}
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#testKey',
            data: {
                numbers:[1,2,3,4,5,6,7,8,9,10]
            },
            methods:{
                even:function (numbers) {
                    return numbers.filter(function (number) {
                        return number%2 === 0;
                    })
                }
            }
        })
    </script>

#### v-for和v-if

当它们处于同一节点时，v-for的优先级比v-if高。意味着v-if将分别重复运行于v-for的每个循环中

    <li v-for="todo in todos" v-if="!todo.isComplete">
        {{ todo }}
    </li>    