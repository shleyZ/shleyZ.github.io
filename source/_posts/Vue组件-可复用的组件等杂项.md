---
title: Vue组件--可复用的组件等杂项
date: 2017-09-22 14:05:51
categories: Vue
tags: Vue
---

#### 1.可复用的组件

可复用的组件需要与其他的组件解耦。它应当定义一个清晰的接口，方便使用。

Vue组件的API包括三个部分：

	Props 允许外部环境传递数据给组件  
	Events 允许从外部环境在组件内触发副作用  
	Slots 允许外部环境将额外的内容组合在组件中。  

这样我们可以有一个清晰的模版：

	<component :foo="baz" :bar="qux" @event-a="doThis" @event-b="doThat">
		<img slot="con" src="" alt="">
		<p slot="main-text">Hello!</p>
	</component>


#### 2.子组件索引

虽然有props和events来实现外部环境向内部传递数据，内部触发事件影响外部环境，但是我们仍然需要在javascript中直接访问子组件。  
这时我们就需要在子组件中添加ref索引ID。

	<div id="app">
        <navbar ref="navbar"></navbar>
        <pagefooter ref="pagefooter"></pagefooter>
    </div>
    <script>
        Vue.component('navbar',{
            templates:'<div>' +
            '<ul>' +
            '<li>apple</li>' +
            '<li>pear</li>' +
            '<li>grape</li>' +
            '</ul>' +
            '</div>',
            data:function () {
                return {
                    navs:[1,2,3]
                }
            }
        })

        Vue.component('pagefooter',{
            templated:'<div><img src="https://www.baidu.com"></div>',
            data:function () {
                return {
                    footer:'hello'
                }
            }
        })

        var vm = new Vue({
            el:'#app',
            mounted:function () {
                //钩子函数，在vue的多有函数执行之前必须先执行钩子函数。
                console.log(this.$refs.navbar.navs);
                console.log(this.$refs.pagefooter.footer);
            }
        })
    </script>

#### 3.异步组件

暂时还没有弄明白

#### 4.组件命名约定

当注册组件 (或者 props) 时，可以使用 kebab-case，camelCase，或 PascalCase。

	// 在组件定义中
	components: {
	// 使用 kebab-case 形式注册
	'kebab-cased-component': { /* ... */ },
	// register using camelCase
	'camelCasedComponent': { /* ... */ },
	// register using PascalCase
	'PascalCasedComponent': { /* ... */ }
	}    

在 HTML 模板中，请使用 kebab-case 形式：

	<!-- 在 HTML 模板中始终使用 kebab-case -->
	<kebab-cased-component></kebab-cased-component>
	<camel-cased-component></camel-cased-component>
	<pascal-cased-component></pascal-cased-component>	

当使用字符串模式时，可以不受 HTML 的 case-insensitive 限制。这意味实际上在模板中，你可以使用下面的方式来引用你的组件：

	kebab-case
	camelCase 或 kebab-case 如果组件已经被定义为 camelCase
	kebab-case，camelCase 或 PascalCase 如果组件已经被定义为 PascalCase	

PascalCase 是最通用的 声明约定 而 kebab-case 是最通用的 使用约定。	

#### 递归组件

当组件有name选项时，组件在它的模版里面可以递归的调用自己。  
当你利用Vue.component全局注册了一个组件，全局的 ID 作为组件的 name 选项，被自动设置.

