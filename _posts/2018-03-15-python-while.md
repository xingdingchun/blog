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

**程序执行效果**

<pre>
D:\>python36 num_compare.py
Enter one number >>>:1
Enter tow number >>>:2
Enter three number >>>:3
Max number is: 3

D:\>python36 num_compare.py
Enter one number >>>:45
Enter tow number >>>34
Enter three number >>>98
Max number is: 98
</pre>

## 赋值运算

<pre>
= 等于
num += 1 等价于 num = num + 1
num -= 1 等价于 num = num - 1
num *= 2 等价于 num = num * 2
num /= 2 等价于 num = num / 2
num //=2  等价于 num = num // 2
num %= 2 等价于 num = num % 2
num **= 2 等价于 num = num **2
</pre>

**赋值运算测试效果**

<pre>
>>> num = 1
>>> num +=1
>>> num
2
>>> num = num + 1
>>> num
3
>>> num -= 1
>>> num
2
>>> num *= 2
>>> num
4
>>> num /= 2
>>> num
2.0
>>> num = 4
>>> num //= 2
>>> num
2
>>> num = 5
>>> num %= 2
>>> num
1
>>> num = 5
>>> num **= 2
>>> num
25
</pre>

## 逻辑运算符

<pre>
and 且，并且
只有两个条件全部为True(正确)的时候,结果才为True(正确)

and中文 条件1 and 条件2
5>3 and 6>2   True

or  或，或者
只要有一个条件为True，则结果为True
比如：5>3 or 6 <2

not 不
not 5>3   ====False
not 5<3   ====True
</pre>

**逻辑运算测试**

<pre>
>>> 5>3 and 6>2
True
>>> 5<3 and 6>2
False
>>> 5>3 or 6 <2
True
>>> 5<3 or 6 <2
False
>>> 5>3 or 6 >2
True
</pre>

**表达式就是像1-2+3这样的运算式，我们统称为表达式**

**注意：逻辑运算的时候会出现短路原则，就是前面判断以后直接略过后面的，直接出结果**

**比如：**

<pre>
>>> not not True or False and not True
True
>>> True or True and False
True
</pre>

## while循环

while循环结构：

while  条件：

    print("any")


**简单while循环代码：**

<pre>
#!-*- coding:utf-8 -*-

num = 1

while num <= 10:
    print(num)
    num += 1
</pre>

代码执行效果：

<pre>
D:\pythontest>python while.py
1
2
3
4
5
6
7
8
9
10
</pre>

**终止程序循环语句**

break 终止，在循环的时候如果执行break语句退出循环(结束循环)

continue 结束本次循环进入下一次循环

### 猜数字游戏程序

>需求分析：使用Python写一个猜数字的小游戏，实现用户输入数字，如果猜对了，程序退出，如果猜错了，则提示用户是猜大了还是猜小了。

### 程序实现方式一

问题：即使猜正确了程序依然不退出

<pre>
#! -*- coding:utf-8 -*-

number = 50

while True:
    user_input_number = int(input("Enter number is >>>:"))
    if user_input_number == number:
        print("Congratulations yours,guess correct!")
    elif user_input_number > number:
        print("Your guess number too bigger...")
    else:
        print("Your guess number too small...")
</pre>

### 程序实现方式二

<pre>
#! -*- coding:utf-8 -*-

number = 50
flag = True


while flag:
    user_input_number = int(input("Enter number is >>>:"))
    if user_input_number == number:
        print("Congratulations yours,guess correct!")
        flag = False
    elif user_input_number > number:
        print("Your guess number too bigger...")
    else:
        print("Your guess number too small...")
</pre>

### 程序实现方式三

<pre>
#! -*- coding:utf-8 -*-

number = 50

while True:
    user_input_number = int(input("Enter number is >>>:"))
    if user_input_number == number:
        print("Congratulations yours,guess correct!")
        break
    elif user_input_number > number:
        print("Your guess number too bigger...")
    else:
        print("Your guess number too small...")
</pre>

## 使用Python while循环拼出固定图形

>Python while工作的详细过程

![python_while](https://raw.githubusercontent.com/xingdingchun/chadwick/master/image/python/python_while.jpg "python_while")

