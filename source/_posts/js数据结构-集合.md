---
title: js数据结构-集合
date: 2019-04-24 11:10:53
categories: 数据结构
---

#### 集合是一组无序但彼此之间又有一定关系的成员构成，每个成员在集合中只出现一次。

集合的特征就是： 1. 无序， 2. 不重复

不包含任何成员的集合是*空集*
包含一切可能成员的集合是*全集*
两个集合里面成员完全相同，则*两个集合相等*
如果一个集合A的成员全部包含在另一个成员B,则A是B的*子集*

#### 集合的操作

*并集：*两个集合中的成员合并，生成的一个新的集合
*交集：*两个集合中的共同存在的成员，生成的新的集合
*补集：*属于一个集合但不属于另一个集合，生成的新的集合

#### 集合常见的属性或者方法：

| 类型   |      描述      |
|----------|:-------------:|
| add |  添加成员 |
| remove | 删除成员 |
| size |  集合成员个数  |
| union | 求并集 |
| intersect | 求交集 |
| subset | 判断一个集合是否是另一个集合的子集  |
| difference | 求补集 |
| contains | 判断成员是否属于集合 |
| show | 显示集合 |


#### 集合的实现

    var mySet = function() {
      this.dataStore = []; //初始化为空
    }

    mySet.prototype = (function () {
      return {
        add: add,
        remove: remove,
        size: size,
        union: union,
        intersect: intersect,
        subset: subset,
        difference: difference,
        contains: contains,
        show: show,
      }

      // 添加成员, 返回boolean
      function add(data) {
        let index = this.dataStore.indexOf(data)
        if(index === -1) {
          // 如果元素不存在
          this.dataStore.push(data);
          return true
        } else {
          // 如果元素存在
          console.log('can not add ' + data + ', must already be existed')
          return false
        }
      }

      // 删除一个成员
      function remove(data){
        let index = this.dataStore.indexOf(data)
        if(index === -1) {
          // 如果元素不存在
          console.log(data + ', is not in set')
          return false
        } else {
          // 如果元素存在
          this.dataStore.splice(index, 1)
          return true
        }
      }

      // 集合成员个数
      function size(key) {
        return this.dataStore.length;
      }

      // 判断成员是不是在集合内
      function contains(data) {
        let index = this.dataStore.indexOf(data)
        return index !== -1
      }

      // 两个集合的并集
      function union(set){
        var tempSet = new mySet();
        for (var i=0; i<this.dataStore.length; i++) {
          tempSet.add(this.dataStore[i])
        }
        for (var i=0; i<set.dataStore.length; i++) {
          if(!tempSet.contains(set.dataStore[i])) {
            tempSet.add(set.dataStore[i])
          } 
        }
        return tempSet
      }

      // 集合的交集
      function intersect(set){
        var tempSet = new mySet();
        for (var i=0; i<this.dataStore.length; i++) {
          if(set.contains(this.dataStore[i])) {
            tempSet.add(this.dataStore[i])
          } 
        }
        return tempSet
      }

      // 判断一个集合是否是另一个集合set的子集
      function subset(set) {
        if(this.size() > set.size()) {
          return false
        } else {
          for (var i=0; i<this.dataStore.length; i++) {
            if(!set.contains(this.dataStore[i])) {
              return false
            } 
          }
        }
        return true
      }

      // 集合的补集
      function difference(set) {
        var tempSet = new mySet();
        for (var i=0; i<this.dataStore.length; i++) {
          if(!set.contains(this.dataStore[i])) {
            tempSet.add(this.dataStore[i])
          } 
        }
        return tempSet
      }

      // 显示集合
      function show() {
        console.log(this.dataStore)
        return this.dataStore
      }
    }())

操作：

    var myset = new mySet()
    myset.show()  // []
    
    myset.add('cat') // ["cat"]
    myset.add('dog') // ["cat", "dog"]
    myset.add('cat')  // can not add cat, must already be existed
    myset.add('fish') // ["cat", "dog", "fish"]
    myset.add('bird') // ["cat", "dog", "fish", "bird"]
    myset.remove('cat') // ["dog", "fish", "bird"]

    var otherset = new mySet()
    otherset.add('cat')
    otherset.add('dog')
    otherset.add('fish') // ["cat", "dog", "fish"]

    console.log(myset.subset(otherset))  // false
    otherset.add('bird') // ["cat", "dog", "fish", "bird"]
    console.log(myset.subset(otherset))  // true
    console.log(myset.union(otherset)) // ["dog", "fish", "bird", "cat"]
    console.log(myset.intersect(otherset)) // ["dog", "fish", "bird"]
    console.log(myset.difference(otherset)) // []

