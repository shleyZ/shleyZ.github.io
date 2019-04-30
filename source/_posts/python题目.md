---
title: python题目
date: 2017-10-14 13:27:40
categories: Python
---

1.打印多行字符串：

``` python
>>> print('''line1
... line2
... line3''')
line1
line2
line3
```

2.练习格式化字符串的使用：

``` python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

3.小明身高1.75，体重80.5kg。请根据BMI公式（体重除以身高的平方）帮小明计算他的BMI指数，并根据BMI指数：

低于18.5：过轻
18.5-25：正常
25-28：过重
28-32：肥胖
高于32：严重肥胖
用if-elif判断并打印结果：

``` python
# -*- coding: utf-8 -*-

height = 1.75
weight = 80.5
bmi = weight/(height*height)
if bmi<18.5:
	print('过轻')
elif bmi<25:
	print('正常')
elif bmi<28:
	print('过重')
elif bmi<32:
	print('肥胖')
else:
	print('严重肥胖')

>>>过重
```

4.请利用循环依次对list中的每个名字打印出Hello, xxx!：

``` python
# -*- coding: utf-8 -*-
L = ['Bart', 'Lisa', 'Adam']

for i in L:
	print('hello,',i)

>>> hello, Bart
>>> hello, Lisa
>>> hello, Adam
```

5.请利用Python内置的hex()函数把一个整数转换成十六进制表示的字符串：

``` python
# -*- coding: utf-8 -*-

n1 = 255
n2 = 1000
print('n1转换成十六进制：%s'% (hex(n1)))
print('n2转换成十六进制：%s'% (hex(n2)))

>>> n1转换成十六进制：0xff
>>> n2转换成十六进制：0x3e8
```

6.请定义一个函数quadratic(a, b, c)，接收3个参数，返回一元二次方程：ax2 + bx + c = 0的两个解。

提示：计算平方根可以调用math.sqrt()函数：

``` python
# -*- coding: utf-8 -*-

import math

def quadratic(a, b, c):
	deta = b*b - 4*a*c
	if deta == 0:
		return -b/(2*a)
	elif deta < 0:
		print('无解')
		return
	else:
		x1 = (-b + math.sqrt(deta))/(2*a)
		x2 = (-b - math.sqrt(deta))/(2*a)
		
	return x1,x2
```

7.一个函数：可接收一个或多个参数并计算乘积

``` python
# -*- coding: utf-8 -*-
def product(x,*y):
	result = x
	if len(y)==0:
		return result
	else:
		for i in y:
			result = result * i       
	return result
```

8.汉诺塔的移动可以用递归函数非常简单地实现。

请编写move(n, a, b, c)函数，它接收参数n，表示3个柱子A、B、C中第1个柱子A的盘子数量，然后打印出把所有盘子从A借助B移动到C的方法，例如：

``` python
move(3, 'A', 'B', 'C')
# 期待输出:
# A --> C
# A --> B
# C --> B
# A --> C
# B --> A
# B --> C
# A --> C

# -*- coding: utf-8 -*-
def move(n, a, b, c):
	if n == 1:
		print(a, '-->', c)
	else:
		move(n-1,a,c,b)
		move(1,a,b,c)
		move(n-1,b,a,c)
```

9.利用切片操作，实现一个trim()函数，去除字符串首尾的空格，注意不要调用str的strip()方法：

``` python
# -*- coding: utf-8 -*-
def trim(s):
	while s[0:1] == ' ':
		s = s[1:]
	while s[-1:] == ' ':
		s = s[:-1]
	return s
```

10.请使用迭代查找一个list中最小和最大值，并返回一个tuple：

``` python
# -*- coding: utf-8 -*-
def findMinAndMax(L):
	if len(L) == 0:
		return None,None
	max_value = L[0]
	min_value = L[0]
	for i in L:
		if max_value < i:
			max_value = i
		if min_value > i:
			min_value = i
	return min_value,max_value
```

11.请修改列表生成式，通过添加if语句保证列表生成式能正确地执行：

``` python
# -*- coding: utf-8 -*-
L1 = ['Hello', 'World', 18, 'Apple', None]
L2 = [i.lower() for i in L1 if isinstance(i,str)]

# 测试:
print(L2)
if L2 == ['hello', 'world', 'apple']:
	print('测试通过!')
else:
	print('测试失败!')
```

12.杨辉三角定义如下：

          1
         / \
        1   1
       / \ / \
      1   2   1
     / \ / \ / \
    1   3   3   1
   / \ / \ / \ / \
  1   4   6   4   1
 / \ / \ / \ / \ / \
1   5   10  10  5   1

把每一行看做一个list，试写一个生成器generator，不断输出下一行的list：

``` python
# -*- coding: utf-8 -*-

def triangles():
	result = [1]
	while True:
		yield result
		result = [result[x] + result[x+1] for x in range(len(result)-1)]
		result.append(1)
		result.insert(0,1)
		if n>10:
			break
