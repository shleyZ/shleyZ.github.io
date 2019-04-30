---
title: python基础总结
date: 2017-11-26 16:11:12
categories: Python
---

## 1.原始数据类型和运算符

#### (is vs. ==) is 检查两个变量是否引用同一个对象, 但是 == 检查指向的对象是否具有相同的值.

``` python
a = [1, 2, 3, 4]  # Point a at a new list, [1, 2, 3, 4]
b = a             # Point b at what a is pointing to
b is a            # => True, a and b refer to the same object
b == a            # => True, a's and b's objects are equal
b = [1, 2, 3, 4]  # Point b at a new list, [1, 2, 3, 4]
b is a            # => False, a and b do not refer to the same object
b == a            # => True, a's and b's objects are equal
```

#### 字符串也可以+ 但尽量不要这样做。
   
``` python 
"Hello " + "world!"  # => "Hello world!"
```

#### 字符串文字（但不是变量）可以连接而不使用'+'
  
``` python  
"Hello " "world!"    # => "Hello world!"
```

#### 可以将字符串视为字符列表

``` python 
"This is a string"[0]  # => 'T'
```

#### 字符串的长度

``` python 
len("This is a string")  # => 16
```

#### .format可用于格式化字符串，如下所示：

``` python 
"{} can be {}".format("Strings", "interpolated")  
# => "Strings can be interpolated"
```

#### 重复的format 以方便输入.

``` python 
"{0} be nimble, {0} be quick, {0} jump over the {1}".format("Jack", "candle stick")
# => "Jack be nimble, Jack be quick, Jack jump over the candle stick"
```

#### format可以使用关键字。

``` python 
"{name} wants to eat {food}".format(name="Bob", food="lasagna")  
# => "Bob wants to eat lasagna"
```

#### 如果Python 3代码也需要在Python 2.5及更低版本上运行，你仍然可以使用旧格式：

``` python 
"%s can be %s the %s way" % ("Strings", "interpolated", "old")  
# => "Strings can be interpolated the old way"
```

#### 还可以使用f-strings或格式化的字符串文字进行格式化（在Python 3.6+中）

``` python 
name = "Reiko"
f"She said her name is {name}." # => "She said her name is Reiko"
```

#### 可以将任何Python语句放在大括号内，它将在字符串中输出。

``` python 
f"{name} is {len(name)} characters long."
```

#### None 是一个对象

``` python 
None  # => None
```

#### 不要使用等号“==”符号将对象与None进行比较，而是使用“is”。 这会检查对象标识是否相等。

``` python 
"etc" is None  # => False
None is None   # => True    
```

#### None, 0和空字符串/列表/ dicts /元组都计算为False。 所有其他值均为True

``` python 
bool(0)   # => False
bool("")  # => False
bool([])  # => False
bool({})  # => False
bool(())  # => False
```


## 2.变量和集合

#### print 方法，默认情况下，print方法最后有一个换行符。使用可选参数end更改结束字符串。

``` python 
print("I'm Python. Nice to meet you!")  
# => I'm Python. Nice to meet you!

print("Hello, World", end="!")  
# => Hello, World!
```

#### 从控制台获取输入数据

``` python 
input_string_var = input("Enter some data: ") 
```

#### 变量没有声明，只有赋值。约定命名规则：小写和下划线lower_case_with_underscores

``` python 
some_var = 5
some_var  # => 5
```

#### 访问以前未分配的变量会报错。

``` python 
some_unknown_var  # 引发异常
```

#### if 表达式，等价于三元运算符

``` python 
"yahoo!" if 3 > 2 else 2 # => 'yahoo!'
"yahoo!" if 3 > 4 else 2 # => 2
```

#### 数组--使用append将内容添加到列表末尾

``` python 
li = []
li.append(1)    # li is now [1]
li.append(2)    # li is now [1, 2]
li.append(4)    # li is now [1, 2, 4]
li.append(3)    # li is now [1, 2, 4, 3]    
```

#### 数组--使用pop从列表末尾删除

``` python 
li.pop()        # => 3 and li is now [1, 2, 4]    
```

#### 数组--使用切片语法查看范围。包含起始索引，不包括结束索引

``` python 
li = [1, 2, 4, 3] 
li[1:3]   # => [2, 4]
li[2:]    # => [4, 3]
li[:3]    # => [1, 2, 4]
li[::2]   # =>[1, 4] //间隔输出
li[::-1]  # => [3, 4, 2, 1] // list反转reverse

li[start:end:step]
```

