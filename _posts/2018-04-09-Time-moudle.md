---
layout: post
title: Python Time Moudel
key: 20180409
tags: Python
---

# Time Moudel

## Time Moudel Function

### The method of using moudles

使用模块钱需要导入模块，导入模块的方法(Python中)是import 模块名称

#### 导入代码如下：

<pre>
import time
</pre>

### See the help

使用help方法

#### 代码如下：

<pre>
improt time

print(help(time))
</pre>

### 生成时间戳

时间戳是指从1970年到现在(当前时间)的秒(过去多少秒)

#### 代码如下：

<pre>
# 生成时间戳
import time
s_time = time.time()
print(s_time)
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
1523506225.8395889
</pre>

### 等待时间sleep(睡眠时间)

间隔多长时间

#### 代码如下：

<pre>
# 等待时间
import time

print('OK one')
time.sleep(1)
print('OK tow')
time.sleep(2)
print('OK three')
</pre>

#### 执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
OK one
OK tow
OK three
</pre>

### CPU工作时间

#### 代码如下：

<pre>
# CPU工作或者执行程序的时间
import time

print(time.clock())
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
6.414796112120372e-07
</pre>

### 打印结构化时间

#### 代码如下：

<pre>
# 结构化时间
import time
print(time.gmtime())
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
time.struct_time(tm_year=2018, tm_mon=4, tm_mday=12, tm_hour=5, tm_min=20, tm_sec=33, tm_wday=3, tm_yday=102, tm_isdst=0)
</pre>

### 打印本地时间(当地)

#### 代码如下：

<pre>
# 打印本地时间
import time
print(time.localtime())
</pre>

#### 执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
time.struct_time(tm_year=2018, tm_mon=4, tm_mday=12, tm_hour=13, tm_min=25, tm_sec=33, tm_wday=3, tm_yday=102, tm_isdst=0)
</pre>

### 自定义格式时间strftime

#### 代码如下：

<pre>
import time
# 自定义时间格式
time_format = '%Y-%m-%d %X'
# 本地时间
struce_time = time.localtime()
# 自定义格式的时间
print(time.strftime(time_format,struce_time))
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
2018-04-12 13:32:17
</pre>

### 时间转换

#### 代码如下：

<pre>
# 时间转换
import time

convert = time.strptime('2018-04-12 13:32:17','%Y-%m-%d %X')
print(convert.tm_year)
print(convert.tm_mon)
print(convert.tm_yday)
print(convert.tm_hour)
print(convert.tm_min)
print(convert.tm_wday)
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
2018
4
102
13
32
3
</pre>

### 时间与时间戳之间相互转换

#### 代码如下：

<pre>
# 当前时间与时间戳之间的转换
import time
# 时间戳转换为时间
print(time.ctime(time.time()))
# 时间转换成时间戳
print(time.mktime(time.localtime()))
</pre>

#### 执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
Thu Apr 12 13:46:44 2018
1523512004.0
</pre>

## datetime moudel

### 打印当前时间

#### 代码如下：

<pre>
# datetime模块
import datetime
# 打印当前时间
print(datetime.datetime.now())
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day17/practice_time_moude.py
2018-04-12 13:49:22.427142
</pre>





