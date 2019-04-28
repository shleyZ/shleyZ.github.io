---
title: 关于ES6语法
date: 2017-09-28 10:39:20
categories: javascript
---

## 1.let和const命令

#### let

1.let用法类似var，但是let声明的变量只在let代码块内有效。用let很适用于for循环。

2.for循环有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

	for (let i = 0; i < 3; i++) {
	  let i = 'abc';
	  console.log(i);
	}
	// abc
	// abc
	// abc

上面代码正确运行，输出了3次abc。这表明函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域。	

3.let不存在变量提升，即不可以没有声明就使用。否则会报错。

	// var 的情况
	console.log(foo); // 输出undefined
	var foo = 2;
	
	// let 的情况
	console.log(bar); // 报错ReferenceError
	let bar = 2;

这表示在let声明bar之前，变量bar是不存在的，这时如果用到它，就会抛出一个错误。

4.暂时性死区

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。  
总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。	

ES6 规定暂时性死区和let、const语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。这样的错误在 ES5 是很常见的，现在有了这种规定，避免此类错误就很容易了。

5.不允许重复声明

let不允许在*相同作用域内*，重复声明同一个变量。

	// 报错
	function func() {
	  let a = 10;
	  var a = 1;
	}

	// 报错
	function func() {
	  let a = 10;
	  let a = 1;
	}

因此，不能在函数内部重新声明参数。

	function func(arg) {
	  let arg; // 报错
	}

	function func(arg) {
	  {
	    let arg; // 不报错
	  }
	}

#### 块级作用域

ES5的作用域只有全局作用域和函数作用域
ES6中的let相当于增加了块级作用域

1.块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。

	// IIFE 写法
	(function () {
	  var tmp = ...;
	  ...
	}());

	// 块级作用域写法
	{
	  let tmp = ...;
	  ...
	}

2.块级作用域与函数声明

ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。

	// 情况一（在ES5规定中是非法的，但是实际可以运行不会报错）
	if (true) {
	  function f() {}
	}

	// 情况二（在ES5规定中是非法的，但是实际可以运行不会报错）
	try {
	  function f() {}
	} catch(e) {
	  // ...
	}	

ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

ES6在附录B里面规定，浏览器的实现可以不遵守上面的规定，有自己的行为方式。

	允许在块级作用域内声明函数。
	函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
	同时，函数声明还会提升到所在的块级作用域的头部。

注意，上面三条规则只对 ES6 的浏览器实现有效，其他环境的实现不用遵守，还是将块级作用域的函数声明当作let处理。

考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。

	// 函数声明语句
	{
	  let a = 'secret';
	  function f() {
	    return a;
	  }
	}

	// 函数表达式
	{
	  let a = 'secret';
	  let f = function () {
	    return a;
	  };
	}


#### const 声明一个只读的常量

1.一旦声明，常量的值不能改变。并且声明的时候必须赋值，不能先声明后赋值。

	const PI = 3.1415;
	PI // 3.1415

	PI = 3;
	// TypeError: Assignment to constant variable.

2.const与let的作用域一样，只在声明所在的块级作用域内有效。

	if (true) {
	  const MAX = 5;
	}
	MAX // Uncaught ReferenceError: MAX is not defined	

3.const同样存在暂时性死区，只能在声明的位置后面使用。	

4.const声明的常量，也与let一样不可重复声明。

5.const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了。


#### ES6声明对象的六种方法

ES5有:var,function两种方法

ES6有:var,function,let,const,import,class六种方法

#### 顶层对象的属性

ES5之中，顶层对象的属性与全局变量是等价的。

	window.a = 1;
	a // 1

	a = 2;
	window.a // 2

顶层对象的属性与全局变量挂钩，被认为是JavaScript语言最大的设计败笔之一。这样的设计带来了几个很大的问题，首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道（因为全局变量可能是顶层对象的属性创造的，而属性的创造是动态的）；其次，程序员很容易不知不觉地就创建了全局变量（比如打字出错）；最后，顶层对象的属性是到处可以读写的，这非常不利于模块化编程。另一方面，window对象有实体含义，指的是浏览器的窗口对象，顶层对象是一个有实体含义的对象，也是不合适的。	

ES6为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩。

	var a = 1;
	// 如果在Node的REPL环境，可以写成global.a
	// 或者采用通用方法，写成this.a
	window.a // 1

	let b = 1;
	window.b // undefined

全局变量a由var命令声明，所以它是顶层对象的属性；  
全局变量b由let命令声明，所以它不是顶层对象的属性，返回undefined。


## 2.变量的结构赋值

#### 数组的解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

	let [a, b, c] = [1, 2, 3];

