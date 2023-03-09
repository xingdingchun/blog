---
layout: post
title: Linux nmap 命令
key: 20230309
tags: 网络安全
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# Linux  nmap 命令



## Linux nmap 命令



### nmap 介绍



> nmap(Network Mapper)网络映射器
>
> nmap是一款开源的网络探测和安全审核工具，快速扫描大型网络，是网络管理员必用的命令之一，用以评估网络系统安全。
>
> 基本功能 1.探测一组主机是否在线；2.扫描主机端口，嗅探所提供的网络；3.推断主机所用的操作系统；



### nmap



| T1   | 发送TCP数据包(Flag=SYN)到开放TCP端口               |
| ---- | -------------------------------------------------- |
| T2   | 发送一个空的TCP数据包到开放的TCP端口               |
| T3   | 发送TCP数据包(Flag=SYN,URG,PSH.FIN)到开放的TCP端口 |
| T4   | 发送TCP数据包(Flag=ACK)到开放的TCP端口             |
| T5   | 发送TCP数据包(Flag=SYN)到关闭的TCP端口             |
| T6   | 发送TCP数据包(Flag=ACK)到关闭的TCP端口             |
| T7   | 发送TCP数据包(Flag=URG,PSH,FIN)到关闭的TCP端口     |



### nmap 命令参数说明


```shell
nmap --help           
Nmap 7.93 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks 读取主机列表
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports sequentially - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection 启用操作系统检测
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect) 显示详细的扫描过程
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --noninteractive: Disable runtime interactions via keyboard
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute 全系统检测
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
```



## nmap 运用实例



### nmap 全面扫描



```shell
// 使用域名扫描
nmap -A www.epal.gg
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-09 14:59 CST
Nmap scan report for www.epal.gg (52.222.214.33)
Host is up (0.0035s latency).
Other addresses for www.epal.gg (not scanned): 52.222.214.115 52.222.214.72 52.222.214.14 2600:9000:223d:3a00:8:739c:7c00:93a1 2600:9000:223d:9c00:8:739c:7c00:93a1 2600:9000:223d:de00:8:739c:7c00:93a1 2600:9000:223d:5a00:8:739c:7c00:93a1 2600:9000:223d:2e00:8:739c:7c00:93a1 2600:9000:223d:c00:8:739c:7c00:93a1 2600:9000:223d:ec00:8:739c:7c00:93a1 2600:9000:223d:5400:8:739c:7c00:93a1
rDNS record for 52.222.214.33: server-52-222-214-33.fra56.r.cloudfront.net
Not shown: 998 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
80/tcp  open  http     Amazon CloudFront httpd
|_http-server-header: CloudFront
|_http-title: Did not follow redirect to https://www.epal.gg/
443/tcp open  ssl/http Amazon CloudFront httpd
|_http-title: E-Pal: Teammates On-Demand
| ssl-cert: Subject: commonName=*.epal.gg
| Subject Alternative Name: DNS:*.epal.gg
| Not valid before: 2023-02-24T00:00:00
|_Not valid after:  2024-02-07T23:59:59
| http-server-header: 
|   CloudFront
|_  nginx
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Actiontec MI424WR-GEN3I WAP (94%), Microsoft Windows XP SP3 or Windows 7 or Windows Server 2012 (94%), Linux 3.2 (93%), Microsoft Windows XP SP3 (93%), DD-WRT v24-sp2 (Linux 2.4.37) (92%), VMware Player virtual NAT device (92%), Linux 4.4 (91%), BlueArc Titan 2100 NAS device (88%), IBM OS/2 Warp 2.0 (86%), IBM AIX 7.1 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 80/tcp)
HOP RTT     ADDRESS
1   0.09 ms 192.168.66.2
2   0.12 ms server-52-222-214-33.fra56.r.cloudfront.net (52.222.214.33)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.12 seconds
// 使用IP地址扫描
nmap -v -A 51.89.5.185
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-09 15:22 CST
NSE: Loaded 155 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 15:22
Completed NSE at 15:22, 0.00s elapsed
Initiating NSE at 15:22
Completed NSE at 15:22, 0.00s elapsed
Initiating NSE at 15:22
Completed NSE at 15:22, 0.00s elapsed
Initiating Ping Scan at 15:22
Scanning 51.89.5.185 [4 ports]
Completed Ping Scan at 15:22, 3.02s elapsed (1 total hosts)
Nmap scan report for 51.89.5.185 [host down]
NSE: Script Post-scanning.
Initiating NSE at 15:22
Completed NSE at 15:22, 0.00s elapsed
Initiating NSE at 15:22
Completed NSE at 15:22, 0.00s elapsed
Initiating NSE at 15:22
Completed NSE at 15:22, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 3.66 seconds
           Raw packets sent: 8 (304B) | Rcvd: 0 (0B)
```



### nmap 穿透防火墙扫描



```shell
nmap -Pn -A 51.89.5.185
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-09 15:12 CST
Nmap scan report for 51.89.5.185
Host is up.
All 1000 scanned ports on 51.89.5.185 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
Too many fingerprints match this host to give specific OS details

TRACEROUTE (using proto 1/icmp)
HOP RTT    ADDRESS
1   ... 30

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 239.19 seconds
```

​    

### nmap TCP SYN Ping 扫描

```shell
nmap  -PS  -v 51.89.5.185
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-09 15:20 CST
Initiating Ping Scan at 15:20
Scanning 51.89.5.185 [1 port]
Completed Ping Scan at 15:20, 2.02s elapsed (1 total hosts)
Nmap scan report for 51.89.5.185 [host down]
Read data files from: /usr/bin/../share/nmap
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 2.05 seconds
           Raw packets sent: 2 (88B) | Rcvd: 0 (0B)
```



### nmap TCP ACK Ping 扫描



```shell
nmap  -PA  -v  39.105.111.235           
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-09 15:28 CST
Initiating Ping Scan at 15:28
Scanning 39.105.111.235 [1 port]
Completed Ping Scan at 15:28, 0.00s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 15:28
Completed Parallel DNS resolution of 1 host. at 15:28, 0.05s elapsed
Initiating SYN Stealth Scan at 15:28
Scanning 39.105.111.235 [1000 ports]
Discovered open port 22/tcp on 39.105.111.235
Discovered open port 8080/tcp on 39.105.111.235
Discovered open port 80/tcp on 39.105.111.235
Discovered open port 8888/tcp on 39.105.111.235
Increasing send delay for 39.105.111.235 from 0 to 5 due to 11 out of 15 dropped probes since last increase.
Increasing send delay for 39.105.111.235 from 5 to 10 due to 11 out of 11 dropped probes since last increase.
SYN Stealth Scan Timing: About 49.75% done; ETC: 15:29 (0:00:31 remaining)
Completed SYN Stealth Scan at 15:29, 55.74s elapsed (1000 total ports)
Nmap scan report for 39.105.111.235
Host is up (0.0048s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
8080/tcp open  http-proxy
8888/tcp open  sun-answerbook

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 55.83 seconds
           Raw packets sent: 2038 (89.504KB) | Rcvd: 266 (10.656KB)
```







