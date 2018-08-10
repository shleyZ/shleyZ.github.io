---
title: Array.prototype.slice.call(obj)
date: 2017-10-11 14:01:38
categories: javascript
tags: 
---

Array.prototype.slice.call(obj)能将具有length属性的对象obj转换为数组。

1.首先我们看Array.prototype.slice()

Array.prototype.slice()
返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象。
如果开始和结束都省略，那就是返回一个新的数组，是原来数组（从头到尾浅拷贝原数组）

2.然后我们看Function.prototype.call()

	call()方法的语法是：

		fun.call(thisArg, arg1, arg2, ...)

	applay()方法的语法：
		
		fun.apply(thisArg, [argsArray])

	（两者差不多，唯一的区别就是call接受的是若干个参数的列表，而apply接受的是一个参数数组,这样实现了对象的继承）

thisArg是在fun函数运行时指定的this值，后面的arg1, arg2, ...是指定的参数列表。

这样很清楚了，Array.prototype.slice.call(obj)就是把obj这个类似数组的对象转换为数组，让它可以有数组的方法来进行其他操作。

也就相当于：

	obj.slice()

看栗子：

	var a = {length:4,0:'a',1:'b',2:'c',3:'d'}
	Array.prototype.slice.call(a)
	//["a", "b", "c", "d"]
	Array.prototype.slice.call(a,2)	
	//["c", "d"]
	Array.prototype.slice.call(a,2,3)
	//["c"]

用在函数参数中：

	function comp(a,b,c,d) {
        var newArr = Array.prototype.slice.call(arguments);
        console.log(arguments);
        console.log(newArr);
    }
    comp('a','b','c','d','e');	

这样从控制台看到：arguments是一个对象，而newArr则是一个数组。