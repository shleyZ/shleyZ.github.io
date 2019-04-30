---
title: Vue--混合
date: 2017-09-23 15:54:08
categories: Vue
---

混合 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。混合对象可以包含任意组件选项。以组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项。
混合以一种灵活的方式为组件提供分布复用功能。混合对象可以包含任意的组件选项。当组件使用了混合对象时，混合对象的所有选项将被“混入”组件自己的选项中。
两种写法都不是很容易理解，我的理解是：组件使用混合对象时，组件就拥有了混合对象里面的所有的属性，相当于把这个组件extend了。

原文示例：

``` js
	// 定义一个混合对象
	var myMixin = {
	  created: function () {
	    this.hello()
	  },
	  methods: {
	    hello: function () {
	      console.log('hello from mixin!')
	    }
	  }
	}
	// 定义一个使用混合对象的组件
	var Component = Vue.extend({
	  mixins: [myMixin]
	})
	var component = new Component() // => "hello from mixin!"

``` 

1.选项合并：

当组件和混合对象含有同名选项时，这些选项将以恰当的方式混合。比如，同名钩子函数将混合为一个数组，因此都将被调用。另外，混合对象的 钩子将在组件自身钩子 之前 调用 

``` js

	var mixin = {
	  created: function () {
	    console.log('混合对象的钩子被调用')
	  }
	}
	new Vue({
	  mixins: [mixin],
	  created: function () {
	    console.log('组件钩子被调用')
	  }
	})
	// => "混合对象的钩子被调用"
	// => "组件钩子被调用"

```

值为对象的选项，例如 methods, components 和 directives，将被混合为同一个对象。两个对象键名冲突时，取组件对象的键值对。

``` js

	var mixin = {
	  methods: {
	    foo: function () {
	      console.log('foo')
	    },
	    conflicting: function () {
	      console.log('from mixin')
	    }
	  }
	}
	var vm = new Vue({
	  mixins: [mixin],
	  methods: {
	    bar: function () {
	      console.log('bar')
	    },
	    conflicting: function () {
	      console.log('from self')
	    }
	  }
	})
	vm.foo() // => "foo"
	vm.bar() // => "bar"
	vm.conflicting() // => "from self"	

```

2.全局混合	

全局注册混合对象。一旦使用全局混合对象，将会影响到 所有 之后创建的 Vue 实例。

``` js

	// 为自定义的选项 'myOption' 注入一个处理器。
	Vue.mixin({
	  created: function () {
	    var myOption = this.$options.myOption
	    if (myOption) {
	      console.log(myOption)
	    }
	  }
	})
	new Vue({
	  myOption: 'hello!'
	})
	// => "hello!"

```
大多数情况下，全局混合只应当应用于自定义选项，就像上面示例一样。也可以将其用作 Plugins 以避免产生重复应用	