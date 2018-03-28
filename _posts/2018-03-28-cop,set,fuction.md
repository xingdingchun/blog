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







## function(函数)