从数组中提取值，按照对应位置，对变量赋值。如果解构不成功，变量的值就等于undefined。如果等号的右边不是数组（或者严格地说，不是可遍历的结构），那么将会报错。

解构赋值允许指定默认值。（如果一个数组成员不严格等于undefined，默认值是不会生效的。）

	let [foo = true] = [];
	foo // true
	let [x, y = 'b'] = ['a']; // x='a', y='b'
	let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'

如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。	

#### 对象的解构赋值

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

	let { bar, foo } = { foo: "aaa", bar: "bbb" };
	foo // "aaa"
	bar // "bbb"

	let { baz } = { foo: "aaa", bar: "bbb" };
	baz // undefined

对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。	

#### 字符串的解构赋值

字符串被转换成了一个类似数组的对象。

	let {length : len} = 'hello';
	len // 5

解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。  
由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。	

#### 圆括号的问题

ES6 的规则是，只要有可能导致解构的歧义，就不得使用圆括号。因此，建议只要有可能，就不要在模式中放置圆括号。

不可以使用圆括号的情况：

1.变量声明语句
2.函数参数
3.赋值语句的模式

可以使用圆括号的情况：

只有一种：赋值语句的非模式部分，可以使用圆括号。

	[(b)] = [3]; // 正确
	({ p: (d) } = {}); // 正确
	[(parseInt.prop)] = [3]; // 正确

#### 变量解构赋值的用途

1.交换变量的值

	let x = 1;
	let y = 2;
	[x, y] = [y, x];

2.从函数返回多个值

	// 返回一个数组
	function example() {
	  return [1, 2, 3];
	}
	let [a, b, c] = example();

	// 返回一个对象
	function example() {
	  return {
	    foo: 1,
	    bar: 2
	  };
	}
	let { foo, bar } = example();

3.函数参数的定义

	// 参数是一组有次序的值
	function f([x, y, z]) { ... }
	f([1, 2, 3]);

	// 参数是一组无次序的值
	function f({x, y, z}) { ... }
	f({z: 3, y: 2, x: 1});

4.提取JSON数据

	let jsonData = {
	  id: 42,
	  status: "OK",
	  data: [867, 5309]
	};

	let { id, status, data: number } = jsonData;

	console.log(id, status, number);
	// 42, "OK", [867, 5309]

5.函数参数的默认值

	jQuery.ajax = function (url, {
	  async = true,
	  beforeSend = function () {},
	  cache = true,
	  complete = function () {},
	  crossDomain = false,
	  global = true,
	  // ... more config
	}) {
	  // ... do stuff
	};

6.遍历Map结构
	
	任何部署了Iterator接口的对象，都可以用for...of循环遍历。
	const map = new Map();
	map.set('first', 'hello');
	map.set('second', 'world');

	for (let [key, value] of map) {
	  console.log(key + " is " + value);
	}
	// first is hello
	// second is world


7.输入模块的指定方法

	const { SourceMapConsumer, SourceNode } = require("source-map");


## 3.字符串的扩展

codePointAt()  ---返回一个字符的码点。
String.fromCodePoint() 	 ---从码点返回对应字符。
字符串的遍历器接口   ---字符串可以被for...of循环遍历。
normalize()   ---将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化。
includes(), startsWith(), endsWith()
repeat(n)   ---返回一个新字符串，表示将原字符串重复n次。
padStart()，padEnd()    ---头部补全，尾部补全
模板字符串   ---使用
`In JavaScript '\n' is a line-feed.`
模板字符串中嵌入变量，需要将变量名写在${}之中。使用<%...%>放置JavaScript代码，使用<%= ... %>输出JavaScript表达式。
标签模板   
String.raw()   ---模板字符串的处理函数，返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串


## 4.正则的扩展

#### 1.RegExp构造函数

在ES5中RegExp构造函数的参数有两种表现形式：

	var regex = new RegExp('xyz', 'i');
	var regex = new RegExp(/xyz/i);

在第二种参数为正则表达式时，不允许有第二个参数。

但是在ES6中，如果RegExp构造函数第一个参数是一个正则对象，那么可以使用第二个参数指定修饰符。并且会忽略第一个参数正则里面的修饰符，只使用新的修饰符。

	new RegExp(/abc/ig, 'i').flags
	// "i"


#### 2.字符串的正则方法

match()
replace()
search()
split()

#### 3.u修饰符

ES6 对正则表达式*添加了u修饰符*，含义为“Unicode模式”，用来正确处理大于\uFFFF的 Unicode 字符。也就是说，会正确处理四个字节的 UTF-16 编码。

1.点（.）字符在正则表达式中，含义是除了换行符以外的任意单个字符。对于码点大于0xFFFF的 Unicode 字符，点字符不能识别，必须加上u修饰符。

	var s = '𠮷';
	/^.$/.test(s) // false
	/^.$/u.test(s) // true

