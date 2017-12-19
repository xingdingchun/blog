---
layout: post
title: CentOS7系统初始化优化
key: 20161012
tags: Linux CentOS
---

#CentOS7系统初始化优化
##修改主机名配置主机名解析
**修改主机名**

配置**/etc/hostname**文件下写入需要命名的主机名，**hostname**命令可以查看配置的主机名，退出登录，再重新登录后可以看到修改的新主机名。
<pre>
[root@localhost ~]# vi /etc/hostname
linux-node01
[root@linux-node01 ~]# hostname
linux-node01
</pre>
**配置主机名解析**

编辑配置文件**/etc/hosts**，加入本机IP地址对应的主机名，使用**ping**命令测试是否成功。
<pre>
[root@linux-node01 ~]# vi /etc/hosts
192.168.56.11 linux-node01
[root@linux-node01 ~]# ping -c 3 linux-node01
PING linux-node01 (192.168.56.11) 56(84) bytes of data.
64 bytes from linux-node01 (192.168.56.11): icmp_seq=1 ttl=64 time=0.187 ms
64 bytes from linux-node01 (192.168.56.11): icmp_seq=2 ttl=64 time=0.098 ms
64 bytes from linux-node01 (192.168.56.11): icmp_seq=3 ttl=64 time=0.103 ms

--- linux-node01 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 0.098/0.129/0.187/0.041 ms
</pre>
##关闭防火墙

测试方便使用，实际工作中如果需要使用防火墙，可以配置相应的规则，此步骤忽略。

**关闭防火墙，查看关闭是否成功（状态）**
<pre>
[root@linux-node01 ~]# systemctl stop firewalld
[root@linux-node01 ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Sat 2016-11-26 23:36:04 CST; 44s ago
  Process: 1228 ExecStart=/usr/sbin/firewalld --nofork --nopid $FIREWALLD_ARGS (code=exited, status=0/SUCCESS)
 Main PID: 1228 (code=exited, status=0/SUCCESS)

Nov 26 23:05:35 localhost.localdomain systemd[1]: Starting firewalld - dynami....
Nov 26 23:05:38 localhost.localdomain systemd[1]: Started firewalld - dynamic....
Nov 26 23:36:04 linux-node01 systemd[1]: Stopping firewalld - dynamic firewal....
Nov 26 23:36:04 linux-node01 systemd[1]: Stopped firewalld - dynamic firewall....
Hint: Some lines were ellipsized, use -l to show in full.
</pre>
**移除出开启启动项**
<pre>
[root@linux-node01 ~]# systemctl disable fire.service
</pre>
##配置关闭selinux
一般不会使用selinux，工作中大多也是关闭的。

**注意**修改的时候不要修改错误了，不要修改**SELINUXTYPE=targeted**,如果修改成这个，重启系统系统会起不来。

关闭的方法用多种，如：
<pre>
vi /etc/selinux/config
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are prot
ected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
</pre>
或者使用sed命令替换
<pre>
sed -i 's#SELINUX=enforcing#SELINUX=disabled#g' /etc/selinux/config
</pre>
**临时使用命令关闭selinux**
<pre>
[root@linux-node01 ~]# setenforce 0
[root@linux-node01 ~]# getenforce 
Permissive
</pre>
##创建用户，给用户sudo权限
<pre>
[root@linux-node01 ~]# useradd chadwick
[root@linux-node01 ~]# echo "123456" | passwd --stdin chadwick
Changing password for user chadwick.
passwd: all authentication tokens updated successfully.
</pre>
**给用户设置sudo权限**

有两种方法，使用vi命令编辑/etc/sudoers文件，或者使用echo命令追加
<pre>
echo "chadwick ALL=(ALL) NOPASSWD=ALL" >> /etc/sudoers
grep chadwick /etc/sudoers
visudo -c
</pre>
##内核常规优化
<pre>
cat >> /etc/sysctl.conf << EOF
net.ipv4.tcp_fin_timeout = 2
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_keepalive_time = 600
net.ipv4.ip_local_port_range = 4000    65000
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_max_tw_buckets = 36000
net.ipv4.route.gc_timeout = 100
net.ipv4.tcp_syn_retries = 1
net.ipv4.tcp_synack_retries = 1
net.core.wmem_default = 8388608
net.core.rmem_default = 8388608
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
EOF
</pre>
**确认内核优化成功**
<pre>
sysctl -p
</pre>
##时间同步
根据实际情况来定，可以做时间服务器，同步时间，可以使用定时任务同步时间，以定时任务同步时间为例：
<pre>
yum install -y ntpdate
[root@linux-node01 ~]# echo "#Time sync 2016-11-16" >>/var/spool/cron/root
[root@linux-node01 ~]# echo "*/5 * * * * /usr/sbin/ntpdate time.nist.gov >/dev/n
ull 2>/&1" >>/var/spool/cron/root 
[root@linux-node01 ~]# crontab -l
#Time sync 2016-11-16
*/5 * * * * /usr/sbin/ntpdate time.nist.gov >/dev/null 2>/&1
</pre>
##调整文件描述符长度
**备注**使用**ulimit -n**命令来查看文件描述符长度，默认是1024，修改需要reboot系统才能生效。
<pre>
echo "*               -       nofile          65535 " >>/etc/security/limits.conf
</pre>
##调整超时参数，命令历史记录，命令操作时间，操作日志记录
**调整超时时间和命令历史记录**

超时参数会设置，命令历史记录长度和大小不一定设置，根据实际情况设置
<pre>
echo 'export TMOUT=300' >> /etc/profile
ehco 'export HISTSIZE=5' >> /etc/profile
echo 'export HISTFILESIZE=5' >> /etc/profile
tail -3 /etc/profile
. /etc/proflie
</pre>
**调整操作命令记录时间**
<pre>
echo 'export HSITTIMEFORMAT="%F %T `whoamin`"' >> /etc/profile
. /etc/profile
</pre>
**记录操作日志**
<pre>
echo 'export PROMPT_COMMAND='{ msg=$(history 1 | { read x y; echo $y; });logger "[euid=$(whoami)]":$(who am i)[`pwd`]"$msg"; }''>>/etc/profile
</pre>
##调整ssh连接提高安全
主要是改变默认端口22，可以调整为自己定义的端口，禁止root用户直接登录，禁止空密码登录，必须设置密码。

两种方法，一种使用vi命令到/etc/ssh/sshd_config中直接修改
一种是使用sed命令直接追加
<pre>
sed -ir '13 iPort 50009\nPermitRootLogin no\nPermitEmptyPasswords no' /etc/ssh/sshd_config
systemctl restart sshd
</pre>
##yum源调整
官方yum源在国外，由于网速问题，一般都会调整yum源为阿里源

备份源
<pre>
\mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
</pre>
调整为阿里源
<pre>
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
</pre>
添加第三方epel源

默认是没有的，不需要备份，如果以前有epel源，需要先备份在修改
<pre>
rpm -ivh http://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm
</pre>
##安装必备的软件
<pre>
yum install lrzsz nmap tree dos2unix nc telnet vim net-tools wget -y
</pre>
##更新系统重启系统
<pre>
yum upgrade
reboot
</pre>
##CentOS无法使用tab不全功能解决办法
CentOS7在使用最小化安装的时候，没有安装自动不全的包，需要手动安装。
<pre>
yum install bash-completion -y
</pre>
或者可以安装这些基本使用的包
<pre>
yum groupinstall Base Copatibility libraries Debugging Tools Dial-up Networking suppport Hardware monitoring utilities Performance Tools Devlopment tools
</pre>