#### 数组--数组拷贝

``` python 
li2 = li[:]  # => li2 = [1, 2, 4, 3] but (li2 is li) will result in false.
```

#### 数组--使用del从列表中删除任意元素

``` python 
del li[2]  # li is now [1, 2, 3]
```

#### 数组--删除第一次出现的值

``` python 
li.remove(2)  # li is now [1, 3]
li.remove(2)  # Raises a ValueError as 2 is not in the list
```

#### 数组--在特定索引处插入元素

``` python 
li.insert(1, 2)  # li is now [1, 2, 3] again    
```

#### 数组--获取与参数匹配的第一个项目的索引

``` python 
li.index(2)  # => 1
li.index(4)  # Raises a ValueError as 4 is not in the list
```
 
#### 数组--数组相加，原数组不会改变

``` python 
li=[1, 2, 3]
other_li=[4, 5, 6]
li + other_li  # => [1, 2, 3, 4, 5, 6]   
```

#### 数组--in检查是否存在于列表中

``` python 
1 in li  # => True  
```

#### 元组--元组类似列表，但是不可变。

``` python 
tup = (1, 2, 3)
tup[0]      # => 1
tup[0] = 3  # Raises a TypeError
```

#### 元组--注意，长度为1的元组必须在最后一个元素之后使用逗号

``` python 
type((1))   # => <class 'int'>
type((1,))  # => <class 'tuple'>
type(())    # => <class 'tuple'>
```

#### 元组--元组操作

``` python 
a, b, c = (1, 2, 3)      # a is now 1, b is now 2 and c is now 3
a, *b, c = (1, 2, 3, 4)  # a is now 1, b is now [2, 3] and c is now 4
d, e, f = 4, 5, 6        # d is now 4, e is now 5 and f is now 6
e, d = d, e              # d is now 5 and e is now 4 两个值交换
```

#### dictionaries--键值对存储，keys for dictionaries必须是不可变类型。 这是为了确保可以将密钥转换为常量哈希值，以便快速查找。不可变类型包括整数，浮点数，字符串，元组。但是，值可以是任何类型

``` python 
empty_dict = {}
filled_dict = {"one": 1, "two": 2, "three": 3}
invalid_dict = {[1,2,3]: "123"}  # => Raises a TypeError: unhashable type: 'list'
valid_dict = {(1,2,3):[1,2,3]}   
filled_dict["one"]  # => 1
```

#### dictionaries--使用“keys（）”获取所有keys。 使用“values()”获取所有的值，需要用list（）包装调用以将其转换为列表。 注 - 对于Python版本<3.7，不保证字典键排序。 但是，从Python 3.7开始，字典项保持它们插入字典的顺序。

``` python 
list(filled_dict.keys())  # => ["three", "two", "one"] in Python <3.7
list(filled_dict.keys())  # => ["one", "two", "three"] in Python 3.7+
list(filled_dict.values())  # => [3, 2, 1]  in Python <3.7
list(filled_dict.values())  # => [1, 2, 3] in Python 3.7+

"one" in filled_dict  # => True
1 in filled_dict      # => False
```

#### dictionaries--查找不存在的key值会报错，使用get不会报错

``` python 
filled_dict["four"]  # KeyError
filled_dict.get("one")      # => 1
filled_dict.get("four")     # => None
```

#### dictionaries--get方法设置默认参数，如果查询不到key对应的值，则返回默认参数

``` python 
filled_dict.get("one", 4)   # => 1
filled_dict.get("four", 4)  # => 4
```

#### dictionaries--setdefault（）仅在给定键不存在时才插入字典

``` python 
filled_dict.setdefault("five", 5)  # filled_dict["five"] is set to 5
filled_dict.setdefault("five", 6)  # filled_dict["five"] is still 5
```

#### dictionaries--向字典中添加和删除

``` python 
filled_dict.update({"four":4})  # => {"one": 1, "two": 2, "three": 3, "four": 4}
filled_dict["four"] = 4         # another way to add to dict
del filled_dict["one"]  # Removes the key "one" from filled dict
```

#### set() 函数创建一个无序不重复元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等。 

``` python 
empty_set = set()
some_set = {1, 1, 2, 2, 3, 4}  # some_set is now {1, 2, 3, 4}
```

