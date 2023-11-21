---
layout: post
title: Python random moudle
key: 20180410
tags: Python
---

# Python random moudle

## random moudle

### random功能

#### 代码如下：

<pre>
# random 功能测试
import random
# 生成随机数
print(random.random())
# 在指定范围内随机输出一个数字
print(random.randint(1,5))
# 随机choice选择
print(random.choice('hello'))
print(random.choice(['123',4,[1,2]]))
# 分别组合
print(random.sample(['123',4,[1,2]],2))
# 范围随机
print(random.randrange(1,5))
</pre>

#### 代码执行效果

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_random_moucle.py
0.07440702542256883
2
e
4
['123', [1, 2]]
4
</pre>

## 随机生成验证码脚本

### 随机生成验证码

#### 代码如下：

<pre>
# 随机生成验证码
import random

def verification_code():
    code = ''
    for i in range(5):
       # add = random.choice([random.randrange(10)],[chr(random.randrange(65.91))])
        if i == random.randint(0,3):
            add = random.randrange(10)
        else:
            add = chr(random.randrange(65,91))
        code += str(add)
    print(code)
verification_code()
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_random_moucle.py
I6GTE
</pre>