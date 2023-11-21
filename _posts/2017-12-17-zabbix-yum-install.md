---
layout: post
title: Zabbix Yum Install
key: 20171217
tags: Zabbix
---


## zabbix安装基础环境配置
>安装zabbix的yum源，这里有必要提一点，阿里的yum源已经提供了zabbix3.0。因此我们不需要使用官方yum源。当然如果你的网络足够好，也可以尝试使用官方yum源，本次安装以CentOS 7系统来进行安装，在安装之前做好关闭firewalld及selinux准备，并做好时间同步前提。

### yum源配置(阿里云yum源)

<pre>
rpm -ivh http://mirrors.aliyun.com/zabbix/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1.el7.noarch.rpm
</pre>

### 安装相关软件

<pre>
yum install zabbix-server zabbix-web zabb-server-mysql zabbix-web-mysql mariadb-server mariadb zabbix-agent -y
</pre>

### 修改PHP时区配置

#### 方法一：

<pre>
sed -i 's@# php_value date.timezone Europe/Riga@php_value date.timezone Asia/Shanghai@g' /etc/httpd/conf.d/zabbix.conf
</pre>

#### 方法二：

>直接使用vim修改/etc/httpd/conf.d/zabbix.conf文件

>**注意：**这里修改的是/etc/httpd/conf.d/zabbix.conf而不是/etc/php.ini文件


## PHP编译安装

### PHP程序下载

<pre>
wget http://mirrors.sohu.com/php/php-5.6.9.tar.gz
tar zxf php-5.6.9.tar.gz 
cd php-5.6.9
</pre>

### 编译安装PHP

>依赖包安装：

>**注意**：有些包需要第三方epel源，需要先安装epel源的情况下，在安装这些依赖包。

<pre>
yum install –y zlib-devel libxm12-devel libjpeg-devel libjpeg-turbo-devel libpng-devel gd-devel libcurl-devel libxslt-devel openssl-devel freetype-devel libmcrypt-devel mhash mcrypt
</pre>

### PHP-5.6.9编译安装配置启动

<pre>
./configure --prefix=/usr/local/php-5.6.9 --with-mysql=mysqlnd --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --with-gettext --with-mysqli --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline
-optimization --with-curl --enable-mbregex --enable-fpm --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-penssl --with-mhash --enable-pentl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --enable-short-tags --enable-static --with-xsl --with-fpm-user=www --with-fpm-group=www --enable-ftp --enable-opcache=no
make && make install
ln -s /usr/local/php-5.6.9/ /usr/local/php
cp php.ini-production /usr/local/php/lib/php.ini
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
</pre>

## 数据库配置

>在CentOS7上如果通过yum方式安装的话，已经找不到MySQL数据库了，已经变成了mairdb，启动方式略有不同。

### 启动数据

<pre>
systemctl start mariadb
</pre>

### 创建zabbix所用的数据库及库名

<pre>
mysql     #进入mariadb数据库
create dabatase zabbix character set utf8 collate utf8)bin;      #创建数据库
grant all on zabbix.* to zabbix@'localhost' dentified by '123456'; #给数据库授权
exit    #退出数据库
</pre>

### 导入数据库

>早期的版本可以使用mysql命令直接导入，zabbix3.0版本数据库变成了create.sql.gz，需要使用zcat命令查看内容。

<pre>
cd /usr/share/doc/zabbix-server-mysql-3.0.5
zcat cerate.sql.gz | mysql -uzabbix -p123456 zabbix
</pre>

## Zabbix配置

### 修改zabbix-server配置文件/etc/zabbix/zabbix-server.conf
<pre>
vim /etc/zabbix/zabbix_server.conf
# 修改内容
DBHost=localhost  #数据库所在主机
DBName=zabbix     #数据库名
DBUser=zabbix     #数据库用户名
DBPassword=123456 #数据库密码
</pre>

### 启动Zabbix及http服务

<pre>
systemctl start zabbix-server
systemctl start zabbix-agent
systemctl start httpd
# 启动httpd服务报错CentOS内核问题
# 执行：
yum upgrade 更新重启就解决
</pre>

### 网页配置

>**注意**：
配置数据库的端口使用的是mariadb数据库端口是3306
>
密码，用户名是自行设置的。完成后登陆。
>
登陆的用户名是:**Adimin**密码是:**zabbix**登陆后的第一件事应该是修改密码。
