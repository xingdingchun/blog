---
layout: post
title: Python logging moudle
key: 20180416
tags: Python
---

# logging moudle

## logging module 使用

### logging 简单打印日志

注意：logging本身是有级别的，debug和info默认是不打印的

#### 代码如下：

<pre>
import logging

logging.debug('debug message')
logging.info('info message')
logging.warning('warning message')
logging.error('error message')
logging.critical('critical message')
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/logger_moudle.py
WARNING:root:warning message
ERROR:root:error message
CRITICAL:root:critical message
</pre>

### logging 调整日志输出级别

#### 代码如下：
<pre>
import logging

logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',
                    datefmt='%a,%d %b %Y %H:%M:%S',
                    filename='log_test.log',
                    filemode='a')

logging.debug('debug message')
logging.info('info message')
logging.warning('warning message')
logging.error('error message')
logging.critical('critical message')
</pre>

#### 执行效果(不输出内容，直接写入文件)

<pre>
Tue,17 Apr 2018 16:11:56 logger_moudle.py[line:22] DEBUG debug message
Tue,17 Apr 2018 16:11:56 logger_moudle.py[line:23] INFO info message
Tue,17 Apr 2018 16:11:56 logger_moudle.py[line:24] WARNING warning message
Tue,17 Apr 2018 16:11:56 logger_moudle.py[line:25] ERROR error message
Tue,17 Apr 2018 16:11:56 logger_moudle.py[line:26] CRITICAL critical message
</pre>

### logging 日志选项解释

> filemode 写入模式，默认是a，追加模式，w写入模式，会清空以前的
> 
> filename 写入文件名称，默认是输出到屏幕上
> 
> datefmt 时间格式%a 星期 %d 日 %b 月份 %Y 年份 %H 小时 %M 分钟 %S 秒钟
> 
> format 日志格式设置
> 
> level 调整日志级别，默认是warning


## logging module 写入文件同时显示

### 使用getLogger()函数实现同时写入文件和输出到控制台

#### 代码如下：

<pre>
# 需求日志显示在屏幕上同时写入文件中
# 日志另外一个模块级别的函数

import logging

# 定义函数
chadwick = logging.getLogger()
# 创建一个handler，用于存储日志，文件流的方式
write_log = logging.FileHandler("log_test.log")
# 创建一个handler，用于控制台输出
read_log = logging.StreamHandler()
# 标准流对象(定义一个输出日志的格式)
format_log = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s' )

# 让读取和写入使用定义的格式
write_log.setFormatter(format_log)
read_log.setFormatter(format_log)

# 使用logLogger()函数，输出日志(同时写入和控制台输出)
chadwick.addHandler(write_log)
chadwick.addHandler(read_log)

# 调整日志输出级别
chadwick.setLevel(logging.DEBUG)

logging.debug('debug message getLogger')
logging.info('info message getLogger')
logging.warning('warning message getLogger')
logging.error('error message getLogger')
logging.critical('critical message getLogger')
</pre>

#### 执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/logger_moudle.py
2018-04-17 16:42:51,536 - root - DEBUG - debug message getLogger
2018-04-17 16:42:51,536 - root - INFO - info message getLogger
2018-04-17 16:42:51,536 - root - WARNING - warning message getLogger
2018-04-17 16:42:51,536 - root - ERROR - error message getLogger
2018-04-17 16:42:51,536 - root - CRITICAL - critical message getLogger
</pre>

#### 写入文件效果：

<pre>
2018-04-17 16:42:51,536 - root - DEBUG - debug message getLogger
2018-04-17 16:42:51,536 - root - INFO - info message getLogger
2018-04-17 16:42:51,536 - root - WARNING - warning message getLogger
2018-04-17 16:42:51,536 - root - ERROR - error message getLogger
2018-04-17 16:42:51,536 - root - CRITICAL - critical message getLogger
</pre>