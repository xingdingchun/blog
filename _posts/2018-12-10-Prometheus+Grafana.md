---
layout: post
title: Prometheus+Grafana
key: 20181210
tags: Linux-Command
---

# Prometheus+Grafana

随着Docker容器的热度不减，Docker容器的使用频率也随之增加。那么，问题来了，Docker容器也需要监控，我们也需要了解Docker容器的健康状况，使用情况才能根据业务的实际情况做出一定的调整。说到监控很多人都想起大名鼎鼎的zabbix。但是这次我们不讨论这个大名鼎鼎的监控软件，我们来讨论更适合容器监控的Prometheus+Grafana。

本篇文章先简单介绍一下安装，使用和简单监控，后期在做深入分析

## Prometheust(普罗米修斯)安装


## Prometheust安装步骤介绍

**软件下载：**

[Prometheust下载](https://github.com/prometheus/prometheus/releases "Prometheust下载")

**下载软件：**
<pre>wget https://github.com/prometheus/prometheus/releases/download/v2.6.0-rc.0/prometheus-2.6.0-rc.0.linux-amd64.tar.gz</pre>
**解压软件：**
<pre>tar xf prometheus-2.6.0-rc.0.linux-amd64.tar.gz</pre>
**修改软件名称：**
<pre>mv prometheus-2.6.0-rc.0.linux-amd64 prometheus-2.6.0</pre>
**软件移动到相应目录：**
<pre>mv prometheus-2.6.0 /usr/local/</pre>
**创建软件软连接：**
<pre>cd /usr/local/ && ln -s /usr/local/prometheus-2.6.0/ /usr/local/prometheus</pre>
**简单测试和查看版本：**
<pre>cd prometheus && ./prometheus --version</pre>
**备份配置文件：**
<pre>cp prometheus.yml prometheus.yml.bak</pre>
**修改配置文件：**

    # my global config
       evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
      # scrape_timeout is set to the global default (10s).
    # Alertmanager configuration
    alerting:
      alertmanagers:
      - static_configs:
        - targets:
         #  - alertmanager:9093
    # Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
    rule_files:
      # - "first_rules.yml"
      # - "second_rules.yml"
    # A scrape configuration containing exactly one endpoint to scrape:
    # Here it's Prometheus itself.
    scrape_configs:
      # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
      - job_name: 'prometheus'
        # metrics_path defaults to '/metrics'
        # scheme defaults to 'http'.
        static_configs:
        - targets:['192.168.62.243:9090','192.168.62.243:9100','192.168.62.254:9100','192.168.62.254:8080','192.168.62.243:8080','192.168.62.253:8080']
        # 主要是修改这个地方，添加需要监控的项目

**启动prometheus：**
<pre>./prometheus --config.file="/usr/local/prometheus/prometheus.yml" &</pre>
**查看是否启动成功：**
<pre>
[root@docker-test-b prometheus]# netstat -lntup | grep 9090
tcp6       0      0 :::9090                 :::*                    LISTEN      79088/./prometheus 
</pre>
**移步到浏览器访问：**
<pre>http://localhost:9090</pre>

**prometheus启动脚本**
>启动脚本放入/etc/systemd/system/目录或者/usr/lib/systemd/system/目录下(两个目录没有实质上的区别，相当于软连接的形式)。
>注意：这个地方需要创建prometheus运行的用户，并且要求数据目录和程序目录都是prometheus用户所有。
创建用户
    user prometheus -M -s /sbin/nologin
授权
    chown prometheus:prometheus /usr/local/prometheus/ -R
    chown prometheus:prometheus /data/prometheus/ -R
配置文件详细信息
    [Unit]
    Description=Prometheus
    After=network.target

    [Service]
    Type=simple
    Environment="GOMAXPROCS=4"
    User=prometheus
    Group=prometheus
    ExecReload=/bin/kill -HUP $MAINPID
    ExecStart=/usr/local/prometheus/prometheus \
      --config.file=/usr/local/prometheus/prometheus.yml \
      --storage.tsdb.path=/data/prometheus \
      --storage.tsdb.retention=30d \
      --web.console.libraries=/usr/local/prometheus/console_libraries \
      --web.console.templates=/usr/local/prometheus/consoles \
      --web.listen-address=0.0.0.0:9090
    PrivateTmp=true
    PrivateDevices=true
    ProtectHome=true
    NoNewPrivileges=true
    LimitNOFILE=infinity
    ReadWriteDirectories=/data/prometheus
    ProtectSystem=full

    SyslogIdentifier=prometheus
    Restart=always

    [Install]
    WantedBy=multi-user.target

配置文件解释

    [Unit]
    Description=Prometheus
    After=network.target

    [Service]
    Type=simple
    Environment="GOMAXPROCS=4"
    User=prometheus
    Group=prometheus
    ExecReload=/bin/kill -HUP $MAINPID
    ExecStart=/usr/local/prometheus/prometheus \  # prometheus安装的路径
      --config.file=/usr/local/prometheus/prometheus.yml \ # prometheus配置文件路径
      --storage.tsdb.path=/data/prometheus \ # prometheus数据存储路径(注意需要有prometheus用户权限)
      --storage.tsdb.retention=30d \ # 数据存储时间
      --web.console.libraries=/usr/local/prometheus/console_libraries \
      --web.console.templates=/usr/local/prometheus/consoles \
      --web.listen-address=0.0.0.0:9090  # 配置prometheus服务监听的端口
    PrivateTmp=true
    PrivateDevices=true
    ProtectHome=true
    NoNewPrivileges=true
    LimitNOFILE=infinity
    ReadWriteDirectories=/data/prometheus
    ProtectSystem=full


    SyslogIdentifier=prometheus
    Restart=always

    [Install]
    WantedBy=multi-user.target


## 系统监控模块下载安装

**软件下载：**

[node_exporter下载](https://github.com/prometheus/node_exporter/releases "node_exporter下载")

**指定版本软件下载**
<pre>
wget https://github.com/prometheus/node_exporter/releases/download/v0.17.0/node_exporter-0.17.0.linux-amd64.tar.gz
</pre>

**解压软件**
<pre>
tar xf node_exporter-0.17.0.linux-amd64.tar.gz
</pre>

**修改软件名称**
<pre>
mv node_exporter-0.17.0.linux-amd64 node_exporter-0.17.0
</pre>

**软件移动到指定目录**
<pre>
mv node_exporter-0.17.0 /usr/local/
</pre>

**做软件软连接**
<pre>
ln -s /usr/local/node_exporter-0.17.0/ /usr/local/node_exporter
</pre>

**启动node_exporter**
<pre>
./node_exporter &
</pre>

**查看是否启动成功，端口为9100**
<pre>
[root@docker-test-b node_exporter]# netstat -lntup | grep 9100
tcp6       0      0 :::9100                 :::*                    LISTEN      19097/./node_export
</pre>

## Grafana安装

**官方文档地址**
[Grafana](https://grafana.com/grafana/download "Grafana")

**Grafana安装**

安装方式有：二进制安装和RPM包安装，下面采用RPM包安装方式(CentOS)

<pre>
wget https://dl.grafana.com/oss/release/grafana-5.4.2-1.x86_64.rpm 
sudo yum localinstall grafana-5.4.2-1.x86_64.rpm 
</pre>

**Grafana配置文件修改**

    [server]
    # Protocol (http, https, socket)
    ;protocol = http

    # The ip address to bind to, empty will bind to all interfaces
    http_addr = 192.168.62.243     #配置主机地址

    # The http port  to use
    http_port = 3000    #指定访问端口

    # The public facing domain name used to access grafana from a browser
    domain = 192.168.62.243   #指定主机域

**Granfana启动**

<pre>
systemctl enable grafana   #加入开机启动项
systemctl start grafana    #启动
systemctl status grafana   #查看状态
</pre>

**移步浏览器访问**
<pre>http://localhost:3000</pre>


