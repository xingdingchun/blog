---
layout: post
title: python-while
key: 20180315
tags: Python
---

## Python运算符

**运算符使用**

<pre>
>>> 1+2       # 加
3
>>> 3-2       # 减
1
>>> 2*3       # 乘
6
>>> 5/2       # 除
2.5
>>> 2**2      # 幂（平方）
4
>>> 5//2      # 地板除(整除)
2
>>> 9//2      # 地板除(整除)
4
>>> 9 % 2     # 取模(取余)
1
>>> 2**10     # 幂
1024
</pre>

**运算符优先级**

Python运算符优先级和我们数学中的优先级一样，先乘除后加减，如果要更明了清晰的体现优先级，最好的办法是加小括号"()"，注意Python中没有中括号和大括号之分，只有小括号，可以多层级加小括号。

<pre>
>>> 2**3*5
40
>>> (2+3)*5
25
>>> (((2+3)+3)+3)*5
55
</pre>

## 比较运算符

比较运算符包括：

<pre>
> ,< ,== ,!=,<= ,>=

True   真  正确的
False  假  错误的
</pre>

**比较运算符使用测试**

<pre>
>>> a = 3
>>> b = 5
>>> a > b
False
>>> a < b
True
>>> a == b
False
>>> a != b
True
>>> a >= b
False
>>> a <= b
True
</pre>

## 条件语句if...else...

>代码需求分析：

>让用户根据提示输入三个数字，并显示出最大的数字

<pre>
#! -*- coding:utf-8 -*-

num_one = int(input("Enter one number >>>:"))
num_tow = int(input("Enter tow number >>>:"))
num_three = int(input("Enter three number >>>:"))

max_num = 0

if num_one > num_tow:
    max_num = num_one
    if max_num > num_three:
        print("Max number is: ",max_num)
    else:
        print("Max number is: ",num_three)
else
    max_num = num_tow
    if max_num >num_three:
        print("Max number is: ",max_num)
    else:
        print("Max number is: ".num_three)
</pre>