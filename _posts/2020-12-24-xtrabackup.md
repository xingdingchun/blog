---
layout: post
title: xtrabackup
key: 20201224
tags: Linux-Command
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# xtrabackup

## xtrabackup

### xtrabackup介绍

[PERCONA官方网站](https://www.percona.com "PERCONA官方网站")

>xtrabackup是由percona开源的免费数据库热备软件，它能对InnoDB数据库和XtraDB存储引擎的数据库非阻塞的备份(对于MyISAM的备份同样需要加表锁);mysqldump备份方式是采用的逻辑备份，其最大的缺陷是备份和恢复速度较慢，如果数据库大于50GB,mysqldump备份就不太适合。

>xtrabackup安装完成后有4个可执行文件，其中2个比较重要的备份工具是innobackupex,xtrabackup

1. xtrabackup是专门用来备份InnoDB表的，和MySQL server没有交互;
2. innobackupex是一个封装xtrabackup的Perl脚本，支持同时备份InnoDB和MyISAM,但在对MyISAM备份时需要叫一个全局的读锁;
3. xbcrypt加密解密备份工具;
4. xbstream流传打包传输工具，类似于tar;
5. 物理备份工具，在同级数据量基础上，都要比逻辑备份性能好的多，特别是在数据量较大的时候，提现的更加明显。

### xtarbackup有点

1. 备份速度快，物理备份可靠；
2. 备份过程不会打断正在执行的事务(无需锁表);
3. 能够基于压缩等功能节约磁盘空间和流量;
4. 自动备份校验;
5. 还原速度快;
6. 可以流传将备份传输到另外一台机器上;
7. 在不增加服务器负载的情况下备份数据;
8. 物理备份工具，在同级数据量基础上，都要比逻辑备份性能好很多，几十GB到TB级别的条件下，物理备份恢复数据有一定的优势。
 
### xtarbackup备份原理

    拷贝数据文件，拷贝数据页。

对InnoDB表可以实现热备

    1. 在数据库还在修改的时刻，直接将数据文件备份走，此时，备份走的数据相对于前MySQL来讲是不一致的；
    2. 将备份过程中的redo和undo一并备份；
    3. 为了恢复的时候，只要保证备份出来的数据页isn能和redo,isn匹配，将来恢复的就是一致的数据，redo应用和undo应用。    
对MyISAM表实现自动锁表拷贝文件
    备份开始是首先会开启一个后台检测进程，实时检测mysql redo的变化，一旦发现有新的日志写入，立刻将日志计入后台日志文件xtarbackup_log中，之后复制InnoDB的数据文件，系统表空间文件ibdatax，复制结束后，将执行flush tables with readlock然后复制.frm MYI MYD等文件，最后执行unlock tables，最终停止xtrabackup_log

### xtrabackup安装

>注官方提供不同的版本，这里以2.4版本安装为实例，测试及使用都使用的2.4版本

[PERCONA官方网安装文档](https://www.percona.com/doc/percona-xtrabackup/2.4/index.html "PERCONA官方安装文档")

>Installing Percona XtraBackup on Debian and Ubuntu

[Installing Percona XtraBackup on Debian and Ubuntu](https://www.percona.com/doc/percona-xtrabackup/2.4/installation/apt_repo.html "Installing Percona XtraBackup on Debian and Ubuntu")

>Installing Percona XtraBackup on Red Hat Enterprise Linux and CentOS

[Installing Percona XtraBackup on Red Hat Enterprise Linux and CentOS](https://www.percona.com/doc/percona-xtrabackup/2.4/installation/yum_repo.html "Installing Percona XtraBackup on Red Hat Enterprise Linux and CentOS")

### 全量备份与恢复

>备份

    创建备份目录(根据需求自定义)：mkdir -p /backup/xfull
    innobackupex --defaults-file=/etc/my.cnf --user=root --password=123 --socket=/application/mysql/tmp/mysql.sock --no-timestamp /backup/xfull

>恢复

    恢复前准备
    恢复数据前的准备(合并xtabackup_log_file和备份的物理文件)、
    innobackupex --apply-log --use-memory=1024M /backup/xfull/

    查看合并后的checkpoints，其中的类型变为full-prepared，即可以恢复。
    cat xtrabackup_checkpoints 
    backup_type = full-prepared # 注意此处的状态必须为full-prepared才可以恢复
    from_lsn = 0
    to_lsn = 12381621941
    last_lsn = 12381621950
    compact = 0
    recover_binlog_info = 0
    flushed_lsn = 12381605106

    数据恢复方法一：直接将备份文件复制回来(不推荐)
    cp -a xfull /data/mysql/
    chown mysql.mysql /data/mysql/ -R

    数据恢复方法二：使用innobackupex命令恢复(推荐使用方法)
    cd /data/mysql
    innobackupex --copy-back /root/xfull
    chown mysql.mysql /data/mysql -R

    数据恢复完成后启动数据库即可
    systemctl start mysqld

### 增量备份与恢复

innobackupex增量备份过程中的"增量"处理，其实主要是InnoDB，对MyISAM和其他存储引擎而言，它仍然是全拷贝(全备)。"增量"备份过程主要是通过拷贝InnoDB中有变更的"页"(这些变更的数据页指的是"页"的LSN大于xtrabackup_checkpoints中给定的LSN).增量备份是基于全备的，第一次增量的数据必须要基于上一次的全备，之后的每次增量都是基于上一次的增备，最终达到一致性的增备。

>增量备份

    1.先进行一次全备份
    innobackupex --defaults-file=/etc/my.cnf --user=root --password=123 --socket=/application/mysql/tmp/mysql.sock --no-timestamp /root/xfull
 
    2.载进行一次增量备份
    innobackupex --defaults-file=/etc/my.cnf --user=root --password=123  --incremental --no-timestamp --incremental-basedir=/backup/xfull/  /backup/incremental01

>增量恢复

    1.先应用全备日志(--apply-log,暂时不需要做回滚操作 --redo-only)
    innobackupex --apply-log --redo-only /backup/xfull

    2.合并增量到全备中(一致性的合并)
    innobackupex --apply-log --incremental-dir=/backup/incremental01 /backup/xfull
    innobackupex --apply-log /backup/xfull

    3.恢复
    innobackupex --copy-back /backup/xfull
    chown mysql.mysql /data/mysql -R

    
### 数据库备份策略

>按照周时间备份，每周日全备，每周一到每周六进行增量备份；数据比较小的话建议全备，恢复速度比较快，增量备份恢复时间相对长于增量备份时间；生产事故建议保障数据库完全恢复，停止数据库的写入和查询，以最快的速度恢复数据库。

### innobackupex参数说明

    --compress   表示压缩InnoDB数据库文件备份;
    --compress-threads 表示压缩Worker线程的数量;
    --compress-chunk-size 表示每个压缩线程worker buffer的大小，单位是字节，默认是64K;
    --encrypt 通过ENCRYPTION_ALGORITHM的算法加密InnoDB数据文件的备份，目前支持的算法有：AES128,AES192,AES256;
    --encrypt-threds 表示并行加密worker线程数量；
    --encrypt-chunk-size 每个加密线程worker buffer的大小，单位是字节，默认是64K；
    --encrypt-key 使用适合长度加密key，因为记录到命令行，所以不推荐使用；
    --encryption-key-file 文件必须是一个简单二进制货文本文件，加密key可通过一下命令行生成:openssl rand -base64 24;
    --include 使用正则表达式匹配表的名字[db.tb],要求为其指定匹配要备份的表的完善名称，即databasename.tablename;
    --user 备份账号(用户);
    --password 备份账号的密码;
    --port 备份数据库的端口;
    --host 备份数据库的地址;
    --database 该选项接受的参数为数据库名，如果需要指定多个数据库，彼此之间使用空格隔开，同时指定某个数据库的时候也可以指定某张表;
    --tables-file 指定含有表的文件，格式database.table,该选项直接传给--tables-file;
    --socket 该选项表示mysql.sock所在的位置,以便备份进程登录MySQL;
    --no-timestamp 该选项可以表示不要创建一个时间戳目录来存储备份，指定到自己想要的备份文件夹;
    --ibbackup 该选项指定使用哪个xtarbackup二进制程序，IBBACKUP-BINARY是运行percona xtrabackup的命令，这个选项适用于xtrabackup二进制不在你搜索和工作目录，如果指定了该选项innobackupex自动决定用的二进制程序；
