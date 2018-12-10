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

**软件下载：**

[Prometheust下载](https://github.com/prometheus/prometheus/releases "Prometheust下载")
## Prometheust安装步骤介绍

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
<pre>
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
</pre>

**启动prometheus：**
<pre>./prometheus --config.file="/usr/local/prometheus/prometheus.yml" &</pre>
**查看是否启动成功：**
<pre>
[root@docker-test-b prometheus]# netstat -lntup | grep 9090
tcp6       0      0 :::9090                 :::*                    LISTEN      79088/./prometheus 
</pre>
**移步到浏览器访问：**
<pre>http://localhost:9090</pre>