#### set--与字典的键类似，集合的元素必须是不可变的。

``` python 
invalid_set = {[1], 1}  # => Raises a TypeError: unhashable type: 'list'
valid_set = {(1,), 1}
filled_set = {1, 2, 3, 4}
filled_set.add(5)  # filled_set is now {1, 2, 3, 4, 5}
filled_set.add(5)  # it remains as before {1, 2, 3, 4, 5}
```

#### set--与＆设置交集

``` python 
other_set = {3, 4, 5, 6}
filled_set & other_set  # => {3, 4, 5}
```

#### set--用|设置并集

``` python 
filled_set | other_set  # => {1, 2, 3, 4, 5, 6}
```

#### set--用-设置差集

``` python 
{1, 2, 3, 4} - {2, 3, 5}  # => {1, 4}
{2, 3, 5} - {1, 2, 3, 4}  # => {5}
```

#### set--用^设置对称差集 

``` python 
{1, 2, 3, 4} ^ {2, 3, 5}  # => {1, 4, 5}
```

#### set--检查左侧的设置是否是右侧设置的超集

``` python 
{1, 2} >= {1, 2, 3} # => False
```

#### set--检查左侧的set是否是右侧set的子集

``` python 
{1, 2} <= {1, 2, 3} # => True
```

#### set--in检查是否存在

``` python 
2 in filled_set   # => True
10 in filled_set  # => False
```

## 3.控制流和循环

#### if语句。缩进在Python中很重要！约定是使用四个空格，而不是制表符。

``` python 
some_var = 5
if some_var > 10:
    print("some_var is totally bigger than 10.")
elif some_var < 10:    # This elif clause is optional.
    print("some_var is smaller than 10.")
else:                  # This is optional too.
    print("some_var is indeed 10.")
```

#### for循环

``` python 
for animal in ["dog", "cat", "mouse"]:
    print("{} is a mammal".format(animal))
```

#### for in 便历(range返回可迭代的数字,从小到大)

``` python 
for i in range(4):
    print(i)      # 1 2 3 4

for i in range(4, 8):
    print(i)      # 4 5 6 7

for i in range(4, 8, 2):
    print(i)      # 4 6   
```

#### while 循环

``` python 
x = 0
while x < 4:
    print(x)
    x += 1  # 0 1 2 3
```

#### 使用try / except块处理异常

``` python 
try:
    # Use "raise" to raise an error
    raise IndexError("This is an index error")
except IndexError as e:
    pass                 # Pass is just a no-op. Usually you would do recovery here.
except (TypeError, NameError):
    pass                 # Multiple exceptions can be handled together, if required.
else:                    # Optional clause to the try/except block. Must follow all except blocks
    print("All good!")   # Runs only if the code in try raises no exceptions
finally:                 #  Execute under all circumstances
    print("We can clean up resources here")
```

## 4.Functions


#### 使用“def”创建新方法

``` python 
def add(x, y):
    print("x is {} and y is {}".format(x, y))
    return x + y  

add(5, 6)  # => prints out "x is 5 and y is 6" and returns 11
add(y=6, x=5) # => prints out "x is 5 and y is 6" and returns 11  
```

#### 可变数量的参数

``` python 
def manyArgs(*args):
    print(args)
    return args 

manyArgs(1, 2, 3, 4, 5)  # => prints out "(1, 2, 3, 4, 5)" and returns (1, 2, 3, 4, 5)
```

#### 可变数量的关键字参数

``` python 
def keyword_args(**kwargs):
return kwargs

# Let's call it to see what happens
keyword_args(big="foot", loch="ness")  # => {"big": "foot", "loch": "ness"}
```

#### 同时传 可变数量的参数 和 可变数量的关键字参数

``` python 
def all_the_args(*args, **kwargs):
    print(args)
    print(kwargs)

all_the_args(1, 2, a=3, b=4) 
prints:
(1, 2)
{"a": 3, "b": 4} 

args = (1, 2, 3, 4)
kwargs = {"a": 3, "b": 4}
all_the_args(*args)            # equivalent to all_the_args(1, 2, 3, 4)
all_the_args(**kwargs)         # equivalent to all_the_args(a=3, b=4)
all_the_args(*args, **kwargs)  # equivalent to all_the_args(1, 2, 3, 4, a=3, b=4)   
```

#### 返回多个值（以元祖的形式， 值不可变）

