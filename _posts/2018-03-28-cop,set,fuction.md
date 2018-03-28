---
layout: post
title: copy,set,function
key: 20180328
tags: Python
---

# copy,set,function(深浅拷贝，集合，函数)

## copy(深浅拷贝)

> 浅拷贝 ===> 只拷贝第一层
>
> 深拷贝 ===> 完全拷贝，复制一份

### 浅拷贝，只拷贝第一层，没有变化

#### 代码

<pre>
# 需求，需要同时使用一个列表，
list_one = ['chadwick','kevin','chris','pony']
list_tow = ['gavin','kevin','chris','pony']
list_tow[0] = 'bill'
print(list_one,list_tow)
# list拷贝
list_three = ['sichuan','chengdu','chenghua']
list_four =  list_three.copy()
print(list_three,list_four)
</pre>

#### 代码执行效果
<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/copy.py
['chadwick', 'kevin', 'chris', 'pony'] ['bill', 'kevin', 'chris', 'pony']
['sichuan', 'chengdu', 'chenghua'] ['sichuan', 'chengdu', 'chenghua']
</pre>

### 深拷贝，拷贝多层，变化(没有完全拷贝)

#### 代码

<pre>
# 多层list copy
list_one = [['chadwick','chris'],'kevin','peter',]
list_tow = list_one.copy()
list_tow[1] = 'pony'
print(list_one,list_tow)
list_tow[0][0] = 'gavin'
print(list_one,list_tow)
</pre>

#### 代码执行效果

<pre>
[['chadwick', 'chris'], 'kevin', 'peter'] [['chadwick', 'chris'], 'pony', 'peter']
[['gavin', 'chris'], 'kevin', 'peter'] [['gavin', 'chris'], 'pony', 'peter']
</pre>

### 深浅拷贝图示说明

#### 浅拷贝，只复制第一层，没有变化

