---
title: Vue之事件处理
date: 2017-09-17 15:58:58
categories: Vue
---

1.方法事件处理器

用 v-on 指令监听 DOM 事件来触发一些 JavaScript 代码.
当事件处理的逻辑很复杂时，v-on 可以接收一个定义的方法来调用：

``` js

	<div id="test">
        <button v-on:click="greet">Greet</button>
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            data:{
                name:'Vue.js'
            },
            methods:{
                greet:function (event) {
                    alert('hello'+this.name+'!');
                    if(event){
                        alert(event.target.tagName);
                    }
                }
            }
        })
    </script>

``` 

2.内联事件处理器

如果需要在内联语句里面访问原生DOM事件，可以使用特殊变量$event把它传入方法：

``` js

	<div id="test">
        <button v-on:click="warn('Form cannot be submmit yet',$event)">warn</button>
    </div>
    
    var vm = new Vue({
        el:'#test',
        methods:{
            warn:function (msg,event) {
                if(event){
                    event.preventDefault();
                }
                alert(msg);

            }
        }
    })

```

3.事件修饰符

在事件处理程序中调用 event.preventDefault() 或 event.stopPropagation() 是非常常见的需求。尽管我们可以在 methods 中轻松实现这点，但更好的方式是：*methods 只有纯粹的数据逻辑，而不是去处理 DOM 事件细节*。
为了解决这个问题，Vue.js 为 v-on 提供了 事件修饰符。通过由点 (.) 表示的指令后缀来调用修饰符。

.stop
.prevent
.capture
.self
.once    

``` html

	<!-- 阻止单击事件冒泡 -->
	<a v-on:click.stop="doThis"></a>
	<!-- 提交事件不再重载页面 -->
	<form v-on:submit.prevent="onSubmit"></form>
	<!-- 修饰符可以串联 -->
	<a v-on:click.stop.prevent="doThat"></a>
	<!-- 只有修饰符 -->
	<form v-on:submit.prevent></form>
	<!-- 添加事件侦听器时使用事件捕获模式 -->
	<div v-on:click.capture="doThis">...</div>
	<!-- 只当事件在该元素本身 (比如不是子元素) 触发时触发回调 -->
	<div v-on:click.self="doThat">...</div>
	<!-- 点击事件将只会触发一次 -->
	<a v-on:click.once="doThis"></a>

``` 

4.键值修饰符

监听键盘事件的键值，Vue 为最常用的按键提供了别名。

``` html

	<div id="test">
        <input v-on:keyup.enter="sub">
    </div>
    <script>
        var vm = new Vue({
            el:'#test',
            methods:{
                sub:function () {
                    alert('回车事件');
                }
            }
        })
    </script>

```

全部的按键别名：

.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right

可以通过全局 config.keyCodes 对象自定义键值修饰符别名：
	
	// 可以使用 v-on:keyup.f1
	Vue.config.keyCodes.f1 = 112

5.修饰键

可以用如下修饰符开启鼠标或键盘事件监听，使在按键按下时发生响应。

.ctrl
.alt
.shift
.meta

注意：在 Mac 系统键盘上，meta 对应命令键 (⌘)。在 Windows 系统键盘 meta 对应 windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 键盘及其后续，比如 Knight 键盘，space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

6.鼠标按钮修饰符

.left
.right
.middle
这些修饰符会限制处理程序监听特定的滑鼠按键。