2.Unicode 字符表示法：ES6 新增了使用大括号表示 Unicode 字符，这种表示法在正则表达式中必须加上u修饰符，才能识别当中的大括号，否则会被解读为量词。

	/\u{61}/.test('a') // false
	/\u{61}/u.test('a') // true
	/\u{20BB7}/u.test('𠮷') // true

3.量词:使用u修饰符后，所有量词都会正确识别码点大于0xFFFF的 Unicode 字符。

	/a{2}/.test('aa') // true
	/a{2}/u.test('aa') // true
	/𠮷{2}/.test('𠮷𠮷') // false
	/𠮷{2}/u.test('𠮷𠮷') // true

4.预定义模式

	/^\S$/.test('𠮷') // false
	/^\S$/u.test('𠮷') // true

上面代码的\S是预定义模式，匹配所有不是空格的字符。只有加了u修饰符，它才能正确匹配码点大于0xFFFF的 Unicode 字符。

5.i 修饰符:有些 Unicode 字符的编码不同，但是字型很相近，比如，\u004B与\u212A都是大写的K。

	/[a-z]/i.test('\u212A') // false
	/[a-z]/iu.test('\u212A') // true

上面代码中，不加u修饰符，就无法识别非规范的K字符。
	

#### y修饰符---“粘连”（sticky）修饰符

y修饰符的作用与g修饰符类似，也是全局匹配，后一次匹配都从上一次匹配成功的下一个位置开始。不同之处在于，g修饰符只要剩余位置中存在匹配就可，而y修饰符确保匹配必须从剩余的第一个位置开始，这也就是“粘连”的涵义。

y修饰符的设计本意，就是让头部匹配的标志^在全局匹配中都有效。

单单一个y修饰符对match方法，只能返回第一个匹配，必须与g修饰符联用，才能返回所有匹配。

	'a1a2a3'.match(/a\d/y) // ["a1"]
	'a1a2a3'.match(/a\d/gy) // ["a1", "a2", "a3"]


## 5.数值的扩展

#### 1.二进制和八进制表示法：

ES6 提供了二进制和八进制数值的新的写法，分别用前缀0b（或0B）和0o（或0O）表示。

如果要将0b和0o前缀的字符串数值转为十进制，要使用Number方法。

	Number('0b111')  // 7
	Number('0o10')  // 8

#### 2.Number.isFinite(), Number.isNaN()

#### 3.Number.parseInt(), Number.parseFloat()

	// ES5的写法
	parseInt('12.34') // 12
	parseFloat('123.45#') // 123.45
	// ES6的写法
	Number.parseInt('12.34') // 12
	Number.parseFloat('123.45#') // 123.45

#### 4.Number.isInteger()判断一个值是否为整数
#### 5.Number.EPSILON

ES6在Number对象上面，新增一个极小的常量Number.EPSILON。

	Number.EPSILON
	// 2.220446049250313e-16
	Number.EPSILON.toFixed(20)
	// '0.00000000000000022204'	

Number.EPSILON的实质是一个可以接受的误差范围。

	function withinErrorMargin (left, right) {
	  return Math.abs(left - right) < Number.EPSILON;
	}
	withinErrorMargin(0.1 + 0.2, 0.3)
	// true
	withinErrorMargin(0.2 + 0.2, 0.3)
	// false	

上面的代码为浮点数运算，部署了一个误差检查函数。	

#### 6.Math对象的扩展

Math.trunc()去除一个数的小数部分，返回整数部分
Math.sign()判断一个数是正数(返回+1)、负数(返回-1)、还是0(返回0)，非数值转换为数值，无法转换为数值返回NaN。
Math.cbrt()计算一个数的立方根
Math.clz32()返回一个数的32位无符号整数形式有多少个前导0,如果是小数的话，则只考虑整数部分。
Math.imul()返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32位的带符号整数。
Math.fround()返回一个数的单精度浮点数形式。
Math.hyot()返回所有参数的平方之和的平方根。如果参数不是数值，Math.hypot方法会将其转为数值。只要有一个参数无法转为数值，就会返回NaN。

对数方法：
Math.expm1()返回ex - 1，即Math.exp(x) - 1（e的x次方减1）.
Math.log1p()返回1 + x的自然对数，即Math.log(1 + x)。如果x小于-1，返回NaN。
Math.log10()返回以10为底的x的对数。如果x小于0，则返回NaN。
Math.log2()返回以2为底的x的对数。如果x小于0，则返回NaN。

双曲函数方法：
Math.sinh(x)返回x的双曲正弦
Math.cosh(x)返回x的双曲余弦
Math.tanh(x)返回x的双曲正切
Math.asinh(x)返回x的反双曲正弦
Math.acosh(x)返回x的反双曲余弦
Math.atanh(x)返回x的反双曲正切

