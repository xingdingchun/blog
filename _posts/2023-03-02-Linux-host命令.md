---
layout: post
title: Linux host 命令
key: 20230302
tags: 网络安全
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# Linux  host 命令



## Linux host 命令



### Linux host 命令介绍
> host 命令是常用的[域名解析工具，可以用来测试域名系统工作是否正常。一个用于执行 DNS 查找的简单实用程序。它通常用于将域名转换为IP地址，反之亦然。



### Linux host 命令安装

<u>注：host nslookup dig 命令包含在bind-utils包里面</u>

```
yum install -y bind-utils
```



### Linux host 命令参数说明



```shell
host: illegal option -- -
Usage: host [-aCdilrTvVw] [-c class] [-N ndots] [-t type] [-W time]
            [-R number] [-m flag] hostname [server]
       -a is equivalent to -v -t ANY  查询DNS详细信息相当于 -v -t
       -c specifies query class for non-IN data 指定查询类型，默认为IN
       -C compares SOA records on authoritative nameservers 查询指定主机的完整的SOA记录
       -d is equivalent to -v
       -i IP6.INT reverse lookups
       -l lists all hosts in a domain, using AXFR
       -m set memory debugging flag (trace|record|usage)
       -N changes the number of dots allowed before root lookup is done
       -r disables recursive processing 禁用递归处理
       -R specifies number of retries for UDP packets
       -s a SERVFAIL response should stop query
       -t specifies the query type 指定查询类型 包括a、all、mx、ns 
       -T enables TCP/IP mode
       -v enables verbose output 显示指令执行的详细信息
       -V print version number and exit
       -w specifies to wait forever for a reply 如果域名服务器没有给出应答信息，则总是等待，直到域名服务器给出应答
       -W specifies how long to wait for a reply 指定域名查询的最长时间，如果在指定时间内域名服务器没有给出应答信息则退出
       -4 use IPv4 query transport only 使用 IPv4 查询传输(默认)
       -6 use IPv6 query transport only 使用 IPv6 查询传输
```



## Linux host 运用实例



### 查询域名对应的 IP 地址



    host www.epal.gg
    d31vbts4lh9wj4.cloudfront.net has address 52.222.214.72
    d31vbts4lh9wj4.cloudfront.net has address 52.222.214.14
    d31vbts4lh9wj4.cloudfront.net has address 52.222.214.33
    d31vbts4lh9wj4.cloudfront.net has address 52.222.214.115
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:5400:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:8800:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:b600:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:c800:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:aa00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:b400:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:6200:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:223d:4400:8:739c:7c00:93a1



### 显示执行域名查询的详细信息



    host -v www.epal.gg
    Trying "www.epal.gg"
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 43330
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 0
    
    ;; QUESTION SECTION:
    ;www.epal.gg.			IN	A
    
    ;; ANSWER SECTION:
    www.epal.gg.		60	IN	CNAME	d31vbts4lh9wj4.cloudfront.net.
    d31vbts4lh9wj4.cloudfront.net. 60 IN	A	52.222.214.72
    d31vbts4lh9wj4.cloudfront.net. 60 IN	A	52.222.214.33
    d31vbts4lh9wj4.cloudfront.net. 60 IN	A	52.222.214.14
    d31vbts4lh9wj4.cloudfront.net. 60 IN	A	52.222.214.115
    
    Received 136 bytes from 61.139.2.69#53 in 4 ms
    Trying "d31vbts4lh9wj4.cloudfront.net"
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28540
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 8, AUTHORITY: 0, ADDITIONAL: 0
    
    ;; QUESTION SECTION:
    ;d31vbts4lh9wj4.cloudfront.net.	IN	AAAA
    
    ;; ANSWER SECTION:
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:a800:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:7000:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:f400:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:c800:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:2a00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:9a00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:bc00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net. 60 IN	AAAA	2600:9000:223d:6200:8:739c:7c00:93a1
    
    Received 271 bytes from 61.139.2.69#53 in 78 ms
    Trying "d31vbts4lh9wj4.cloudfront.net"
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31797
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 0
    
    ;; QUESTION SECTION:
    ;d31vbts4lh9wj4.cloudfront.net.	IN	MX
    
    Received 47 bytes from 61.139.2.69#53 in 117 ms



### 查询域名的 MX 信息



    host -t MX www.epal.gg
    www.epal.gg is an alias for d31vbts4lh9wj4.cloudfront.net.



### 显示详细的 DNS 信息



    host -a www.epal.gg
    Trying "www.epal.gg"
    Received 29 bytes from 61.139.2.69#53 in 1 ms
    Trying "www.epal.gg"
    Host www.epal.gg not found: 5(REFUSED)
    Received 29 bytes from 61.139.2.69#53 in 4 ms



### 用指定DNS来查主机的 IP



    host www.epal.gg 8.8.8.8
    Using domain server:
    Name: 8.8.8.8
    Address: 8.8.8.8#53
    Aliases: 
    
    www.epal.gg is an alias for d31vbts4lh9wj4.cloudfront.net.
    d31vbts4lh9wj4.cloudfront.net has address 13.225.78.95
    d31vbts4lh9wj4.cloudfront.net has address 13.225.78.90
    d31vbts4lh9wj4.cloudfront.net has address 13.225.78.7
    d31vbts4lh9wj4.cloudfront.net has address 13.225.78.82
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:d200:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:4600:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:3c00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:5a00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:9a00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:9c00:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:c000:8:739c:7c00:93a1
    d31vbts4lh9wj4.cloudfront.net has IPv6 address 2600:9000:21f3:fe00:8:739c:7c00:93a1
