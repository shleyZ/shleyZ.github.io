---
title: js数据结构-栈
date: 2018-04-21 13:37:35
categories: 数据结构
---

#### 上一篇的列表对数据的存储顺序不要求，而这次的栈和列表类似，但是比列表更高效。

+ 栈，又称堆栈，栈内的元素只能通过数据的一端访问，称为栈顶，数据只能在栈顶添加或删除，遵循先入后出的原则（last-in-first-out）

+ 栈的主要操作：push--元素入栈， pop--元素出栈


#### 栈常见的属性或者方法：

| 类型   |      描述      |
|----------|:-------------:|
| push |  新元素入栈 |
| pop |    顶部元素出栈   |
| top | 查看当前栈顶位置 |
| peek | 查看当前栈顶元素 |
| length | 查看当前栈元素总数 |
| clear | 清空栈内元素  |


#### 栈的实现

``` js

    var Stack = function() {
      this.top = 0;   //记录栈顶位置
      this.dataStore = []; //初始化为空
    }

    Stack.prototype = (function () {
      return {
        length: length,
        clear: clear,
        pop: pop,
        push: push,
        peek: peek,
      }

      // 返回栈元素的个数
      function length() {
        return this.top;
      }
      // 清空栈的所有元素
      function clear() {
        delete this.dataStore;
        this.top = 0;
        this.dataStore = [];
      }

      // 该方法将一个新元素入栈，放到数组中 top 所对应的位置上，并将 top 的值加 1，让其指向数组的下一个空位置
      function push(ele){
        this.dataStore[this.top++] = ele;
      }

      // 该方法与入栈相反，返回栈顶元素，并将 top 的值减 1
      function pop(){
        return this.dataStore[--this.top];
      }

      // 该方法返回的是栈顶元素，即 top - 1 个位置元素
      function peek(){
        if(this.top > 0) {
          return this.dataStore[this.top - 1]
        } else {
          return 'empty'
        }
      }
      
    }())

```

操作：

``` js

    var stack = new Stack()
    console.log(stack.peek())  // empty
    stack.push('one')
    stack.push('two')
    console.log(stack.peek())  // two
    stack.pop()
    console.log(stack.peek())  // one
    stack.length()             // 1
    stack.clear()
    console.log(stack.peek())  // empty

```

####  栈的应用


判断回文字符串，即反转后的字符串和原字符串一样

``` js

    function isPalindrome(word) {
      var s = new Stack()
      for (var i = 0; i < word.length; i++) {
          s.push(word[i])
      }
      var rword = "";
      while (s.length() > 0) {
          rword += s.pop();
      }
      if (word == rword) {
          return true;
      } else {
          return false;
      }
    }

```

实现一个阶乘算法 5! = 5 * 4 * 3 * 2 * 1 = 120

``` js
    function fact(n) {
      var s = new Stack()
      while (n > 1) {
          //[5,4,3,2]
          s.push(n--);
      }
      var product = 1;
      while (s.length() > 0) {
          product *= s.pop();
      }
      return product;
    }

    fact(6)  // 720
```    