#### 7.Math.signbit()判断值是否是负值（包括-0，判断符号位）

如果参数是NaN，返回false
如果参数是-0，返回true
如果参数是负值，返回true
其他情况返回false

#### 8.指数运算符**

ES6新增的指数运算符：

	a ** b  //a的b次幂	

注：在V8引擎中，指数运算符与Math.pow的实现不相同，对于特别大的运算结果，两者会有细微的差异。	

	Math.pow(99,99)
	//3.697296376497263e+197
	99 ** 99
	//3.697296376497268e+197

#### 9.integer数据类型

JavaScript 所有数字都保存成64位浮点数，这决定了整数的精确程度只能到53个二进制位。大于这个范围的整数，JavaScript 是无法精确表示的，这使得 JavaScript 不适合进行科学和金融方面的精确计算。

为了与 Number 类型区别，Integer 类型的数据必须使用后缀n表示。


## 6.函数的扩展

#### 1.函数参数的默认值

在ES6以前是不能给参数指定默认值的，但在ES6可以，即直接写着参数定义的后面。  
参数变量是默认声明的，所以不能用let或const再次声明。  
参数不能有同名参数出现

	function(x,y = 'world'){
		console.log(x,y);
	}

优点：
1.可以明显得知那些参数是可以省略的
2.利于代码的优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。

#### 2.函数的length属性

arguments.length是函数实际接收到的参数个数
Function.length 是函数形参的个数，即期望得到的参数的个数

当函数制定了默认值，函数的length属性将失真，即不包含默认值的参数（也不包含设置默认值参数之后的参数）

	(function (a) {}).length // 1
	(function (a = 5) {}).length // 0
	(function (a, b, c = 5) {}).length // 2

如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了。

	(function (a = 0, b, c) {}).length // 0
	(function (a, b = 1, c) {}).length // 1


#### 3.rest参数

用于获取函数的多余参数（values是一个数组）

	形式为  ...变量名
	function add(...values){}

栗子：

	function add(...values) {
        let sum = 0;

        for(var val of values){
            sum += val;
        }
        return sum;
    }

    add(1,2,3,4,5);  //15

注意，rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。

函数的length属性，不包括 rest 参数。

	(function(a) {}).length  // 1
	(function(...a) {}).length  // 0
	(function(a, ...b) {}).length  // 1

函数的arguments对象不是数组，而是一个类似数组的对象。所以为了使用数组的方法，必须使用Array.prototype.slice.call先将其转为数组。rest 参数就不存在这个问题，它就是一个真正的数组，数组特有的方法都可以使用。

	function push(array, ...items) {
	  items.forEach(function(item) {
	    array.push(item);
	    console.log(item);
	  });
	}
	var a = [];
	push(a, 1, 2, 3)

#### 4.严格模式

从 ES5 开始，函数内部可以设定为严格模式。

ES2016 做了一点修改，规定只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。

两种方法可以规避这种情况：

1.设定全局性的严格模式，这是合法的。

	'use strict';

	function doSomething(a, b = a) {
	  // code
	}

2.把函数包在一个无参数的立即执行函数里面。

	const doSomething = (function () {
	  'use strict';
	  return function(value = 42) {
	    return value;
	  };
	}());

#### 5.name属性

返回函数的函数名

	function foo() {}
	foo.name // "foo"

匿名函数在ES5和ES6中返回结果不一样，ES5返回空，ES6返回函数名

	var f = function () {};
	// ES5
	f.name // ""
	// ES6
	f.name // "f"	

构造函数的name返回值为：anonymous

	(new Function).name // "anonymous"

bind返回的函数，name属性值会加上bound前缀。

	function foo() {};
	foo.bind({}).name // "bound foo"

	(function(){}).bind({}).name // "bound "	


#### 6.箭头函数

ES6允许使用“箭头”（=>）定义函数。

	var sum = (num1, num2) => { return num1 + num2; }

(num1, num2)是参数部分，	{ return num1 + num2; }是函数体

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。

	var f = () => 5;
	// 等同于
	var f = function () { return 5 };

由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

	// 报错
	let getTempItem = id => { id: id, name: "Temp" };

	// 不报错
	let getTempItem = id => ({ id: id, name: "Temp" });	


箭头函数的一个用处是简化回调函数。

	// 正常函数写法
	[1,2,3].map(function (x) {
	  return x * x;
	});

	// 箭头函数写法
	[1,2,3].map(x => x * x);

rest 参数与箭头函数结合的例子：

	const numbers = (...nums) => nums;

	numbers(1, 2, 3, 4, 5)
	// [1,2,3,4,5]

	const headAndTail = (head, ...tail) => [head, tail];

	headAndTail(1, 2, 3, 4, 5)
	// [1,[2,3,4,5]]	

