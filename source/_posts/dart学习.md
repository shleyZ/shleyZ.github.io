---
title: dart学习
date: 2020-02-13 17:36:57
tags: dart
categories: dart
---

- Dart 要求以 main 函数作为执行的入口

- 变量的声明可以用 var 或者 具体的类型，当使用 var 定义变量时，编译器会推断决定变量的类型。

- 默认未初始化的变量都是 null

- Dart 是类型安全的语言，所有的类型都是对象，继承自 Object。一切类型的值都是对象的实例，包括数字，布尔，函数和 null

- Dart 内置数据类型: num

    - num 只有两种子类： 64 位 int 和 64 位 double（符合 IEEE 754 标准的）
    - 64 位 int 代表整数类型， 64 位 double 代表浮点数
    - 继承自num的运算符有 +,-,*,/,%,abs()绝对值,round()取整,还有<,>等比较运算符
    - dart:math 库提供了诸如三角函数、指数、对数、平方根等高级函数

- Dart 内置数据类型: bool

    因为 Dart 类型是安全的，所以不能在 if 判断或者 assert 中使用非布尔类型。

- Dart 内置数据类型: String

    使用 ' 或者 " 来表示字符串字面量
    ${express} 可以在字符串中嵌入变量
    '+' 可以实现字符串的拼接
    '''jksdhk
    jashdkjas
    dgkhjas'''可以实现多行字符输出

- Dart 内置数据类型: List

    - 声明方法一： var arr1 = <String>["Tom", "Andy", "Jack"];
    - 声明方法二： var arr2 = new List<int>.of([1,2,3]);
    - 数组添加元素： arr2.add(499); // 添加的元素类型必须与数组里面元素类型一致。
    - 数组遍历： arr2.forEach((v) => print('${v}'));

- Dart 内置数据类型: Map

    - 声明方法一： var map1 = {'name': 'Tom','sex': 'male',};;
    - 声明方法二： var map2 = new Map<String String>();
    - 数组添加元素： map2['name'] = 'Tom'; // 添加的元素类型必须与Map里面元素类型一致。
    - 数组遍历： map2.forEach((k,v) => print('${k}: ${v}'));

- Dart 常量，使用 final 或者 const 声明

    - const，表示变量在编译期间即能确定的值；
    - final 则不太一样，用它定义的变量可以在运行时确定值，而一旦确定后就不可再变。