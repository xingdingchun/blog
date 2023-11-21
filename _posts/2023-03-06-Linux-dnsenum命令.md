---
layout: post
title: Linux dnsenum 命令
key: 20230306
tags: 网络安全
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# Linux  dnsenum 命令



## Linux dnsenum 命令



### DNS 介绍
> DNS 信息会暴露很多的服务信息，渗透测试项目往往就是从一个域名开始，拿到一个域名后，也往往是先通过 DNS 信息收集的方式获得目标更多的信息。所以掌握 DNS 信息收集的工具是渗透测试者必须的。
> 通过 DNS 信息，可以获取到 IP 地址，根据 IP 地址可以获取同网段其他 IP 地址，而这些 IP 地址后面就能获取到开放的端口号，然后通过端口号可以获取到背后跑的服务。通过 DNS 信息还能获取到一些子域名信息，获取到 DNS 域名注册信息，甚至可能对 DNS 服务器做区域传输，把整个目标后面的信息全部获取到。



### Linux dnsenum命令介绍
> dnsenum 是一款用于收集 DNS 信息的工具，通过字典爆破、搜索引擎、whois 查询、区域传输等手段，获取域名背后的 DNS 信息并发现不连续的 IP 地址。它是一个多线程的 perl 语言脚本。

> dnsenum 在默认情况下会综合查询给定域名后面的 A 记录、NS 记录、MX 记录、bind 版本信息以及尝试做区域传输，最后使用默认的字典做 DNS 信息的爆破。当查询到 C 类地址的时候还会做反向的域名查询，以发现更多的域名。

> 默认的字典文件所在位置：/usr/share/dnsenum/dns.txt

> 可以用它进行如下操作：

> 获取主机记录（A 记录）
获取域名服务器
获取 MX 记录
在域名服务器上执行 axfr 查询并获取绑定版本
通过 Google 抓取获取额外的域名和子域（Google query=“allinurl:-www site:domain”）
通过字典文件进行暴力破解子域，也可以对具有 NS 记录的子域执行递归查询
计算 C 类网络地址范围并对其进行 whois 查询
在 netrange（C 类或者 whois netrange）上执行反向查找
将 ip-blocks 写入 domain_ips.txt 文件


### Linux  dnsenum命令安装

> 一般的 Linux 上可以安装 dnsenum 命令，安装比较复杂，使用Linux kali 系统，命令已经被集成到系统上了。建议直接使用Linux kali系统，集成了相关命令，不需要在去安装相关命令。


### Linux dnsenum 命令参数说明


```shell
dnsenum --help
dnsenum VERSION:1.2.6
Usage: dnsenum [Options] <domain>
[Options]:
Note: If no -f tag supplied will default to /usr/share/dnsenum/dns.txt or
the dns.txt file in the same directory as dnsenum
GENERAL OPTIONS:
  --dnsserver 	<server>
			Use this DNS server for A, NS and MX queries.
  --enum		Shortcut option equivalent to --threads 5 -s 15 -w.
  -h, --help		Print this help message.
  --noreverse		Skip the reverse lookup operations.
  --nocolor		Disable ANSIColor output.
  --private		Show and save private ips at the end of the file domain_ips.txt.
  --subfile <file>	Write all valid subdomains to this file.
  -t, --timeout <value>	The tcp and udp timeout values in seconds (default: 10s).
  --threads <value>	The number of threads that will perform different queries.
  -v, --verbose		Be verbose: show all the progress and all the error messages.
GOOGLE SCRAPING OPTIONS:
  -p, --pages <value>	The number of google search pages to process when scraping names,
			the default is 5 pages, the -s switch must be specified.
  -s, --scrap <value>	The maximum number of subdomains that will be scraped from Google (default 15).
BRUTE FORCE OPTIONS:
  -f, --file <file>	Read subdomains from this file to perform brute force. (Takes priority over default dns.txt)
  -u, --update	<a|g|r|z>
			Update the file specified with the -f switch with valid subdomains.
	a (all)		Update using all results.
	g		Update using only google scraping results.
	r		Update using only reverse lookup results.
	z		Update using only zonetransfer results.
  -r, --recursion	Recursion on subdomains, brute force all discovered subdomains that have an NS record.
WHOIS NETRANGE OPTIONS:
  -d, --delay <value>	The maximum value of seconds to wait between whois queries, the value is defined randomly, default: 3s.
  -w, --whois		Perform the whois queries on c class network ranges.
			 **Warning**: this can generate very large netranges and it will take lot of time to perform reverse lookups.
REVERSE LOOKUP OPTIONS:
  -e, --exclude	<regexp>
			Exclude PTR records that match the regexp expression from reverse lookup results, useful on invalid hostnames.
OUTPUT OPTIONS:
  -o --output <file>	Output in XML format. Can be imported in MagicTree (www.gremwell.com)
```



## Linux dnsenum 运用实例



### 搜索域名



    dnsenum -f /usr/share/dnsenum/dns.txt baidu.com
    dnsenum VERSION:1.2.6



### 查询匹配子域名



    dnsenum -r -f /usr/share/dnsenum/dns.txt jd.com
    dnsenum VERSION:1.2.6


    