箭头函数的注意点（箭头函数没有自己的this）：

1.函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
2.不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
3.不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
4.不可以使用yield命令，因此箭头函数不能用作 Generator 函数。	


#### 7.绑定this

函数绑定运算符::，运算符左边是对象，右边是函数，将对象绑定到函数的this对象上。

	foo::bar;
	// 等同于
	bar.bind(foo);

	foo::bar(...arguments);
	// 等同于
	bar.apply(foo, arguments);

	const hasOwnProperty = Object.prototype.hasOwnProperty;
	function hasOwn(obj, key) {
	  return obj::hasOwnProperty(key);
	}

如果双冒号左边为空，右边是一个对象的方法，则等于将该方法绑定在该对象上面。

	var method = obj::obj.foo;
	// 等同于
	var method = ::obj.foo;

	let log = ::console.log;
	// 等同于
	var log = console.log.bind(console);

由于双冒号运算符返回的还是原对象，因此可以采用链式写法。

	// 例一
	import { map, takeWhile, forEach } from "iterlib";

	getPlayers()
	::map(x => x.character())
	::takeWhile(x => x.strength > 100)
	::forEach(x => console.log(x));

	// 例二
	let { find, html } = jake;

	document.querySelectorAll("div.myClass")
	::find("p")
	::html("hahaha");

#### 8.尾调用优化

尾调用就是函数的最后一步是调用另一个函数。

“尾调用优化”（Tail call optimization），即只保留内层函数的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。

注意，只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”。

	function f() {
	  let m = 1;
	  let n = 2;
	  return g(m + n);
	}
	f();

	// 等同于
	function f() {
	  return g(3);
	}
	f();

	// 等同于
	g(3);	

尾递归：尾调用自身的情况

递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。

	function factorial(n) {
	  if (n === 1) return 1;
	  return n * factorial(n - 1);
	}

	factorial(5) // 120	

上面代码是一个阶乘函数，计算n的阶乘，最多需要保存n个调用记录，复杂度 O(n) 。	

	function factorial(n, total) {
	  if (n === 1) return total;
	  return factorial(n - 1, n * total);
	}

	factorial(5, 1) // 120

如果改写成尾递归，只保留一个调用记录，复杂度 O(1) 。	

非尾递归的 Fibonacci 数列实现如下。

	function Fibonacci (n) {
	  if ( n <= 1 ) {return 1};

	  return Fibonacci(n - 1) + Fibonacci(n - 2);
	}

	Fibonacci(10) // 89
	Fibonacci(100) // 堆栈溢出
	Fibonacci(500) // 堆栈溢出

尾递归优化过的 Fibonacci 数列实现如下。

	function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
	  if( n <= 1 ) {return ac2};

	  return Fibonacci2 (n - 1, ac2, ac1 + ac2);
	}

	Fibonacci2(100) // 573147844013817200000
	Fibonacci2(1000) // 7.0330367711422765e+208
	Fibonacci2(10000) // Infinity

*ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。*

## 7.数组的扩展

#### 1.扩展运算符spread（...）

它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。主要用于函数的调用

	function push(array, ...items) {
	  array.push(...items);
	}

	function add(x, y) {
	  return x + y;
	}

	const numbers = [4, 38];
	add(...numbers) // 42

扩展运算符与正常的函数参数可以结合使用，非常灵活。

	function f(v, w, x, y, z) { }
	const args = [0, 1];
	f(-1, ...args, 2, ...[3]);

扩展运算符可以替换apply方法（把数组转换为函数的参数）：

	// ES5 的写法
	Math.max.apply(null, [14, 3, 77])

	// ES6 的写法
	Math.max(...[14, 3, 77])

	// 等同于
	Math.max(14, 3, 77);

上面代码中，由于 JavaScript 不提供求数组最大元素的函数，所以只能套用Math.max函数，将数组转为一个参数序列，然后求最大值。有了扩展运算符以后，就可以直接用Math.max了。

扩展运算符的应用：

1.复制数组

对于ES5，一维数组可以用slice或者concat实现深拷贝。

	const a1 = [1, 2];
	const a2 = a1.concat();

	a2[0] = 2;
	a1 // [1, 2]

用扩展运算符...实现：

	const a1 = [1,2,3];
    const a2 = [...a1];

2.合并数组

	// ES5
	[1, 2].concat(more)
	// ES6
	[1, 2, ...more]

3.与解构赋值结合

4.将字符串转换为数组

	[...'hello']
	// [ "h", "e", "l", "l", "o" ]

5.实现了 Iterator 接口的对象

任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组。

	let nodeList = document.querySelectorAll('div');
	let array = [...nodeList];


