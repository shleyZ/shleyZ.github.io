---
title: js数据结构-链表
date: 2018-04-23 15:54:06
categories: 数据结构
tags: 数据结构
---


#### 链表 Linked-list

链表的种类有单向链表，双向链表，单向循环链表和双向循环链表

链表是一组节点的集合，每个节点都使用一个对象的引用（链）来指向后一个节点。


    data | next --> data2 | next --> Null

其中data保存数据，next保存下一个节点的引用（指针），上面最后一个节点也就是链表的为元素指向null,表示链接结束。

有头节点的链表: 由于链表的起始点比较难以确定，所以很多链表的实现都是在链表的最前面加一个特殊的节点，称为头节点，来表示链表的头部，链表就变成了这样：

    Header | next --> data1 | next --> data2 | next --> Null  


1. 向链表中插入一个节点：

    只需改变前一个节点的指向（指向到新节点），新节点的指向原来前一个节点的指向，如下新节点data3

    Header | next --> data1 | next  --> data3 | next --> data2 | next --> Null

2. 向列表删除一个节点(删除data1节点)：

    只需将带删除节点的前一个节点指向改为待删节点的指向，然后把待删节点指向null

    Header | next --> data3 | next --> data2 | next --> Null
    data1 | next --> null


#### 链表的实现

设计的链表应该包含两个类，一个是Node类用来表示节点，一个是LinkedList用来表示对节点的操作（添加/删除等）

``` js 

    function Node(ele) {
      this.element = ele; 
      this.next = null
    }

    function LList() {
      this.head = new Node('head);
      this.find = find;
      this.insert = insert;
      this.remove = remove;
      this.findPrev = findPrev;
      this.display = display;
    }

    // 查找某个节点
    function find(item){
      var currNode = this.head;
      while(currNode.element !== item) {
        currNode = currNode.next  
      }
      return currNode;
    }

    // 在item节点插入newEle节点
    function insert(newEle, item) {
      var currNode = this.find(item);
      var newNode = new Node(newEle);
      newNode.next = currNode.next;
      currNode.next = newNode;
    }

    // 查找前一个节点
    function findPrev(item) {
      var currNode = this.head;
      while( !(currNode.next == null)  && currNode.next.element !== item) {
        currNode = currNode.next
      }
      return currNode
    }

    // 移除一个节点
    function remove(item) {
      var currNode = this.find(item);
      var PrevNode = this.findPrev(item);
      PrevNode.next = currNode.next;
      currNode.next = null
    }

    // 显示所有链表元素
    function display() {
      var currNode = this.head;
      while( !(currNode.next == null) ){
        console.log(currNode.element);
        currNode = currNode.next
      }
    }

```

测试：

``` js

    var linkedList = new LList()
    console.log(linkedList) 
    // LList {head: Node, find: ƒ, insert: ƒ, remove: ƒ, findPrev: ƒ, …}
        // display: ƒ display()
        // find: ƒ find(item)
        // findPrev: ƒ findPrev(item)
        // head: Node
        // element: "head"
        // next: Node {element: "node1", next: Node}
        // __proto__: Object
        // insert: ƒ insert(newEle, item)
        // remove: ƒ remove(item)
        // __proto__: Object
    linkedList.insert('node1', 'head')
    linkedList.insert('node2', 'node1')
    linkedList.insert('node3', 'node2')
    console.log(linkedList.display())
    // 
    // head
    // node1
    // node2
    linkedList.remove('node2')
    console.log(linkedList)
    // 
    // head
    // node1

```

#### 双向链表的实现

Null <-- previous | Header | next <--> previous | data1 | next <--> previous | data2 | next --> Null  

要实现双向链表，需要给node类添加一个previous属性，

``` js

    function Node(ele) {
      this.element = ele; 
      this.next = null;
      this.previous = null;
    }

    function LList() {
      this.head = new Node('head');
      this.find = find;
      this.findLast = findLast;
      this.insert = insert;
      this.remove = remove;
      this.findPrev = findPrev;
      this.display = display;
      this.dispReverse = dispReverse; // 反向显示链表
    }

    // 查找某个节点
    function find(item){
      var currNode = this.head;
      while(currNode.element !== item) {
        currNode = currNode.next  
      }
      return currNode;
    }

    // 在item节点插入newEle节点
    function insert(newEle, item) {
      var currNode = this.find(item);
      var newNode = new Node(newEle);
      newNode.next = currNode.next;
      newNode.previous = currNode;
      currNode.next = newNode;
    }

    // 查找前一个节点
    function findPrev(item) {
      var currNode = this.head;
      while( !(currNode.next == null)  && currNode.next.element !== item) {
        currNode = currNode.next
      }
      return currNode
    }

    // 查找最后一个元素
    function findLast() {
      var currNode = this.head;
      while(!(currNode.next == null)) {
        currNode = currNode.next  
      }  
      return currNode
    }

    // 移除一个节点
    function remove(item) {
      var currNode = this.find(item);
      var PrevNode = this.findPrev(item);
      PrevNode.next = currNode.next;
      currNode.next = null
    }

    // 显示所有链表元素
    function display() {
      var currNode = this.head;
      while( !(currNode.next == null) ){
        console.log(currNode.element);
        currNode = currNode.next
      }
    }

    // 反向显示所有链表元素
    function dispReverse() {
      var lastNode = this.findLast();
      while( !(lastNode.previous.element == 'head') ){
        console.log(lastNode.element);
        lastNode = lastNode.previous
      }
    }

```

测试：

``` js

    var lists = new LList();

    lists.insert('node' , 'head');
    lists.insert('node1' , 'node');
    lists.insert('node2' , 'node1');
    lists.insert('node3' , 'node2');

    console.log( lists.display() ); 
    // node
    // node1
    // node2
    // undefined

    console.log( lists.dispReverse() );
    // node2
    // node1
    // node
    // undefined  

```

#### 循环链表的实现

Header | next --> data1 | next --> data2 | next --> Header  

循环链表和单链表相似，节点类型都是一样，唯一的区别是，在创建循环链表的时候，让其头节点的 next 属性执行它本身，即

``` js

        head.next = head;

```        