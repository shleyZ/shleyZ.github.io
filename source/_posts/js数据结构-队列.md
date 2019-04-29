---
title: js数据结构-队列
date: 2018-04-22 14:29:22
categories: 数据结构
---

#### 队列也是一种列表，不同于列表的是，只能在列表尾部插入元素，在队首删除元素，满足先进先出，后进后出的情况都适用于队列（FIFO:first-in-first-out）


#### 队列常见的属性或者方法：

| 类型   |      描述      |
|----------|:-------------:|
| enquene |  向队列末尾添加元素 |
| dequene |    删除队列头部元素   |
| front | 读取队列首元素 |
| back | 读取队列尾元素 |
| toString | 显示队列所有元素 |
| clear | 清空队列内元素  |
| empty | 判断队列是否为空  |

#### 队列的实现

    var Quene = function() {
      this.dataStore = []; //初始化为空
    }

    Quene.prototype = (function () {
      return {
        enquene: enquene,
        dequene: dequene,
        front: front,
        back: back,
        toString: toString,
        clear: clear,
        empty: empty,
      }

      // 入队
      function enquene(ele) {
        this.dataStore.push(ele)
      }
      // 出队
      function dequene() {
        if(this.empty()){
          return 'empty quene'
        } else {
          this.dataStore.shift()
        }
      }

      // 读取队列首元素
      function front(){
        if(this.empty()){
          return 'empty quene'
        } else {
          return this.dataStore[0]
        }
      }

      // 读取队列尾元素
      function back(){
        if(this.empty()){
          return 'empty quene'
        } else {
          return this.dataStore[this.dataStore.length - 1]
        }
      }

      // 显示队列所有元素
      function toString(){
        if(this.empty()){
          return 'empty quene'
        } else {
          return this.dataStore.join(',')
        }
      }

      // 清空队列内元素
      function clear() {
        delete this.dataStore;
        this.dataStore = []
      }

      // 判断队列是否为空
      function empty() {
        return this.dataStore.length === 0
      }
    }())


操作：

    var que = new Quene()
    que.enquene(1)
    console.log(que.front())  // 1
    console.log(que.back())  // 1
    que.enquene(2)
    que.enquene(3)
    console.log(que.front())  // 1
    console.log(que.back())  // 3
    que.dequene()
    console.log(que.front())  // 2
    console.log(que.back())  // 3
    console.log(que.toString())  // "2,3"
    console.log(que.empty())  // false
    que.clear()
    console.log(que.empty())  // true


#### 队列的应用

利用队列实现基数排序

基数排序（radix sort）属于“分配式排序”（distribution sort），它是透过键值的部份资讯，将要排序的元素分配至某些“桶”中，藉以达到排序的作用，基数排序法是属于稳定性的排序，其时间复杂度为O (nlog(r)m)，其中r为所采取的基数，而m为堆数，在某些时候，基数排序法的效率高于其它的稳定性排序法。


排序两位数的思路： 先按个位数字进行排序，再按十位数进行排序

假设我们有一串数字，分别为 73, 22, 93, 43, 55, 14, 28, 65, 39, 81

首先根据个位数字排序，81, 22, 73, 93, 43, 14, 55, 65, 28, 39

然后根据十位数字排序，14, 22, 28, 39, 43, 55, 65, 73, 81, 93


    //基数排序
    var queues = [];    //定义队列数组
    var nums = [];      //定义数字数组

    //选十个0~99的随机数进行排序
    for ( var i = 0 ; i < 10 ; i ++ ){
        queues[i] = new Queue();
        nums[i] = Math.floor( Math.random() * 101 );
    }

    //排序之前
    console.log( 'before radix sort: ' + nums );

    //基数排序
    distribution( nums , queues , 10 , 1 );
    collect( queues , nums );
    distribution( nums , queues , 10 , 10 );
    collect( queues , nums );

    //排序之后
    console.info('after radix sort: ' + nums );

    //根据相应的（个位和十位）数值，将数字分配到相应队列
    function distribution ( nums , queues , n , digit ) {  //digit表示个位或者十位的值
        for( var i = 0 ; i < n ; i++ ){
            if( digit == 1){
                queues[ nums[i] % 10 ].enqueue( nums[i] );
            }else{
                queues[ Math.floor( nums[i] / 10 ) ].enqueue( nums[i] );
            }
        }
    }

    //从队列中收集数字
    function collect ( queues , nums ) {
        var i = 0;
        for ( var digit = 0 ; digit < 10 ; digit ++ ){
            while ( !queues[digit].empty() ){
                nums[ i++ ] = queues[digit].front();
                queues[digit].dequeue();
            }
        }
    }


优先队列/循环队列    