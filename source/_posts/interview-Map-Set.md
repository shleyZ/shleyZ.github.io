---
title: interview-Map/Set
date: 2020-02-24 18:04:18
categories: javascript
tags: js基础
---


## Set

Set 是一个值的集合，无论原始值或者引用值，每个值都是唯一的。

```js
const s1 = new Set()
s1.add('1');
s1.add(1);
s1.add(false);
s1.has(1) // true
for (let item of s1) console.log(item); // '1', 1
s1.delete(1)
s1.has(1) // false
s1.clear()
s1.has('1') // false
```

## WeakSet 

存储 **弱保持对象集合**。

与 Set 相比：

- WeakSet 只能是对象集合。
- 存储对象的引用为弱引用， 如果 WeakSet 中的对象没有被引用， 那么这些对象会被当作垃圾回收。也意味着 WeakSet 并没有存储当前对象列表。
- WeakSet 没有 size 属性不能遍历。


## Map 

Map 是一个键值对集合， 并且能记住键的原始插入顺序。

Map 和 Objects 一样，都允许存储一个值，删除一个键，检测键值。
Map 和 Objects 差异：

- Map 默认不包含任何键, 只包括显式插入的键，但是 Object 默认原型链上有 键。
- Map 的键可以是任意值， 但是 Object 的键必须是 string 或 symbol
- Map 的键个数能用 size 属性获取，但 Object 的键数只能手动计算。
- Map 可以直接被迭代，但 Object 需要获取键在迭代。

```js 
let myMap = new Map();
let keyObj = {};
let keyFunc = function() {};
let keyString = 'a string';

myMap.set(keyString, "和键'a string'关联的值");
myMap.set(keyObj, "和键keyObj关联的值");
myMap.set(keyFunc, "和键keyFunc关联的值");

myMap.size; // 3

myMap.get(keyString);    // "和键'a string'关联的值"
myMap.get(keyObj);       // "和键keyObj关联的值"
myMap.get(keyFunc);      // "和键keyFunc关联的值"

myMap.get('a string');   // "和键'a string'关联的值"
                         // 因为keyString === 'a string'

myMap.get({});           // undefined, 因为keyObj !== {}
myMap.get(function() {}); // undefined, 因为keyFunc !== function () {}

for (let value of myMap.values()) {
  console.log(value);
}
```

## WeakMap 

WeakMap 对象是一组键/值对的集合，其中的键是弱引用的, 其键必须是对象，而值可以是任意的。

WeakMap 是不可枚举的。弱引用使得在没有其他引用存在时垃圾回收能正确进行。