---
layout: post
title: Python Moudle sys hashlib configparser
key: 20180412
tags: Python
---

# Python Moudle sys hashlib configparser

## sys moudle

### sys moudle用户

与Python解释器交互

### sys.argv 传入参数，默认第一个是脚本路径

#### 代码如下：

注意：每个位置都需要传入值，没有就传入空值

<pre>
import sys
print(sys.argv)

def post():
    print('OK')
def download():
    print('Error')
if sys.argv[1] == 'OK':
    print('Post')
    post()
elif sys.argv[2] == 'download':
    print('Download')
    download()
</pre>

### 正常退出和显示Python版本sys.exit(),sys.version

#### 代码如下：

<pre>
import sys

# 正常退出程序
# sys.exit()
# 打印Python版本
print(sys.version)
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/sys_hashlib_configparser.py
3.6.4 (v3.6.4:d48eceb, Dec 19 2017, 06:54:40) [MSC v.1900 64 bit (AMD64)]
</pre>

### 打印Python环境路径，显示平台，标准输出sys.path,sys.platfrom,sys.stdout.write()

#### 代码如下：

<pre>
import sys
# 打印模块路劲
print(sys.path)
# 返回平台名称
print(sys.platform)
# 标准输出
sys.stdout.write('Please ...')
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/sys_hashlib_configparser.py
['G:\\new\\fullstack_s2\\week4\\day18', 'G:\\new\\fullstack_s2', 'C:\\Python36\\python36.zip', 'C:\\Python36\\DLLs', 'C:\\Python36\\lib', 'C:\\Python36', 'C:\\Python36\\lib\\site-packages']
win32
Please ...
</pre>

## hashlib

### 简单明文加密转化

#### 代码如下
<pre>
import hashlib
ps = hashlib.md5()
print(ps)
ps.update(b'Hello world.')
# 转化成二进制
print(ps.digest())
# 转化成十六进制
print(ps.hexdigest())
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/sys_hashlib_configparser.py
<md5 HASH object @ 0x00000000024EA3C8>
b'vEi\xe5\x8fS\xea\x8bd\x04\xf6\xfa\x7f\xc0$\x7f'
764569e58f53ea8b6404f6fa7fc0247f
</pre>

### 明文加密复杂算法

#### 代码如下：

<pre>
import hashlib

ps = hashlib.sha3_256()
ps.update(b'chadwick')
print(ps.hexdigest())
ps256 = hashlib.sha256()
ps256.update(b'chadwick')
print(ps256.hexdigest())
ps512 = hashlib.sha512()
ps512.update(b'chadwick')
print(ps512.hexdigest())
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/sys_hashlib_configparser.py
77e06c3b107a55840126fa41458d0b2bea0d57b9b71424e9d2ca75489edff62b
efae85d0aa7cb7be142be8f280eb733e3cad2849d71bac4251db3ec0a4e39d60
80a198fb15ca360e2108bc14d37164b1c78c97d6c6e469546bf7ae3daf014a512503bf8098d72218d0a94205a140427ca5f19ff8f7059235d194bb08acff5d71
</pre>

## configparser

### configparser基本操作

注意:Python2.x和Python3.x名称有差别

#### 代码如下：

<pre>
import configparser
# 使用定义变量
config_file = configparser.ConfigParser()
# 不通写入内容的方法
config_file['DEFAULT'] = {'ServerAliveInterval':'45',
                          'Compression':'Yes',
                          'CompressionLevel':'6',
                          }
config_file['blog.maihill.com'] = {}
config_file['blog.maihill.com']['User'] = 'Chadwick'
config_file['www.mailhill.com'] = {}
maihill = config_file['www.mailhill.com']
maihill['Host Port'] = '52000'
maihill['ForwardX11'] = 'yes'
config_file['DEFAULT']['ForwardX11'] = 'yes'
# 写入内容写入到文件
with open('chadwick.ini','w',encoding='utf-8') as files_one:
    config_file.write(files_one)
# 查看最后
print(config_file.sections())
# 传统查看方法
config_file.read('chadwick.ini')
# 查看默认模块(全部显示)
print(config_file.defaults())
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/sys_hashlib_configparser.py
['blog.maihill.com', 'www.mailhill.com']
OrderedDict([('serveraliveinterval', '45'), ('compression', 'Yes'), ('compressionlevel', '6'), ('forwardx11', 'yes')])
</pre>

#### 写入文件效果

<pre>
[DEFAULT]
serveraliveinterval = 45
compression = Yes
compressionlevel = 6
forwardx11 = yes

[blog.maihill.com]
user = Chadwick

[www.mailhill.com]
host port = 52000
forwardx11 = yes
</pre>

### configparser增删改查

#### 代码如下：

<pre>
import configparser
# 使用定义变量
config_file = configparser.ConfigParser()
# 不通写入内容的方法
config_file['DEFAULT'] = {'ServerAliveInterval':'45',
                          'Compression':'Yes',
                          'CompressionLevel':'6',
                          }
config_file['blog.maihill.com'] = {}
config_file['blog.maihill.com']['User'] = 'Chadwick'
config_file['www.mailhill.com'] = {}
maihill = config_file['www.mailhill.com']
maihill['Host Port'] = '52000'
maihill['ForwardX11'] = 'yes'
config_file['DEFAULT']['ForwardX11'] = 'yes'
# 写入内容写入到文件
with open('chadwick.ini','w',encoding='utf-8') as files_one:
    config_file.write(files_one)
# # 查看最后
# print(config_file.sections())
# # 传统查看方法
# config_file.read('chadwick.ini')
# # 查看默认模块(全部显示)
# print(config_file.defaults())

# 取值
print(config_file['DEFAULT']['compression'])
# 循环(default会出现)
for i in config_file['blog.maihill.com']:
    print(i)
# 删除
# config_file.remove_section('blog.maihill.com')
# config_file.write(open('chris.ini','w'))
# 判断模块是否存在
print(config_file.has_section('blog.maihill.com'))
# 增加并写入文件
config_file.set('www.mailhill.com','User','kevin')
config_file.write(open('chris.ini','a'))
# 删除模块下的键
config_file.remove_option('www.mailhill.com','User')
config_file.write(open('chris.ini','w'))
</pre>

#### 代码执行效果：

<pre>
C:\Python36\python.exe G:/new/fullstack_s2/week4/day18/sys_hashlib_configparser.py
Yes
user
serveraliveinterval
compression
compressionlevel
forwardx11
True
</pre>