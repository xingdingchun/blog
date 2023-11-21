---
layout: post
title: mosquitto
key: 20190829
tags: Linux-Command
---
>每天记录一点点，时间长了，就是博客，就是书籍！

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# Mosquitto

## Eclipes Mosquitto 介绍

>Eclipse Mosquitto是一个开源(EPL/EDL许可)消息代理，它实现了MQTT协议版本5.0,3.1.1和3.1。Mosquitto重量轻，适用于从低功率单板计算机到完整服务器的所有设备。MQTT协议提供了一种使用发布/订阅模型执行消息传递的轻>量级方法。这使其适用于物联网消息传递，例如低功率传感器或移动设备，如电话，嵌入式计算机或微控制器。Mosquitto项目还提供了一个用于实现MQTT客户端的C库，以及非常流行的mosquitto_pub和mosquitto_sub命令行MQTT客户>端。Mosquitto是Eclipse Foundation的一部分，是一个iot.eclipse.org 项目，由cedalo.com赞助。[Mosquitto官方网站,提供下载软件及技术支持有友好的社区支持](https://mosquitto.org/ "Mosquitto官方网站")

## Mosquitto安装

>当前安装环境使用CentOS系统，其他版本系统未测试

### 软件版本选择及下载

    当前社区最新版本为1.6.2，测试使用版本为1.6.2
    cd /usr/local/src && wget https://mosquitto.org/files/source/mosquitto-1.6.2.tar.gz && tar xf mosquitto-1.6.2.tar.gz && cd mosquitto-1.6.2

### Mosquitto依赖软件安装

    yum install -y c-ares-devel e2fsprogs-devel uuid-devel libuuid-devel openssl openssl-devel gcc gcc-c++

### Mosquitto用户创建

    useradd mosquitto -s /sbin/nologin

### Mosquitto编译安装

    make && make install

### 编译安装完成后显示路径信息

    /usr/local/lib
    /usr/local/include/
    /usr/local/lib/pkgconfig
    /usr/local/bin
    /usr/local/bin/mosquitto_pub
    /usr/local/bin/mosquitto_sub
    /usr/local/bin/mosquitto_rr
    /usr/local/sbin
    /usr/local/sbin/mosquitto
    /usr/local/bin/mosquitto_passwd
    /etc/mosquitto
    /etc/mosquitto/mosquitto.conf.example
    /etc/mosquitto/aclfile.example
    /etc/mosquitto/pwfile.example
    /etc/mosquitto/pskfile.example

### 完成安装Mosquitto保障命令行使用命令不报错

>执行如下命令，有报错忽略。

    cp /usr/local/lib/libmosquitto.so.1 /usr/local/lib
    ln -s /usr/local/lib/libmosquitto.so.1 /usr/lib/libmosquitto.so.1
    ldconfig

### 查看Mosquitto版本

    mosquitto -v

## Mosquitto配置文件详解

###  配置文件复制

    cp /etc/mosquitto/mosquitto.conf.example /etc/mosquitto/mosquitto.conf

### Mosquitto启动

    mosquitto -c /etc/mosquitto/mosquitto.conf -d  # -c 指定配置文件路径; -d 转到后台运行

### Mosquitto配置文件解释

    user mosquitto                                     # Mosquitto使用的用户(不建议直接使用root用户启动)
    bind_address 172.18.42.155                         # Mosquitto绑定的IP地址(默认是0.0.0.0) 
    port 1883                                          # Mosquitto绑定的端口号(默认是1883)
    persistence true                                   # Mosquitto数据持久化开启(数据存储到磁盘，默认是不开启，数据直接存储在内存中)
    persistence_file mosquitto.db                      # Mosquitto数据持久化文件名称
    persistence_location /data/store/logs/mosquitto/   # Mosquitto数据持久化存储路径
    allow_anonymous false                              # 禁止无用户登录(默认是任何用户都可以连接)
    password_file /etc/mosquitto/pskfile               # Mosquitto用户名密码存储路径

## Mosquitto命令简单使用

    mosquitto_sub -u farm -P 123456 -t mqtt  # 查看消息; -u 指定用户; -P 指定密码; -t 指定消息列队 
    mosquitto_pub -u farm -P 123456 -h localhost -p 1883 -t mqtt -m "Hello World"  # 发送消息; -u 指定用户; -P 指定密码; -h 连接地址; -p 指定端口; -t 指定消息列队; -m 发送的消息;

## 启动脚本配置

>实际运用中采用supervisord软件集中管理

    cd /etc/systemd/system  # 在这个目录
    vim mosquitto.service  # 创建文件并写入一下内容

    [Unit]
    Description=Mosquitto MQTT v3.1/v3.1.1 Broker
    Documentation=man:mosquitto.conf(5) man:mosquitto(8)
    After=network-online.target
    Wants=network-online.target

    [Service]
    Type=notify
    NotifyAccess=main
    ExecStart=/usr/local/sbin/mosquitto -c /etc/mosquitto/mosquitto.conf
    Restart=on-failure

    [Install]
    WantedBy=multi-user.target

    systemctl daemon-reload # 执行命令
    systemctl enable mosquitto.service # 加入开机启动项
    systemctl start mosquitto.service  # 启动mosquitto

## Mosquitto集群

>注：mosquitto官方提供的集群方案为桥接方案；经过测试发现桥接不能真正实现集群，仅仅是数据的串联；有可能是测试的方式问题，有需要可以自行测试；
>GitHub上提供的开源mosquitto集群方案可以实现mosquitto真正的集群，但已经很久没有更新了，当前版本为mosquitto 1.5.0版本；[mosquitto开源集群方案GitHub地址](https://github.com/hui6075/mosquitto-cluster "mosquitto集群方案")

### mosquitto集群安装

    yum install -y c-ares-devel e2fsprogs-devel uuid-devel libuuid-devel openssl openssl-devel gcc gcc-c++ # 安装依赖包(库)，注意这个依赖和mosquitto安装的有点不一样
    yum -y install docbook-style-xsl  # man文档添加
    useradd mosquitto -s /sbin/nologin  # 创建mosquitto用户
    find / -type f -name "docbook.xsl"  # 查找docbook.xsl路径
    /usr/share/sgml/docbook/xsl-stylesheets-1.78.1/manpages/docbook.xsl  # 需要添加的路径
    vim man/manpage.xsl  # mosquitto-cluster下修改文件，替换为以上内容
    make && make install  # 编译安装

### 集群配置文件添加

>注：mosquitto集群是以topic为基础的集群

    # 每个节点相同配置
    connection agrtest
    address 192.168.89.13:1883
    topic /#

    connection agrtest
    address 192.168.89.14:1883
    topic /#

    connection agrtest
    address 192.168.89.16:1883
    topic /#