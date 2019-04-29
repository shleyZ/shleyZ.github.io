---
title: js数据结构-字典
date: 2018-04-24 10:19:00
categories: 数据结构
---

#### 字典是一种键值对形式存储的数据结构。

javascript中的Object就是由字典形式设计的


#### 字典常见的属性或者方法：

| 类型   |      描述      |
|----------|:-------------:|
| add |  添加元素 |
| find |  查找一个元素  |
| remove | 删除一个元素 |
| showAll | 显示字典元素 |
| count | 字典元素个数 |
| clear | 清空字典元素  |


#### 字典的实现

``` js

    var Dictionary = function() {
      this.dataStore = []; //初始化为空
    }

    Dictionary.prototype = (function () {
      return {
        add: add,
        find: find,
        remove: remove,
        showAll: showAll,
        count: count,
        clear: clear,
      }

      // 添加元素
      function add(key, value) {
        this.dataStore[key] = value
      }
      // 查找字典中的元素
      function find(key) {
        return this.dataStore[key]
      }

      // 删除一个元素
      function remove(key){
        if(this.dataStore[key]) {
          delete this.dataStore[key]
        } else {
          return 'Not Found'
        }
      }

      // 显示所有字典元素
      function showAll(){
        for(var key in this.dataStore){
          console.log(key + '-->' + this.dataStore[key])
        }
      }

      // 字典元素个数
      function count(){
        var count = 0;
        for(var key in this.dataStore){
          count += 1;
        }
        return count
      }

      // 清空字典元素
      function clear() {
        for(var key in this.dataStore){
          delete this.dataStore[key]
        }
      }
      
    }())

```    

操作：

``` js

    var dict = new Dictionary();

    dict.add('name', 'cat')
    dict.add('name1', 'dog')
    dict.add('name2', 'bird')
    dict.add('name3', 'fish')

    console.log(dict.showAll())
    // name-->cat
    // name1-->dog
    // name2-->bird 
    // name3-->fish

    console.log(dict.find('name'))  // cat

    dict.remove('name3')

    console.log(dict.showAll())
    // name-->cat
    // name1-->dog
    // name2-->bird 

    console.log(dict.count())  // 3

    dict.clear()

    console.log(dict.count()) // 0

```