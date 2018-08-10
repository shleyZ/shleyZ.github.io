---
title: Vue之计算属性
date: 2017-09-14 15:57:09
categories: Vue
tags: Vue
---
当模版比较复杂，不容易理解的时候可以使用计算属性：

    <div id="app">
        <p>message:"{{msg}}"</p>
        <p>reverseMessage:"{{reversedMessage}}"</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                msg: '我是msg',
            },
            computed:{
                reversedMessage:function () {
                    return this.msg.split('').reverse().join('');
                }
            }
        })
    </script>
    这样模版reversedMessage的属性值就与模版msg的属性值进行绑定。

### 计算属性的缓存VS方法：

上面的计算属性可以用**方法**表示如下：

	<div id="app">
        <p>message:"{{msg}}"</p>
        <p>reverseMessage:"{{reversedMessage()}}"</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                msg: '我是msg',
            },
            methods:{
                reversedMessage:function () {
                    return this.msg.split('').reverse().join('');
                }
            }
        })
    </script>

这里计算属性和方法的结果都是一致的。  
计算属性是基于它们的依赖进行缓存的,当msg的值没有改变时,多次访问reversedMessage时,计算属性会立即**返回之前缓存的计算结果,**而不会执行reversedMessage函数。  
而方法(methods),每次访问reversedMessage时，**都会执行一次reversedMessage函数**。  

### 计算属性VS被观察的属性(watch)

watch属性：

    <div id="watch">FullName:{{fullName}}</div>

    <script>
        var vmWatch = new Vue({
            el:'#watch',
            data:{
                firstName:'xiao',
                lastName:'hong',
                fullName:'xiao hong'
            },
            watch:{
                firstName: function (val) {
                    this.fullName = val + " " + this.lastName;
                },
                lastName:function (val) {
                    this.fullName = this.firstName + " " + val;
                }
            }
        })
    </script>

如果用计算属性来实现相同的效果(相对watch属性来说简单)：

	computed:{
        fullName: function () {
            return this.firstName + " " + this.lastName;
        }
    }

### 计算属性默认只有getter,但是可以自定义setter:

    <div id="watch">FullName:{{fullName}}</div>

    <script>
        var vmWatch = new Vue({
            el:'#watch',
            data:{
                firstName:'xiao',
                lastName:'hong',
                fullName:'xiao hong'
            },
            computed:{
                fullname:{
                    get: function () {
                        return this.firstName + " " + this.lastName;
                    },
                    set: function (newValue) {
                        var names = newValue.split(" ");
                        this.firstName = names[0];
                        this.lastName = names[names.length - 1];
                    }
                }
            }
        })
    </script>
运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也相应地会被更新。


### watch

	<div id="watch-example">
        <p>
            提出问题:
            <input v-model="question">
        </p>
        <p>{{answer}}</p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
    <script>
        var watchExampleVM = new Vue({
            el:'#watch-example',
            data:{
                question: "",
                answer: "提问以后会有回答"
            },
            watch:{
                question: function (newQuestion) {
                    this.answer = '等待你的输入...';
                    this.getAnswer();
                }
            },
            methods:{
            	// _.debounce 是一个通过 lodash 限制操作频率的函数。
                // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
                // ajax 请求直到用户输入完毕才会发出
                getAnswer: _.debounce(function () {
                    if(this.question.indexOf('?') === -1){
                        this.answer = '问题需要一个问号来表示...';
                        return
                    }
                    this.answer = '认真思考中...';
                    var vm = this;
                    axios.get('https://yesno.wtf/api').then(function (response) {
                        vm.answer = _.capitalize(response.data.answer)
                    }).catch(function (error) {
                        vm.answer = '错误！不能获取API' + error;
                    })
                },500)
            }
        })
    </script>

这里,使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这是计算属性无法做到的。  

注：
axios-----一个打包好的XMLHttpRequests，也就是说，是一个ajax库。 
可以实现:

1.在浏览器里建立XHR

2.通过nodejs进行http请求

甚至可以实现:

3.转换或者拦截请求数据或响应数据

4.支持Promise的API

5.可以取消请求

6.自动转换JSON

7.可以防御XSRF攻击！

lodash-----一套工具库，它内部封装了诸多对字符串、数组、对象等常见数据类型的处理函数，其中部分是目前 ECMAScript 尚未制定的规范，但同时被业界所认可的辅助函数。