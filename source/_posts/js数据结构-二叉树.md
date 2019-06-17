---
title: js数据结构-树，二叉树
date: 2018-04-25 13:10:24
categories: 数据结构
tags: 数据结构
---

### 不同于列表，队列，栈等线性数据结构，树是一种非线性结构的树。

树的特征就是： 元素之间有明显的层次特性，常用来存储具有层级关系的数据，比如文件系统的文件

*父节点*树结构中每一个节点只有一个前件
*根节点*没有前件的节点只有一个就是 
*子节点*每个节点可以有多个后件 
*叶子节点*没有后件的节点 
*度*一个节点所拥有的子节点的个数 
*树的度*所有节点最大的度 
*树的深度*树的最大层次 

### 二叉树

是一种特殊的树，他的子节点个数不超过两个，分别被成为左子树和右子树  
二叉树也被称为二叉查找树，二叉堆，二叉排序树

![baniery_sort_tree](http://ww1.sinaimg.cn/large/8c85763dgy1g2jh567lzpj20da0ab3yq.jpg)

*树的遍历：* 按一定的规则和顺序走遍二叉树的所有节点，使每个节点都被访问到，并且只被访问一次。

二叉树是非线性结构，所以要先把二叉树抓换成线性序列

按根节点的访问顺序，树的遍历分为三种： 

+  前序遍历： 根节点 --> 左子树 --> 右子树

![qianxu](http://ww1.sinaimg.cn/large/8c85763dgy1g2jh6ovs9dj20f90asaaf.jpg)

+  中序遍历： 左子树 --> 根节点 --> 右子树

![zhongxu](http://ww1.sinaimg.cn/large/8c85763dgy1g2jh72laktj20fb0b9gly.jpg)

+  后续遍历： 左子树 --> 右子树 --> 根节点

![houxu](http://ww1.sinaimg.cn/large/8c85763dgy1g2jh79sbl8j20eb0ah0t2.jpg)

如上图遍历结果：

前序遍历：ABGEFDC
中序遍历：DEBGFAC
后续遍历：EDGFBCA

#### 二叉查找树（BST）的属性或者方法：

| 类型   |      描述      |
|----------|:-------------:|
| insert |  插入节点 |
| preOrder | 先序遍历 |
| inOrder | 中序遍历 |
| postOrder | 后序遍历 |
| find | 查找节点 |
| getMin | 查找最小值  |
| getMax | 查找最大值 |
| remove | 删除节点 |


#### 二叉查找树（BST）的实现

二叉树实际由多个节点组成，因此定义Node类，用于存放树的节点。  
Node类既保存了数据又保存了其左节点和右节点的链接，show方法用来显示保存在节点中的数据。

``` js
    // 节点定义
    function Node(data, left, right) {
      this.data = data;
      this.left = left;
      this.right = right;
      this.show = show
    }
    function show() {
      return this.data;
    }

    // 二叉查找树（BST）的类
    function BST(){
      this.root = null;
      this.insert = insert;       // 插入节点
      this.preOrder = preOrder;   // 先序遍历
      this.inOrder = inOrder;     // 中序遍历
      this.postOrder = postOrder; // 后序遍历
      this.find = find;           // 查找节点
      this.getMin = getMin;       //查找最小值
      this.getMax = getMax;       //查找最大值
      this.remove = remove;       //解除节点
    }
```

向二叉树插入数据，涉及到插入的位置问题

+ 首先创建一个新的Node节点，

+ 其次检查当前BST是否有根节点，如果没有，则是一个新树，添加的节点就是根节点。

+ 如果有根节点，则要遍历BST，找到合适的位置插入：

    1. 设置当前节点为根节点
    2. 如果待插入的data小于当前节点的值，则新节点为当前节点的左节点，反之进行 4 操作
    3. 如果当前节点的左节点为null,则新节点就放在这里，退出循环，否则进入下一次循环
    4. 新节点为当前节点的右节点
    5. 如果当前节点的右节点为null,则新节点就放在这里，退出循环，否则进入下一次循环

具体实现：

``` js

    function insert(data) {
      var insertN = new Node(data, null, null)
      if(this.root == null){
        this.root = insertN
      } else {
        var current = this.root;
        while(true) {
          if(data < current.data) {
            // 放在左节点
            if(current.left == null) {
              current.left = insertN
              break;
            } else {
              current = current.left
            }
          } else {
            // 放在右节点
            if(current.right == null) {
              current.right = insertN
              break;
            } else {
              current = current.right
            }
          }
        }
      }
    }

    // 中序遍历
    function inOrder(node) {
      if(!(node == null)){
        inOrder(node.left)
        console.log(node.data)
        inOrder(node.right)
      }
    }

    // 先序遍历
    function preOrder(node){
      if(!(node == null)) {
        console.log(node.data)
        preOrder(node.left)
        preOrder(node.right)
      }
    }

    // 后序遍历
    function postOrder(node){
      if(!(node == null)) {
        postOrder(node.left)
        postOrder(node.right)
        console.log(node.data)
      }
    }

    //查找最小值, 最小值应该在左子树，遍历左子树直到他的左子树为null时
    function getMin() {
      var current = this.root;
      while(!(current.left == null)){
        current = current.left
      }
      return current.show()
    }

    //查找最大值, 最大值应该在右子树，遍历右子树直到他的右子树为null时
    function getMax() {
      var current = this.root;
      while(!(current.right == null)){
        current = current.right
      }
      return current.show()
    }

    // 查找节点
    function find(data) {
      var current = this.root;
      while(!(current == null)) {
        if(current.data === data) {
          return current
        } else if(current.data > data) {
          current = current.left
        } else {
          current = current.right
        }
      }
      return null
    }

    // 移除节点
    function remove(data) {
      removeNode(this.root, data)
    }

    // 查找最小值
    function getSmallest(node) {
      if(node.left == null) {
        return node
      } else {
        return getSmallest(node.left)
      }
    }

    function removeNode(node, data){
      if (node == null) {
        return null
      }
      if(data == node.data) {
        // 没有子节点
        if(node.left == null && node.right == null) {
          return null
        }
        // 只有右节点
        if(node.left == null){
          return node.right
        }
        // 只有左节点
        if(node.right == null){
          return node.left
        }
        // 有两个子节点
        var tempNode = getSmallest(node.right)
        node.data = tempNode.data;
        node.right = removeNode(node.right, tempNode.data)
        return node
      } else if(data < node.data) {
        removeNode(node.left, data)
      } else {
        removeNode(node.right, data)
      }
    }
```

测试：

``` js

    var bst = new BST()
    bst.insert(9)
    bst.insert(4)
    bst.insert(14)
    bst.insert(12)
    bst.insert(11)
    bst.insert(13)
    bst.insert(15)
    bst.insert(16)
    console.log(bst)
    
    inOrder(bst.root)             // 4, 9, 11, 12, 13, 14, 15, 16  
    console.log('------') 
    preOrder(bst.root)            // 9, 4, 14, 12, 11, 13, 15, 16 
    console.log('------')
    postOrder(bst.root)
    console.log('------')         // 4, 11, 13, 12, 16, 15, 14, 9
    console.log(bst.getMin())     // 4
    console.log(bst.getMax())     // 16
    console.log(bst.find(12))     // Node {data: 12, left: Node, right: Node, show: ƒ}

    bst.remove(12)
    console.log(bst)              // BST {root: Node, insert: ƒ, preOrder: ƒ, inOrder: ƒ, postOrder: ƒ, …}
```


    
