---
title: js算法-排序
date: 2018-05-02 17:05:14
categories: 算法
tags: 算法
---

我们平时对计算机中存储的数据执行的两种最常见的操作就是排序和查找，对于计算机的排序和查找的研究，自计算机诞生以来就没有停止过。如今又是大数据，云计算的时代，对数据的排序和查找的速度、效率要求更高，因此要对排序和查找的算法进行专门的数据结构设计，（例如我们上一篇聊到的二叉查找树就是其中一种），以便让我们对数据的操作更加简洁高效。


介绍一下排序算法的术语：

+ 稳定：如果排序前a在b前面，并且a=b，排序后a仍然在b前面
+ 不稳定：如果排序前a在b前面，并且a=b，排序后a可能在b后面
+ 内排序：所有排序操作都在内存中进行
+ 外排序：由于数据太大，因此数据放在磁盘中，排序操作通过磁盘和内存的传输才能进行
+ 时间复杂度： 算法执行所需要的时间
+ 空间复杂度： 算法执行所需要的内存大小

假设下面的排序都是按从小到大排列

### 冒泡排序

对比相邻两个元素，如果第一个比第二个大就交换。是排序最慢的算法之一

![bubble_sort](http://ww1.sinaimg.cn/large/8c85763dgy1g2ksx26x4qg20my075wqv.gif)

实现如下：

``` js
function bubbleSort(data){
  for (var i=0; i<data.length; i++){
    for(var j=i; j<data.length; j++){
      if(data[i] > data[j]){
        let temp;
        temp = data[i];
        data[i] = data[j];
        data[j] = temp;
      }
    }
  }
  return data
}

var arr = [72 , 1 , 68 , 95 , 75 , 54 , 58 , 10 , 35 , 6 , 28 , 45 , 69 , 13 , 88 , 99 , 24 , 28 , 30 , 31 , 78 , 2 , 77 , 82 , 72]
var bubbleSortArr = bubbleSort(arr)
console.log(arr)
console.log(bubbleSortArr)
// [72, 1, 68, 95, 75, 54, 58, 10, 35, 6, 28, 45, 69, 13, 88, 99, 24, 28, 30, 31, 78]
// [1, 6, 10, 13, 24, 28, 28, 30, 31, 35, 45, 54, 58, 68, 69, 72, 75, 78, 88, 95, 99]

```

### 选择排序

从数组的开头开始遍历，记录里面最小的数，遍历完，把最小的数放在第一位，
然后，从第二位开始遍历，记录里面最小的数，遍历完，把最小的数放在第二位，
。。。。依次遍历
直到倒数第二位遍历完，就完成了选择排序

![bubble_sort](http://ww1.sinaimg.cn/large/8c85763dgy1g2kt00erbbg20mj06w7l2.gif)

实现如下：

``` js
function selectionSort(data) {
  for(var i=0; i<data.length; i++) {
    let min = data[i]
    let minIndex = i;
    for(var j=i; j<data.length; j++) {
      if(data[j] < data[i]) {
        min = data[j]
        minIndex = j
      }
    }
    let temp = data[i];
    data[i] = min;
    data[minIndex] = temp;
  }
}

var arr = [72 , 1 , 68 , 95 , 75 , 54 , 58 , 10 , 35 , 6 , 28 , 45 , 69 , 13 , 88 , 99 , 24 , 28 , 30 , 31 , 78 , 2 , 77 , 82 , 72]
console.log(arr)
var selectionSort = selectionSort(arr)
console.log(selectionSort)

// [72, 1, 68, 95, 75, 54, 58, 10, 35, 6, 28, 45, 69, 13, 88, 99, 24, 28, 30, 31, 78]
// [1, 6, 10, 13, 24, 28, 28, 30, 31, 35, 45, 54, 58, 68, 69, 72, 75, 78, 88, 95, 99]

```

### 插入排序

从第一位开始，默认它为已排序序列，
取下一个元素，在已排序序列中，从后向前比对已排序数据，
如果已排序数据大于取出的元素，将已排序元素后移
如果已排序数据小于等于取出的元素，将取出的元素插入该位置
再取下一个元素。。。。。

![bubble_sort](http://ww1.sinaimg.cn/large/8c85763dgy1g2ktk2o6jfg20mj0e113f.gif)