#### 2.Array.from

将类数组对象和可遍历的对象转换为数组

	Array.from('hello')
	// ['h', 'e', 'l', 'l', 'o']

	let namesSet = new Set(['a', 'b'])
	Array.from(namesSet) // ['a', 'b']	

对于还没有部署该方法的浏览器，可以用Array.prototype.slice方法替代。

Array.from的第一个参数指定了第二个参数运行的次数。这种特性可以让该方法的用法变得非常灵活。

	Array.from({ length: 2 }, () => 'jack')
	// ['jack', 'jack']	

#### 3.Array.of

将一组值，转换为数组。	

	Array.of(3, 11, 8) // [3,11,8]
	Array.of(3) // [3]
	Array.of(3).length // 1

#### 4.数组实例的copyWithin()

数组实例的copyWithin方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

	[1, 2, 3, 4, 5].copyWithin(0, 3)
	// [4, 5, 3, 4, 5]

语法：

	Array.prototype.copyWithin(target, start = 0, end = this.length)

它接受三个参数。

target（必需）：从该位置开始替换数据。
start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。
end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。		


#### 5.数组实例的find()和findIndex()

数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。

数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

	[1, 4, -5, 10].find((n) => n < 0)
	// -5

	[1, 5, 10, 15].findIndex(function(value, index, arr) {
	  return value > 9;
	}) // 2

这两个方法都可以发现NaN，弥补了数组的*IndexOf*方法的不足。

	[NaN].indexOf(NaN)
	// -1
	[NaN].findIndex(y => Object.is(NaN, y))
	// 0	

#### 6.数组实例的fill()	

fill方法使用给定值，填充一个数组。

fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

	['a', 'b', 'c'].fill(7, 1, 2)
	// ['a', 7, 'c']

#### 7.数组实例的	entries(),keys(),values()

ES6 提供三个新的方法——entries()，keys()和values()——用于遍历数组。它们都返回一个遍历器对象，可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。

	for (let index of ['a', 'b'].keys()) {
	  console.log(index);
	}
	// 0
	// 1

	for (let elem of ['a', 'b'].values()) {
	  console.log(elem);
	}
	// 'a'
	// 'b'

	for (let [index, elem] of ['a', 'b'].entries()) {
	  console.log(index, elem);
	}
	// 0 "a"
	// 1 "b"

#### 8.数组实例的	includes()

Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。ES2016 引入了该方法。

	[1, 2, 3].includes(2)     // true
	[1, 2, 3].includes(4)     // false
	[1, 2, NaN].includes(NaN) // true

#### 9.数组的空位

数组的空位指，数组的某一个位置没有任何值。比如，Array构造函数返回的数组都是空位。

	Array(3) // [, , ,]

由于空位的处理规则非常不统一，所以建议避免出现空位。


## 8.对象的扩展	

#### 1.属性的简洁表示法

ES6 允许在对象之中，直接写变量。这时，属性名为变量名, 属性值为变量的值。

	function f(x, y) {
	  return {x, y};
	}

	// 等同于

	function f(x, y) {
	  return {x: x, y: y};
	}

	f(1, 2) // Object {x: 1, y: 2}

方法的简写：

	let birth = '2000/01/01';

	const Person = {

	  name: '张三',

	  //等同于birth: birth
	  birth,

	  // 等同于hello: function ()...
	  hello() { console.log('我的名字是', this.name); }

	};	

#### 2.属性名表达式

ES6允许用表达式定义对象的属性名。

	let propKey = 'foo';
	let obj = {
	  [propKey]: true,
	  ['a' + 'bc']: 123
	};

S6允许用表达式定义对象的方法名：

	let obj = {
	  ['h' + 'ello']() {
	    return 'hi';
	  }
	};

	obj.hello() // hi	

注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串[object Object]，这一点要特别小心。

	const keyA = {a: 1};
	const keyB = {b: 2};

	const myObject = {
	  [keyA]: 'valueA',
	  [keyB]: 'valueB'
	};

	myObject // Object {[object Object]: "valueB"}	

上面代码中，[keyA]和[keyB]得到的都是[object Object]，所以[keyB]会把[keyA]覆盖掉，而myObject最后只有一个[object Object]属性。


#### 3.方法的name属性

函数的name属性，返回函数名。对象方法也是函数，因此也有name属性。

	const person = {
	  sayName() {
	    console.log('hello!');
	  },
	};

	person.sayName.name   // "sayName"

如果对象的方法使用了取值函数（getter）和存值函数（setter），则name属性不是在该方法上面，而是该方法的属性的描述对象的get和set属性上面，返回值是方法名前加上get和set。