``` python 
def swap(x, y):
return y, x  # 将多个值作为元组返回，不带括号。(注意：括号已被排除，但可以包括在内

x = 1
y = 2
x, y = swap(x, y)     # => x = 2, y = 1
```

#### 函数作用域, 局部变量x与全局变量x

``` python 
x = 5

def set_x(num):
    # Local var x not the same as global variable x
    x = num    # => 43
    print(x)   # => 43

def set_global_x(num):
    global x
    print(x)   # => 5
    x = num    # global var x is now set to 6
    print(x)   # => 6

set_x(43)
set_global_x(6)
```

#### 匿名函数

``` python 
(lambda x: x > 2)(3)                  # => True
(lambda x, y: x ** 2 + y ** 2)(2, 1)  # => 5
```

#### python 还有很多内置的高阶函数

``` python 
list(map(max, [1, 2, 3], [4, 2, 1]))  # => [4, 2, 3]
list(filter(lambda x: x > 5, [3, 4, 5, 6, 7]))  # => [6, 7]
[x for x in [3, 4, 5, 6, 7] if x > 5]  # => [6, 7]
{x for x in 'abcddeef' if x not in 'abc'}  # => {'d', 'e', 'f'}
{x: x**2 for x in range(5)}  # => {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

## 5.模块Modules

``` python 
import math
print(math.sqrt(16))  # => 4.0

from math import ceil, floor
print(ceil(3.7))   # => 4.0
print(floor(3.7))  # => 3.0

import math as m
math.sqrt(16) == m.sqrt(16)  # => True
```

#### 查看模块的功能和属性

``` python 
import math
dir(math)   
```

## 6.类Class

``` python 
# 创建一个类
class Human:

    # 一个类属性。 它由此类的所有实例共享
    species = "H. sapiens"

    # 基本初始值设定项，在实例化此类时调用。请注意，双前导和尾随下划线表示Python使用但存在于用户控制的命名空间中的对象或属性,方法（或对象或属性），如：__ init__，__ ttr__，__repr__ etc.被称为特殊方法（或有时称为dunder方法）不应该自己创建这些名称。
    def __init__(self, name):
        # 将参数分配给实例的name属性
        self.name = name

        # 初始化属性
        self._age = 0

    # 实例方法。 所有方法都将“self”作为第一个参数
    def say(self, msg):
        print("{name}: {message}".format(name=self.name, message=msg))

    # 另一种实例方法
    def sing(self):
        return 'yo... yo... microphone check... one two... one two...'

    # 所有实例共享一个类方法,它们以调用类作为第一个参数调用
    @classmethod
    def get_species(cls):
        return cls.species

    # 在没有类或实例引用的情况下调用静态方法
    @staticmethod
    def grunt():
        return "*grunt*"

    # 一个属性就像一个getter.它将方法age（）转换为同名的只读属性。没有必要在Python中编写琐碎的getter和setter。
    @property
    def age(self):
        return self._age

    # 允许设置属性
    @age.setter
    def age(self, age):
        self._age = age

    # 允许删除属性
    @age.deleter
    def age(self):
        del self._age


# 当Python解释器读取源文件时，它会执行所有代码。__name__检查确保仅在此模块是主程序时才执行此代码块。
if __name__ == '__main__':
    # 实例化一个类
    i = Human(name="Ian")
    i.say("hi")                     # "Ian: hi"
    j = Human("Joel")
    j.say("hello")                  # "Joel: hello"
    # i和j是Human类型的实例，换句话说：它们是Human对象

    # 调用我们的类方法
    i.say(i.get_species())          # "Ian: H. sapiens"

    # 更改共享属性
    Human.species = "H. neanderthalensis"
    i.say(i.get_species())          # => "Ian: H. neanderthalensis"
    j.say(j.get_species())          # => "Joel: H. neanderthalensis"

    # 调用静态方法
    print(Human.grunt())            # => "*grunt*"

    # 无法使用对象实例调用静态方法
    # 因为i.grunt（）会自动将“self”（对象i）作为参数
    print(i.grunt())                # => TypeError: grunt() takes 0 positional arguments but 1 was given

    # 更新此实例的属性
    i.age = 42

    # 获取属性
    i.say(i.age)                    # => "Ian: 42"
    j.say(j.age)                    # => "Joel: 0"

    # 删除该属性
    del i.age
    # i.age                         # => this would raise an AttributeError
```

#### inheritance类的继承