```   
输出:

``` python
>>> [1]
>>> [1, 1]
>>> [1, 2, 1]
>>> [1, 3, 3, 1]
>>> [1, 4, 6, 4, 1]
>>> [1, 5, 10, 10, 5, 1]
>>> [1, 6, 15, 20, 15, 6, 1]
>>> [1, 7, 21, 35, 35, 21, 7, 1]
>>> [1, 8, 28, 56, 70, 56, 28, 8, 1]
>>> [1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
```

13.利用map()函数，把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：['adam', 'LISA', 'barT']，输出：['Adam', 'Lisa', 'Bart']：

``` python
# -*- coding: utf-8 -*-
def normalize(name):
	return name[0].upper()+name[1:].lower()

# 测试:
L1 = ['adam', 'LISA', 'barT']
L2 = list(map(normalize, L1))
print(L2)

>>>['Adam', 'Lisa', 'Bart']
```

14.编写一个prod()函数，可以接受一个list并利用reduce()求积：

``` python
# -*- coding: utf-8 -*-
from functools import reduce
def prod(L):
	return reduce(lambda x, y : x * y, L)
```

15.利用map和reduce编写一个str2float函数，把字符串'123.456'转换成浮点数123.456：

``` python
# -*- coding: utf-8 -*-
from functools import reduce

def str2float(s):
	n = s.index('.')
	s1 = list(map(int, [x for x in s[:n]]))
	s2 = list(map(int,[x for x in s[n+1:]]))
	return reduce(lambda x, y : x * 10 + y, s1) + reduce(lambda x, y : x * 10 + y, s2)/10**len(s2)

>>> str2float('123.456') = 123.456
```

16.回数是指从左向右读和从右向左读都是一样的数，例如12321，909。请利用filter()筛选出回数：

``` python
# -*- coding: utf-8 -*-
def is_palindrome(n):
	n = str(n)
	return n == n[::-1]

list(filter(is_palindrome, range(1, 200)))
>>>>[1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 22, 33, 44, 55, 66, 77, 88, 99, 101, 111, 121, 131, 141, 151, 161, 171, 181, 191]
```

17.假设我们用一组tuple表示学生名字和成绩：

L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
请用sorted()对上述列表分别按名字排序：

``` python
# -*- coding: utf-8 -*-

L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
def by_name(t):
	return t[0]
L2 = sorted(L, key=by_name)
print(L2)

>>> [('Adam', 92), ('Bart', 66), ('Bob', 75), ('Lisa', 88)]
```

再按成绩从高到低排序：

``` python
def by_name(t):
		return -t[1]

>>> [('Adam', 92), ('Lisa', 88), ('Bob', 75), ('Bart', 66)]
```

18.利用闭包返回一个计数器函数，每次调用它返回递增整数：

``` python
# -*- coding: utf-8 -*-
def createCounter():
	n = 0
	def counter():
		nonlocal n
		n = n + 1
		return n
	return counter
```

内层函数"可以使用"外层作用域中的变量,内层函数"无法修改"外层变量,使用 nonlocal 修改外层变量.
通常情况下不要在内部函数中对外部作用域的变量重新赋值。

19.请设计一个decorator，它可作用于任何函数上，并打印该函数的执行时间：

``` python
# -*- coding: utf-8 -*-
import time, functools
def metric(fn):
	def wrapper(*args,**wk):
		time_start = time.time()
		fn(*args,**wk)
		time_end = time.time()
		print('%s executed in %s ms' % (fn.__name__, time_end - time_start))
		return fn(*args,**wk)
	return wrapper
```

20.安装常用的第三方模块Anaconda

21.请把下面的Student对象的gender字段对外隐藏起来，用get_gender()和set_gender()代替，并检查参数有效性：

``` python
# -*- coding: utf-8 -*-
class Student(object):
	def __init__(self, name, gender):
		self.name = name
		self.__gender = gender
	def get_gender(self):
		return self.__gender
	def set_gender(self, gende):
		if not isinstance(gende, str):
			raise ValueError('Bad Value')
		else:
			self.__gender = gende
```

22.为了统计学生人数，可以给Student类增加一个类属性，每创建一个实例，该属性自动增加：

``` python
# -*- coding: utf-8 -*-
class Student(object):
	count = 0

	def __init__(self, name):
		self.name = name
		Student.count = Student.count + 1
		print(Student.count)
```

# 测试:

``` python
if Student.count != 0:
	print('测试失败!')
else:
	bart = Student('Bart')
	if Student.count != 1:
		print('测试失败!')
	else:
		lisa = Student('Bart')
		if Student.count != 2:
			print('测试失败!')
		else:
			print('Students:', Student.count)
			print('测试通过!')

>>> 1
>>> 2
>>> Students: 2
>>> 测试通过!
```

实例属性属于各个实例所有，互不干扰；

类属性属于类所有，所有实例共享一个属性；

23.请利用装饰器@property给一个Screen对象加上width和height属性，以及一个只读属性resolution：

``` python
# -*- coding: utf-8 -*-
class Screen(object):
	@property
	def width(self):
		return self._width
	
	@width.setter
	def width(self,value):
		self._width = value
	
	@property
	def height(self):
		return self._height
	@height.setter
	def height(self,value):
		self._height = value
	
	@property
	def resolution(self):
		self._resolution = 786432
		return self._resolution
```

测试:

``` python
s = Screen()
s.width = 1024
s.height = 768
print('resolution =', s.resolution)
if s.resolution == 786432:
	print('测试通过!')
else:
	print('测试失败!')	        

#结果：
>>> resolution = 786432
>>> 测试通过!
```

24.利用os模块编写一个能实现dir -l输出的程序。(显示当前目录下所有的文件和文件夹)

``` python
>>> import os
>>> print([x for x in os.listdir('.')])
['.deploy_git', '.DS_Store', '.gitignore', '_config.yml', 'db.json', 'debug.log', 'node_modules', 'package.json', 'public', 'scaffolds', 'source', 'themes']
```	

	