![copy_shallow](https://raw.githubusercontent.com/xingdingchun/chadwick/master/image/python/copy_shallow.jpg "copy_shallow")

#### 深拷贝，全部复制，内存地址发生改变，变化

![copy_deep](https://raw.githubusercontent.com/xingdingchun/chadwick/master/image/python/copy_deep.jpg "copy_deep")

### 浅拷贝运用

#### 代码

<pre>
# 浅拷贝的用途
# 创建联合账户
account = ['chadwick',980012345,[10000,10000]]
flower = account.copy()
flower[0] = 'flower'
flower[1] ='980012346'
account[2][1] -= 3000
print(flower)
</pre>

#### 代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/copy.py
['flower', '980012346', [10000, 7000]]
</pre>

### 深拷贝，克隆，完全复制

#### 代码

<pre>
# 深拷贝，完全克隆
import copy

account = ['chadwick',980012345,[10000,10000]]
flower = account.copy()
flower[0] = 'flower'
flower[1] = '980012346'

kevin = copy.deepcopy(account)
kevin[0] = 'kevin'
kevin[1] = '980012347'

flower[2][1] -= 3000
kevin[2][1] -= 5000

print(flower)
print(kevin)
</pre>


## set(集合)

> 把不通的元素组成在一起就是Python中的集合

### 集合的创建(list，tuple，dict)

#### 代码

<pre>
# list 创建
list_one = ['chadwick','kevin','chris']
list_tow = list(('pony','gavin'))
print(list_one)
print(list_tow)
# tuple 创建
tuple_one = ('sichuan','shandong')
tuple_tow = tuple(('guizhou','yunnan'))
print(tuple_one)
print(tuple_tow)
# dict 创建
dict_one = {'name':'chadwick','age':30}
dict_tow = dict([('name','chadwick'),('age',30),('job','IT')])
print(dict_one)
print(dict_tow)
# set 创建
set_one = set('chadwick')
print(type(set_one))
print(set_one)
set_tow = ['kevin','gavin','pony','donny']
set_three = set(set_tow)
print(set_three)
print(type(set_three))
</pre>

#### 代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
['chadwick', 'kevin', 'chris']
['pony', 'gavin']
('sichuan', 'shandong')
('guizhou', 'yunnan')
{'name': 'chadwick', 'age': 30}
{'name': 'chadwick', 'age': 30, 'job': 'IT'}
<class 'set'>
{'d', 'k', 'h', 'w', 'a', 'c', 'i'}
{'donny', 'kevin', 'gavin', 'pony'}
<class 'set'>
</pre>

### set 功能去重

> 1. 去重
> 
> 2. 集合是一组无序的可哈西的值(不重复)，集合可以作为字典的键

#### 代码

<pre>
list_one = ['chadwick','gavin','pony','chadwick']
set_one = set(list_one)
print(set_one)
</pre>

#### 代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
{'chadwick', 'pony', 'gavin'}
</pre>

### 集合分类

> 不可变集合，可以是字典的键
>
> 可变集合，可以添加和删除元素，非可哈西，不能用作字典的键，也不可以作为其他集合的元素

### 集合操作

#### 创建集合

<pre>
set_one = set('chadwick')
set_tow = frozenset('chris')
print(set_one,type(set_one))
print(set_tow,type(set_tow))
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
{'i', 'h', 'c', 'k', 'w', 'a', 'd'} <class 'set'>
frozenset({'i', 's', 'h', 'c', 'r'}) <class 'frozenset'>
</pre>

#### 判断是否在集合内

<pre>
set_one = set('chadwick')
print("c" in set_one)
print('x' in set_one)
list_one = ['kevin','chris','pony']
set_tow = set(list_one)
print('kevin' in set_tow)
print('chadwick' in set_tow)
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
True
False
True
False
</pre>

#### set遍历

<pre>
# 集合遍历
list_one = ['chadwick','gavin','chris','kevin',]
set_one = set(list_one)
for i in set_one:
    print(i)
</pre>

代码执行效果
<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
gavin
kevin
chris
chadwick
</pre>

#### add增加

<pre>
# 添加
list_one = ['chadwick','kevin','pony','chris']
set_one = set(list_one)
set_one.add('donny')
print(set_one)
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
{'chris', 'pony', 'chadwick', 'donny', 'kevin'}
</pre>

#### 更新updae

<pre>
list_one = ['chadwick','chris','kevin','pony']
set_one = set(list_one)
set_one.update('donny')
print(set_one)
set_one.update('chadwick')
print(set_one)
set_one.update(['name','chadwick'])
print(set_one)
</pre>

代码执行效果
<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
{'kevin', 'y', 'n', 'd', 'chris', 'o', 'chadwick', 'pony'}
{'k', 'kevin', 'y', 'n', 'h', 'c', 'a', 'w', 'd', 'i', 'chris', 'o', 'chadwick', 'pony'}
{'k', 'kevin', 'y', 'n', 'h', 'c', 'a', 'w', 'd', 'i', 'chris', 'o', 'chadwick', 'pony', 'name'}
</pre>

#### 集合删除清理

<pre>
list_one = ['chadwick','gavin','chris','kevin']
set_one = set(list_one)
set_one.remove("chadwick")
print(set_one)
set_pop = set_one.pop()
print(set_pop,set_one)
set_one.clear()
print(set_one)
del set_one
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
{'chris', 'gavin', 'kevin'}
chris {'gavin', 'kevin'}
set()
</pre>

### set 集合之间的关系

<pre>
set_one = set([1,2,3,4,5,])
set_tow = set([4,5,6,7,8,])

# intersection 交集
print(set_one.intersection(set_tow))
print('jioaji',set_one & set_tow)

# union 联合，并集
print(set_one.union(set_tow))
print('bingji',set_one | set_tow)

# difference 差集
print(set_one.difference(set_tow))
print(set_tow.difference(set_one))
print('chaji',set_one - set_tow)
print('chaji',set_tow - set_one)

# sysmmetric difference 反向交集，对称差
print(set_one.symmetric_difference(set_tow))
print('duichengcha',set_one ^ set_tow)

# 父集 超集 issuperset
print(set_one.issuperset(set_tow))
print(set_one > set_tow)

# 子集 issubset
print(set_one.issubset(set_tow))
print(set_one < set_tow)
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/Set.py
{4, 5}
jioaji {4, 5}
{1, 2, 3, 4, 5, 6, 7, 8}
bingji {1, 2, 3, 4, 5, 6, 7, 8}
{1, 2, 3}
{8, 6, 7}
chaji {1, 2, 3}
chaji {8, 6, 7}
{1, 2, 3, 6, 7, 8}
duichengcha {1, 2, 3, 6, 7, 8}
False
False
False
False
</pre>

## function(函数)

> 函数，来源于数学。但是编程中的函数概念与数学中的函数概念有很大的区别。数学中函数是function，编程语言中的函数是subroutin子程序或者procodures过程的意思。
>
> 函数能提高应用模块性，代码重复利用率。Python提供很多内置函数，也可以定义自己的函数。
>
> 函数是把一组语句集合通过一个名字(函数名)封装起来，想要执行这个函数(这些语句组)，直接调用函数名即可。
> 
> 函数的作用：
> 
> 1. 减少重复代码
>
> 2. 方便修改，更易扩展
>
> 3. 保持代码一致性

### 函数的创建

> def 关键字(define缩写)，注意函数名称，要定义有描述性意义的函数名称，规则和定义变量一样。

#### 函数的简单定义和调用

<pre>
# 简单定义函数
def test():
    print('OK')
test()
# 简单使用函数的参数，函数定义的参数是形参
# 调用时输入的参数是实参

def add(x,y):
    print(x+y)
add(5,8)
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
OK
13
</pre>

### 定义函数的演变过程

#### 过程一j简单记录日志

<pre>
# 程序记录日志
def action_one(n):
    print('action one startiing...')
    with open('logger.txt','a',encoding='utf-8') as f:
        f.write('Print log file one: %s\n'%n)
def action_tow(n):
    print('action tow startiing...')
    with open('logger.txt','a',encoding='utf-8') as f:
        f.write('Print log file tow: %s\n'%n)
def action_three(n):
    print('action three startiing...')
    with open('logger.txt','a',encoding='utf-8') as f:
        f.write('Print log file three: %s\n'%n)
action_one("chadwick")
action_tow("chadwick")
action_three("chadwick")
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
action one startiing...
action tow startiing...
action three startiing...
</pre>

#### 过程二记录日志需要记录时间

<pre>
import time

def logger_format(m):
    time_format = '%Y-%m-%d %X'
    time_current = time.strftime(time_format)
    with open('logger.txt','a',encoding='utf-8') as loggers:
        loggers.write('%s Print aciont one: %s\n'%(time_current,m))
def action_one(n):
    print("Action one starting...")
    logger_format(n)
def action_tow(n):
    print("Action tow starting...")
    logger_format(n)
def action_three(n):
    print("Action three starting...")
    logger_format(n)
action_one("chadwick")
action_tow("chadwick")
action_three("chadwick")
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
Action one starting...
Action tow starting...
Action three starting...
</pre>

### 参数

> 1.必须参数，必须正确的顺序正确的个数传入
>
> 2.关键字参数，对位置没有要求
>
> 3.默认参数，必须参数之后，其他参数之前(大多数情况下默认赋值的参数)
>
> 4.不定长参数

#### 必须参数(必须位置对应，个数相同，否则会错或者显示效果有误)

<pre>
# 必须参数
def add(name,age):
    print('Name: ',name)
    print('Age: ',age)
add('chadwick',30)
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
Name:  chadwick
Age:  30
</pre>

#### 关键字参数(对位置没有要求，对个数有要求)

<pre>
# 关键字参数
def info(name,age,job):
    print('Name: %s'%name)
    print('Age: %d' %age)
    print('Job: %s' %job)

info(job='IT',name='Chadwick',age=30)
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
Name: Chadwick
Age: 30
Job: IT
</pre>

#### 默认参数(对于默认值的参数，必须参数之后)

<pre>
# 默认参数
def student(name,age,sex='boy'):
    print('Name: %s'%name)
    print('Age: %d' %age)
    print('Sex: %s' %sex)
student('Chadwick',30)
student('Flower',30,'girl')
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
Name: Chadwick
Age: 30
Sex: boy
Name: Flower
Age: 30
Sex: girl
</pre>

#### 不定长参数*args

<pre>
# 不定长参数一
# 加法器
# def add(x,y):
#     print(x+y)
# add(5,8)
# 加法器升级版，想加多少加多少
def add_tow(*args):
    sum = 0
    for i in args:
        sum += i
    print(sum)
add_tow(3,4,6,8,)
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
21
</pre>

#### 不定长参数**kwagrs

<pre>
# 解决出入多个参数问题
# 如果需要添加，一直添加下去
# def info(name,age,job,habby):
#     print('Name: %s'%name)
#     print('Age: %d' %age)
#     print('Job: %s' %job)
#     print('Habby: %s' %habby)
# info(name='Chadwick',age=30,job='IT',habby='book')
# **kwares 拿到多个参数的值
def info(*args,**kwargs):
    print(args)
    print(kwargs)
    for i in kwargs:
        print('%s:%s'%(i,kwargs[i]))
info('chadwick','gavin','kevin',name='chris',job='IT',habby='book')
</pre>

代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/practice/function.py
('chadwick', 'gavin', 'kevin')
{'name': 'chris', 'job': 'IT', 'habby': 'book'}
name:chris
job:IT
habby:book
</pre>

### 参数顺序总结

> 关键参数 <=== 默认参数 <=== *args <=== **kwargs













