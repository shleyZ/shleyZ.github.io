---
title: 列表
date: 2018-04-21 11:16:49
categories: 数据结构
---


#### 列表是一组有序的数据，每个列表中的数据被称为一个元素，元素可以是任意类型，列表可以保存多少数据没有限制，但是会受内存的限制。

#### 列表中常见的属性或者方法：

| 类型   |      描述      |
|----------|:-------------:|
| listSize |  列表元素个数 |
| pos |    当前位置   |
| length | 返回列表中元素的个数 |
| clear | 清除列表中所有元素 |
| toString | 返回列表的字符串形式 |
| getElement | 返回当前位置的元素  |
| insert | 在现有元素后插入新元素 |
| append | 在列表的末尾添加新元素 |
| remove | 在列表中删除元素 |
| front | 将列表的当前位置移动到第一个元素位置 |
| end | 将列表的当前位置移动到最后一个元素位置  |
| prev | 将当前位置后移一位 |
| next | 将当前位置前移一位 |
| hasNext | 判断是否还有下一位 |
| hasPrev | 判断是否还有上一位 |
| currPos | 返回列表的当前位置 |
| moveTo | 将当前位置移动到指定位置 |

####  模拟列表来构造函数以及对应迭代器

``` js

    var List = function() {
      this.listSize = 0;
      this.pos = 0;
      this.dataStore = [];
    }

    List.prototype = (function () {
      return {
        length: length,
        clear: clear,
        toString: toString,
        getElement: getElement,
        insert: insert,
        append: append,
        remove: remove,
        front: front,
        end: end,
        prev: prev,
        next: next,
        hasNext: hasNext,
        hasPrev: hasPrev,
        currPos: currPos,
        moveTo: moveTo,
        find: find,
      }

      /*
        * 返回列表元素的个数
        * @returns {number}
        */
      function length() {
        return this.listSize;
      }
      /**
        * 清空列表中的所有元素
        */
      function clear() {
        delete this.dataStore;
        this.listSize = 0;
        this.currPos = 0;
        this.dataStore.length = 0;
      }
      /*
        * 返回要展示的列表
        * @returns {string}
        */
      function toString() {
        return this.dataStore;
      }
      /*
        * 返回当前位置的元素
        * @returns {*}
        */
      function getElement() {
        return this.dataStore[this.pos]
      }
        /*
        * 插入某个元素
        * @param ele 要插入的元素
        * @param after 列表中的元素之后
        * @returns {boolean}
        */
      function insert(ele, after) {
        var insertPos = this.dataStore.find(after);
        if(this.insertPos > -1) {
          this.dataStore.splice(insertPos, 0, ele)
          ++this.listSize;
          return true
        } else {
          return false
        }
      }
      /*
        * 给列表最后添加元素的时候，列表元素个数+1
        * @param ele
        */
      function append(ele) {
        ++this.listSize;
        this.dataStore.push(ele)
      }
      /*
        * 删除元素成功，元素个数-1
        * @param ele
        * @returns {boolean}
        */
      function remove(ele) {
        let removeIndex = this.dataStore.find(ele);
        if(removeIndex > -1) {
          this.dataStore.splice(removeIndex, 1)
          this.listSize --
          return true
        } else {
          return false
        }
      }
      /*
        * 将列表的当前位置移动到第一个元素
        */
      function front() {
        this.pos = 0
      }
      /*
        * 将列表的当前位置移动到最后一个元素
        */
      function end() {
        this.pos = this.listSize - 1
      }
      /*
        * 将当前位置向前移动一位
        */
      function prev() {
        --this.pos
      }
      /*
        * 将当前位置向后移动一位
        */
      function next() {
        ++this.pos
      }
      /*
        * 判断是否有后一位
        * @returns {boolean}
        */
      function hasNext() {
        return this.pos < this.listSize
      }
      /*
        * 判断是否有前一位
        * @returns {boolean}
        */
      function hasPrev() {
        return this.pos > 0
      }
      /*
        * 返回列表的当前位置
        * @returns {number|*}
        */
      function currPos() {
        return this.pos
      }
    }())

```