有两种特殊情况：bind方法创造的函数，name属性返回bound加上原函数的名字；Function构造函数创造的函数，name属性返回anonymous。

	(new Function()).name // "anonymous"

	var doSomething = function() {
	  // ...
	};
	doSomething.bind().name // "bound doSomething"


#### 4.Object.is()比较两个值是否严格相等

它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

它们的不同是(+0与-0，NaN与NaN)：

	+0 === -0     //true
	NaN === NaN   //false

	Object.is(+0,-0)     //false
	Object.is(NaN,NaN)   //true
	 
#### 5.Object.assign()用于对象的合并

	const target = { a: 1 };

	const source1 = { b: 2 };
	const source2 = { c: 3 };

	Object.assign(target, source1, source2);
	target // {a:1, b:2, c:3}

注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

如果该参数不是对象，则会先转成对象，然后返回。

由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。

Object.assign拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）。

用途：

1.为对象添加属性或方法
2.克隆对象（浅拷贝）
3.合并多个对象

	const merge =(...sources) => Object.assign({}, ...sources);

4.为属性指定默认值

#### 6.属性的可枚举性和遍历

Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象。

	let obj = { foo: 123 };
	Object.getOwnPropertyDescriptor(obj, 'foo')
	//configurable:true
	//enumerable:true
	//value:123
	//writable:true

其中的enumerable枚举属性，如果为false，那么for...in,Object.keys(),JSON.stringfy(),Object.assign()这些操作都会忽略该属性。
（for...in会遍历继承的属性，而Object.keys()则不会，因此当我们只关心对象自身的属性时，最好不要用for...in,而选用Object.keys()）.

#### 7.Object.getOwnPropertyDescriptors()

返回某个对象属性的描述对象（descriptor）,即返回某个对象的自身属性，而不包含继承属性

#### 8.__proto__ 属性，Object.setPrototypeOf()，Object.getPrototypeOf()

1.__proto__ 属性用来读取和设置当前对象的prototype属性。

一个属性的__proto__ 值表示这个对象的原型。ES6不建议用这个方法。

2.Object.setPrototypeOf()

设置对象的prototype，ES6建议用Object.setPropertyOf().返回对象本身。

	let proto = {};
	let obj = { x: 10 };
	Object.setPrototypeOf(obj, proto);

	proto.y = 20;
	proto.z = 40;

	obj.x // 10
	obj.y // 20
	obj.z // 40

3.Object.getPrototypeOf()

用于读取一个对象的原型

#### 9.super关键字

super关键字指向当前对象的原型对象。只能用在对象的方法里面，用在其他地方会报错。

#### 10.Object.keys(),Object.values(),Object.entries()

Object.keys()   	遍历对象的所有（enumerable为true，不是继承的）键名
Object.values()  	遍历对象的所有（enumerable为true，不是继承的）键值
Object.entries()    遍历对象的所有（enumerable为true，不是继承的）键值对


## 10.symbol

ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

	let s = Symbol();

	typeof s
	// "symbol"

Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

Symbol值不能与其他类型的值进行运算，否则会报错。

由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖。

注意，Symbol 值作为对象属性名时，不能用点运算符。  
在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。  
Symbol 值作为属性名时，该属性还是公开属性，不是私有属性。


## 11.Set和Map数据结构

#### 1.Set

ES6提供新的数据结构SET。它类似于数组，但是其内部成员的值都是唯一的，没有重复值(判断的方法类似于全等判断)。

	const s = new Set();
	[2,3,5,4,5,2,2].forEach(x => s.add(x));

	for(let i of s){
	    console.log(i);
	}

返回结果是：2 3 5 4。结果表明 Set 结构不会添加重复的值。

Set函数可以接受一个数组作为参数来初始化：

	const set = new Set([1, 2, 3, 4, 4]);
	[...set]
	// [1, 2, 3, 4]

	const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
	items.size // 5	
	items.length //undefined

利用Set的成员唯一性，可以用来数组去重：

	let myArray = [1,2,3,4,3,2,4,1];
	let arr = [...new Set(myArray)];
	console.log(arr);
	//[1, 2, 3, 4]

#### 2.Set实例的属性和方法

操作方法：

1.add(value)     添加某个值，返回Set结构本身
2.delete(value)  删除某个值，返回布尔值，是否删除成功
3.has(value)     判断某个值是不是Set成员，返回布尔值
4.clear()        清空Set成员，没有返回值

	s.add(1).add(2).add(2);
	// 注意2被加入了两次

	s.size // 2

	s.has(1) // true
	s.has(2) // true
	s.has(3) // false

	s.delete(2);
	s.has(2) // false

Array.from方法可以将 Set 结构转为数组。

	const items = new Set([1, 2, 3, 4, 5]);
	const array = Array.from(items);

这样数组去重就可以这样来写：

	Array.from(new Set(array))

