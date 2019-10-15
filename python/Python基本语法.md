[TOC]
# 基本知识
## 数据结构
### 数
Python中有4种类型的数——整数、长整数、浮点数和复数
```
2——整数
长整数——相当于java的BigInteger
浮点数——3.23和52.3E-4，52.3E-4表示52.3 * 10-4。
复数——(-5+4j)和(2.3-4.6j)
```
<!-- more -->

### 字符串
- 1.使用单引号、双引号或者三引号（多行）
- 2.自然字符串——如果你想要指示某些不需要如转义符那样的特别处理的字符串
- 3.字符串是不可变的

### 列表
- 1.list是处理一组有序项目的数据结构
- 2.列表使用
```
创建：classmates = ['Michael', 'Bob', 'Tracy']
计算列表元素个数：len(classmates)
索引访问list元素：classmates[-1]倒数第一个
添加元素：classmates.append('Adam')
删除元素：classmates.pop(i)
添加元素：classmates.insert(1, 'Jack')
列表中可以是不同元素： s = ['python', 'java', ['asp', 'php'], 'scheme']
```
### 元组(tuple)
- 1.tuple一旦初始化就不能修改的list,没有append()，insert()方法
- 2.tuple的不变指每个元素，指向永远不变，要创建一个内容也不变的tuple那就必须保证tuple的每一个元素本身也不能变；
```
t = ('a', 'b', ['A', 'B'])
t[2][0] = 'X' 可以改变list的值
singleton = (2 , )含有一个元素的元组
```
### 字典
- 1.使用键（名字）和值（详细情况）来存储对象，键必须是唯一的
- 2.只能使用不可变的对象（比如字符串）来作为字典的键，但是你可以不可变或可变的对象作为字典的值
- 3.字典使用
```
创建：ab = {'Swaroop':'swaroopch@byteofpython.info','Spammer'  :'spammer@hotmail.com'}
删除：del ab['Spammer']
添加：ab['Guido'] = 'guido@python.org'
```
## 运算符
|运算符|名称|说明                                                           |
|--- |----|----                                                             |
|+	|加	|两个对象相加（字符串和整数）                                       |
|-	|减	|得到负数或是一个数减去另一个数                                     |
|*	|乘	|两个数相乘或是返回一个被重复若干次的字符串                         |
|**	|幂	|返回x的y次幂                                                       |
|/	|除	|x除以y                                                             |
|//	|取整除	|返回商的整数部分                                               |
|%	|取模	|返回除法的余数                                                 |
|<<	|左移	|把一个数的比特向左移一定数目                                   |
|>>	|右移	|把一个数的比特向右移一定数目                                   |
|&	|按位与	|数的按位与                                                     |
|竖线	|按位或	|数的按位或	                                                |
|^	|按位异或	|数的按位异或                                               |
|~	|按位翻转	|x的按位翻转是-(x+1)                                        |
|<	|小于	|返回x是否小于y                                                 |
|>	|大于	|返回x是否大于y                                                 |
|<=	|小于等于	|返回x是否小于等于y(所有比较运算符返回1表示真，返回0表示假) |
|>=	|大于等于	|返回x是否大于等于y                                         |
|==	|等于	|比较对象是否相等                                               |
|!=	|不等于	|比较两个对象是否不相等                                         |
|not	|布尔“非”	|如果x为True，返回False。如果x为False，它返回True       |
|and	|布尔“与”	|如果x为False，x and y返回False，否则它返回y的计算值    |
|or	|布尔“或”	|如果x是True，它返回True，否则它返回y的计算值               |
                                                                            |
## 控制流                                                                   |
### 条件判断                                                                |
如果 条件为真，我们运行一块语句（称为 if-块 ）， 否则 我们处理另外一块语句  |
```
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```

例如
```
guess = int(raw_input('Enter an integer : '))

if guess == number:
    print 'Congratulations, you guessed it.' # New block starts here
    print "(but you do not win any prizes!)" # New block ends here
elif guess < number:
    print 'No, it is a little higher than that' # Another block
    # You can do whatever you want in a block ...
else:
    print 'No, it is a little lower than that' 
    # you must have guess > number to reach here

print 'Done'
```
### while循环
只要在一个条件为真的情况下，while语句允许你重复执行一块语句
```
number = 23
running = True

while running:
    guess = int(raw_input('Enter an integer : '))

    if guess == number:
        print 'Congratulations, you guessed it.' 
        running = False # this causes the while loop to stop
    elif guess < number:
        print 'No, it is a little higher than that' 
    else:
        print 'No, it is a little lower than that' 
else:
    print 'The while loop is over.' 
    # Do anything else you want to do here

print 'Done'
```
### for循环
for..in是另外一个循环语句，它在一序列的对象上递归即逐一使用队列中的每个项目。
```
for i in range(1, 5):
    print i
```
注：python也有break和continue和其他语言一样
## 函数
使用def定义函数
- 1.函数定义和调用
```
def sayHello():
    print 'Hello World!' # block belonging to the function
sayHello()
```
# 面向对象
## 类和实例
### 类的定义
使用class关键字，object代表继承的类名
```
class classname(object):
    pass

class = classname()
```
### 构造函数
__init__方法在类的一个对象被建立时，马上运行。这个方法可以用来对你的对象做一些你希望的 初始化 。
```
class Person:
    def __init__(self, name):
        self.name = name
```
和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量self，并且，调用时，不用传递该参数。
### 数据封装
在Python中，实例的变量名如果以__开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问。
注：在Python中，变量名类似__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的。
## 继承
当我们定义一个class的时候，可以从某个现有的class继承，新的class称为子类（Subclass），而被继承的class称为基类、父类或超类（Base class、Super class）
```
class Animal(object):
    def run(self):
        print('Animal is running...')
```
## 多态
当子类和父类都存在相同的方法时，我们说，子类的方法覆盖了父类的方法，在代码运行的时候，总是会调用子类的方法。
## 其他重要特征
> 以双下划线开头有特殊用途
### __slots__
__slots__变量可以用来限制该class实例能添加的属性，定义该变量后只能添加name、age属性
```
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
### __str__()
定义该方法，当调用print打印类时，可以直接调用该方法；
### __iter__ 和 __getitem__
实现_iter__方法可以使类用于for...in循环，实现__getitem__()可以让类用于切片；