遍历Set：

1.keys()      返回键名
2.values()    返回键值
3.entries()   返回键值对
4.foreach()   使用回调函数遍历

#### 3.WeakSet

与Set差不多，都是不重复值的集合，但是*WeakSet的值必须是对象*，不能是其他类型的值。

语法：

	const ws = new WeakSet()

作为构造函数，WeakSet 可以接受一个数组或类似数组的对象作为参数。这个类数组对象的成员必须是对象。否则会报错

	const a = [[1, 2], [3, 4]];
	const ws = new WeakSet(a);
	// WeakSet {[1, 2], [3, 4]}
	const b = [3, 4];
	const ws = new WeakSet(b);
	// Uncaught TypeError: Invalid value used in weak set(…)

WeakSet的方法：

1.WeakSet.prototype.add(value)
2.WeakSet.prototype.delete(value)
3.WeakSet.prototype.has(value)

他没有size属性,不能遍历它的成员

#### 4.Map

Object 结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应。即对象的属性名（键）不只是字符串形式了，还可以是各种类型的值或对象。

	const m = new Map();
	const o = {p: 'Hello World'};

	m.set(o, 'content')
	m.get(o) // "content"

	m.has(o) // true
	m.delete(o) // true
	m.has(o) // false

作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

	const map = new Map([
	  ['name', '张三'],
	  ['title', 'Author']
	]);

	map.size // 2
	map.has('name') // true
	map.get('name') // "张三"
	map.has('title') // true
	map.get('title') // "Author"


#### 5.WeakMap

WeakMap结构与Map结构类似，也是用于生成键值对的集合。

首先，WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。

其次，WeakMap的键名所指向的对象，不计入垃圾回收机制。

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。

一个典型应用场景是，在网页的 DOM 元素上添加数据，就可以使用WeakMap结构。当该 DOM 元素被清除，其所对应的WeakMap记录就会自动被移除。

	const wm = new WeakMap();

	const element = document.getElementById('example');

	wm.set(element, 'some information');
	wm.get(element) // "some information"

总之，WeakMap的专用场合就是，它的键所对应的对象，可能会在将来消失。WeakMap结构有助于防止内存泄漏。

注意，WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。

	const wm = new WeakMap();
	let key = {};
	let obj = {foo: 1};

	wm.set(key, obj);
	obj = null;
	wm.get(key)
	// Object {foo: 1}

上面代码中，键值obj是正常引用。所以，即使在 WeakMap 外部消除了obj的引用，WeakMap 内部的引用依然存在。

WeakMap规定不能取到键名,无法清空.即不支持clear方法。因此，WeakMap只有四个方法可用：get()、set()、has()、delete()。

	const wm = new WeakMap();

	// size、forEach、clear 方法都不存在
	wm.size // undefined
	wm.forEach // undefined
	wm.clear // undefined


## 12.Proxy

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。

## 13.Reflect

Reflect对象与Proxy对象一样，也是 ES6 为了操作对象而提供的新 API。Reflect对象的设计目的有这样几个。

（1） 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，从Reflect对象上可以拿到语言内部的方法。

（2） 修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。

（3） 让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。

（4）Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。

## 14.Promise对象

一种异步解决方案。从Promise对象，可以获取异步操作的信息。

简单来说Promise是一个容器，它包含着一个未来才会结束的事件（通常是一个异步操作）

Promise分为三个状态：pending-进行中,fulfilled-已成功,rejected-已失败.

Promise的特点：

1.对象的状态不受外部影响。只有异步操作的结果可以影响Promise状态。任何其他操作都无法改变这个状态。

2.一旦状态改变就不会再次改变，这时就称为 resolved（已定型）。任何时候都能得到异步结果（状态）。Promise状态的改变，只有可能有两种：pending -> fulfilled  或者  pending -> rejected.

Promise的缺点：

1.无法取消Promise。一旦新建就会立即执行，无法中途取消。

2.如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。

3.当处于pending状态时，无法得知目前进展到什么阶段（是刚刚开始海试即将完成）。

#### Promise基本用法

Promise对象是一个构造函数。

	var promise = new Promise(function(resolve,reject){
	    //some coding ....

	    //如果异步操作成功resolve(value)，否则reject(err)
	    if(win){
	        resolve(value);
	    }else{
	        reject(err);
	    }
	})

其中的回调函数中的两个参数resolve和reject分别是两个函数，它们由javascript引擎提供，不需要自己部署。

resolve函数的作用：将Promise对象的状态由pending变为fulfilled，并且在异步操作成功时，将异步操作的结果value，作为参数传递出去。

reject函数的作用：将Promise对象的状态由pending变为rejected。并且在异步操作失败时，将异步操作报出的错误作为参数传递出去。














