---
layout: post
title: Linux fierce 命令
key: 20230307
tags: 网络安全
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# Linux  fierce 命令



## Linux fierce 命令



### fierce  介绍
> Fierce是一款IP，域名互查的DNS工具，可进行域传送漏洞检测、字典爆破子域名、反查IP段、反查指定域名上下一段IP，属于一款半轻量级的多线程信息收集用具。Fierce可尝试建立HTTP连接以确定子域名是否存在，此功能为非轻量级功能，所以，定义为半轻量级。
>
> 在一个安全的环境中，暴力破解DNS的方式是一种获取不连续IP地址空间主机的有效手段。fierce工具可以满足这样的需求，而且已经预装在Kali Linux中。fierce是RSnake创立的快速有效地DNS暴力破解工具。fierce工具首先查询域名的IP地址，查询相关的域名服务器。


### fierce 命令参数说明


```shell
fierce --help
usage: fierce [-h] [--domain DOMAIN] [--connect] [--wide] [--traverse TRAVERSE] [--search SEARCH [SEARCH ...]] [--range RANGE] [--delay DELAY] [--subdomains SUBDOMAINS [SUBDOMAINS ...] | --subdomain-file SUBDOMAIN_FILE]
              [--dns-servers DNS_SERVERS [DNS_SERVERS ...] | --dns-file DNS_FILE] [--tcp]

        A DNS reconnaissance tool for locating non-contiguous IP space.
        

options:
  -h, --help            show this help message and exit
  --domain DOMAIN       domain name to test
  --connect             attempt HTTP connection to non-RFC 1918 hosts
  --wide                scan entire class c of discovered records
  --traverse TRAVERSE   scan IPs near discovered records, this won't enter adjacent class c's
  --search SEARCH [SEARCH ...]
                        filter on these domains when expanding lookup
  --range RANGE         scan an internal IP range, use cidr notation
  --delay DELAY         time to wait between lookups
  --subdomains SUBDOMAINS [SUBDOMAINS ...]
                        use these subdomains
  --subdomain-file SUBDOMAIN_FILE
                        use subdomains specified in this file (one per line)
  --dns-servers DNS_SERVERS [DNS_SERVERS ...]
                        use these dns servers for reverse lookups
  --dns-file DNS_FILE   use dns servers specified in this file for reverse lookups (one per line)
  --tcp                 use TCP instead of UDP
```



## fierce 运用实例



### fierce 测试域名



    fierce --domain abc.com
    NS: ns-1368.awsdns-43.org. ns-1869.awsdns-41.co.uk. ns-318.awsdns-39.com. ns-736.awsdns-28.net.
    SOA: ns-318.awsdns-39.com. (205.251.193.62)
    Zone: failure
    Wildcard: failure
    Found: a.abc.com. (68.142.107.103)
    Nearby:
    {'68.142.107.100': 'https-68-142-107-100.lax.llnw.net.',
     '68.142.107.101': 'https-68-142-107-101.lax.llnw.net.',
     '68.142.107.102': 'https-68-142-107-102.lax.llnw.net.',
     '68.142.107.103': 'https-68-142-107-103.lax.llnw.net.',
     '68.142.107.104': 'https-68-142-107-104.lax.llnw.net.',
     '68.142.107.105': 'https-68-142-107-105.lax.llnw.net.',
     '68.142.107.106': 'https-68-142-107-106.lax.llnw.net.',
     '68.142.107.107': 'https-68-142-107-107.lax.llnw.net.',
     '68.142.107.108': 'https-68-142-107-108.lax.llnw.net.',
     '68.142.107.98': 'https-68-142-107-98.lax.llnw.net.',
     '68.142.107.99': 'https-68-142-107-99.lax.llnw.net.'}
    Found: beta.abc.com. (54.71.66.109)
    Nearby:
    {'54.71.66.104': 'ec2-54-71-66-104.us-west-2.compute.amazonaws.com.',
     '54.71.66.105': 'ec2-54-71-66-105.us-west-2.compute.amazonaws.com.',
     '54.71.66.106': 'ec2-54-71-66-106.us-west-2.compute.amazonaws.com.',
     '54.71.66.107': 'ec2-54-71-66-107.us-west-2.compute.amazonaws.com.',
     '54.71.66.108': 'ec2-54-71-66-108.us-west-2.compute.amazonaws.com.',
     '54.71.66.109': 'ec2-54-71-66-109.us-west-2.compute.amazonaws.com.',
     '54.71.66.110': 'ec2-54-71-66-110.us-west-2.compute.amazonaws.com.',
     '54.71.66.111': 'ec2-54-71-66-111.us-west-2.compute.amazonaws.com.',
     '54.71.66.112': 'ec2-54-71-66-112.us-west-2.compute.amazonaws.com.',
     '54.71.66.113': 'ec2-54-71-66-113.us-west-2.compute.amazonaws.com.',
     '54.71.66.114': 'ec2-54-71-66-114.us-west-2.compute.amazonaws.com.'}
    Found: blackberry.abc.com. (192.195.67.80)
    Found: blogs.abc.com. (104.18.137.190)
    Found: connect.abc.com. (107.23.67.121)
    Nearby:
    {'107.23.67.116': 'ec2-107-23-67-116.compute-1.amazonaws.com.',
     '107.23.67.117': 'ec2-107-23-67-117.compute-1.amazonaws.com.',
     '107.23.67.118': 'ec2-107-23-67-118.compute-1.amazonaws.com.',
     '107.23.67.119': 'ec2-107-23-67-119.compute-1.amazonaws.com.',
     '107.23.67.120': 'ec2-107-23-67-120.compute-1.amazonaws.com.',
     '107.23.67.121': 'lb-d.us1.gigya.com.',
     '107.23.67.122': 'ec2-107-23-67-122.compute-1.amazonaws.com.',
     '107.23.67.123': 'ec2-107-23-67-123.compute-1.amazonaws.com.',
     '107.23.67.124': 'ec2-107-23-67-124.compute-1.amazonaws.com.',
     '107.23.67.125': 'ec2-107-23-67-125.compute-1.amazonaws.com.',
     '107.23.67.126': 'ec2-107-23-67-126.compute-1.amazonaws.com.'}
    Found: design.abc.com. (10.102.125.155)
    Found: dev.abc.com. (192.195.67.81)
    Found: developer.abc.com. (3.226.101.136)
    Nearby:
    {'3.226.101.131': 'ec2-3-226-101-131.compute-1.amazonaws.com.',
     '3.226.101.132': 'ec2-3-226-101-132.compute-1.amazonaws.com.',
     '3.226.101.133': 'ec2-3-226-101-133.compute-1.amazonaws.com.',
     '3.226.101.134': 'ec2-3-226-101-134.compute-1.amazonaws.com.',
     '3.226.101.135': 'ec2-3-226-101-135.compute-1.amazonaws.com.',
     '3.226.101.136': 'ec2-3-226-101-136.compute-1.amazonaws.com.',
     '3.226.101.137': 'ec2-3-226-101-137.compute-1.amazonaws.com.',
     '3.226.101.138': 'ec2-3-226-101-138.compute-1.amazonaws.com.',
     '3.226.101.139': 'ec2-3-226-101-139.compute-1.amazonaws.com.',
     '3.226.101.140': 'ec2-3-226-101-140.compute-1.amazonaws.com.',
     '3.226.101.141': 'ec2-3-226-101-141.compute-1.amazonaws.com.'}
    Found: download.abc.com. (192.195.67.81)
    Found: ftp2.abc.com. (153.6.180.32)
    Found: ll.abc.com. (208.111.152.0)
    Nearby:
    {'208.111.152.0': 'https-208-111-152-0.oak.llnw.net.',
     '208.111.152.1': 'https-208-111-152-1.oak.llnw.net.',
     '208.111.152.2': 'https-208-111-152-2.oak.llnw.net.',
     '208.111.152.3': 'https-208-111-152-3.oak.llnw.net.',
     '208.111.152.4': 'https-208-111-152-4.oak.llnw.net.',
     '208.111.152.5': 'https-208-111-152-5.oak.llnw.net.'}
    Found: m.abc.com. (35.82.223.12)
    Nearby:
    {'35.82.223.10': 'ec2-35-82-223-10.us-west-2.compute.amazonaws.com.',
     '35.82.223.11': 'ec2-35-82-223-11.us-west-2.compute.amazonaws.com.',
     '35.82.223.12': 'ec2-35-82-223-12.us-west-2.compute.amazonaws.com.',
     '35.82.223.13': 'ec2-35-82-223-13.us-west-2.compute.amazonaws.com.',
     '35.82.223.14': 'ec2-35-82-223-14.us-west-2.compute.amazonaws.com.',
     '35.82.223.15': 'ec2-35-82-223-15.us-west-2.compute.amazonaws.com.',
     '35.82.223.16': 'ec2-35-82-223-16.us-west-2.compute.amazonaws.com.',
     '35.82.223.17': 'ec2-35-82-223-17.us-west-2.compute.amazonaws.com.',
     '35.82.223.7': 'ec2-35-82-223-7.us-west-2.compute.amazonaws.com.',
     '35.82.223.8': 'ec2-35-82-223-8.us-west-2.compute.amazonaws.com.',
     '35.82.223.9': 'ec2-35-82-223-9.us-west-2.compute.amazonaws.com.'}
    Found: mc.abc.com. (192.195.67.33)
    Nearby:
    {'192.195.67.35': 'mx.x100net.com.'}
    Found: media.abc.com. (68.71.212.241)
    Nearby:
    {'68.71.212.236': 'secure.abc.go.com.',
     '68.71.212.237': 'contentratings.abc.go.com.',
     '68.71.212.238': 'static.abc.go.com.',
     '68.71.212.240': 'media.abcfamily.go.com.',
     '68.71.212.241': 'media.abc.go.com.',
     '68.71.212.243': 'nbtvoteaa.disney.go.com.'}
    Found: partners.abc.com. (184.25.56.66)
    Nearby:
    {'184.25.56.61': 'a184-25-56-61.deploy.static.akamaitechnologies.com.',
     '184.25.56.62': 'a184-25-56-62.deploy.static.akamaitechnologies.com.',
     '184.25.56.63': 'a184-25-56-63.deploy.static.akamaitechnologies.com.',
     '184.25.56.64': 'a184-25-56-64.deploy.static.akamaitechnologies.com.',
     '184.25.56.65': 'a184-25-56-65.deploy.static.akamaitechnologies.com.',
     '184.25.56.66': 'a184-25-56-66.deploy.static.akamaitechnologies.com.',
     '184.25.56.67': 'a184-25-56-67.deploy.static.akamaitechnologies.com.',
     '184.25.56.68': 'a184-25-56-68.deploy.static.akamaitechnologies.com.',
     '184.25.56.69': 'a184-25-56-69.deploy.static.akamaitechnologies.com.',
     '184.25.56.70': 'a184-25-56-70.deploy.static.akamaitechnologies.com.',
     '184.25.56.71': 'a184-25-56-71.deploy.static.akamaitechnologies.com.'}
    Found: r.abc.com. (35.82.223.12)



### fierce  --connect



    fierce --connect --domain metafy.gg     
    NS: amy.ns.cloudflare.com. art.ns.cloudflare.com.
    SOA: amy.ns.cloudflare.com. (172.64.32.101)
    Zone: failure
    Wildcard: failure
    Found: affiliate.metafy.gg. (151.101.194.133)
    HTTP connected:
    [('Connection', 'keep-alive'),
     ('Content-Length', '258'),
     ('Server', 'Varnish'),
     ('Retry-After', '0'),
     ('content-type', 'text/html'),
     ('Cache-Control', 'private, no-cache'),
     ('X-Served-By', 'cache-nrt-rjtf7700044-NRT'),
     ('Accept-Ranges', 'bytes'),
     ('Date', 'Tue, 07 Mar 2023 08:24:41 GMT'),
     ('Via', '1.1 varnish')]
    Found: c.metafy.gg. (172.66.43.166)
    HTTP connected:
    [('Date', 'Tue, 07 Mar 2023 08:25:25 GMT'),
     ('Content-Type', 'text/plain; charset=UTF-8'),
     ('Content-Length', '16'),
     ('Connection', 'close'),
     ('X-Frame-Options', 'SAMEORIGIN'),
     ('Referrer-Policy', 'same-origin'),
     ('Cache-Control',
      'private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, '
      'pre-check=0'),
     ('Expires', 'Thu, 01 Jan 1970 00:00:01 GMT'),
     ('Vary', 'Accept-Encoding'),
     ('Server', 'cloudflare'),
     ('CF-RAY', '7a41729d0ceed049-SJC')]
    Found: class.metafy.gg. (172.66.40.90)
    HTTP connected:
    [('Date', 'Tue, 07 Mar 2023 08:25:36 GMT'),
     ('Content-Type', 'text/plain; charset=UTF-8'),
     ('Content-Length', '16'),
     ('Connection', 'close'),
     ('X-Frame-Options', 'SAMEORIGIN'),
     ('Referrer-Policy', 'same-origin'),
     ('Cache-Control',
      'private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, '
      'pre-check=0'),
     ('Expires', 'Thu, 01 Jan 1970 00:00:01 GMT'),
     ('Vary', 'Accept-Encoding'),
     ('Server', 'cloudflare'),
     ('CF-RAY', '7a4172e58a32cf11-SJC')]
    Found: classes.metafy.gg. (172.66.43.166)
    HTTP connected:
    [('Date', 'Tue, 07 Mar 2023 08:25:38 GMT'),
     ('Content-Type', 'text/plain; charset=UTF-8'),
     ('Content-Length', '16'),
     ('Connection', 'close'),
     ('X-Frame-Options', 'SAMEORIGIN'),
     ('Referrer-Policy', 'same-origin'),
     ('Cache-Control',
      'private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, '
      'pre-check=0'),
     ('Expires', 'Thu, 01 Jan 1970 00:00:01 GMT'),
     ('Vary', 'Accept-Encoding'),
     ('Server', 'cloudflare'),
     ('CF-RAY', '7a4172ed1dc597f3-SJC')]
    Found: email.metafy.gg. (172.66.40.90)
    HTTP connected:
    [('Date', 'Tue, 07 Mar 2023 08:26:52 GMT'),
     ('Content-Type', 'text/plain; charset=UTF-8'),
     ('Content-Length', '16'),
     ('Connection', 'close'),
     ('X-Frame-Options', 'SAMEORIGIN'),
     ('Referrer-Policy', 'same-origin'),
     ('Cache-Control',
      'private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, '
      'pre-check=0'),
     ('Expires', 'Thu, 01 Jan 1970 00:00:01 GMT'),
     ('Vary', 'Accept-Encoding'),
     ('Server', 'cloudflare'),
     ('CF-RAY', '7a4174bc781097bb-SJC')]
    Found: feedback.metafy.gg. (44.214.112.154)
    HTTP connected:
    [('date', 'Tue, 07 Mar 2023 08:27:04 GMT'),
     ('content-type', 'text/html'),
     ('content-length', '20069'),
     ('connection', 'close'),
     ('set-cookie',
      '__canny__experimentID=a5d7e354-4381-45df-6929-4334a95f398b; path=/; '
      'expires=Fri, 04 Mar 2033 08:27:04 GMT; domain=44.214.112.154; '
      'samesite=none; secure'),
     ('strict-transport-security', 'max-age=31536000; includeSubDomains'),
     ('x-content-type-options', 'nosniff'),
     ('x-permitted-cross-domain-policies', 'none'),
     ('x-frame-options', 'sameorigin'),
     ('content-security-policy',
      "default-src 'self' https://canny.io https://*.canny.io; child-src 'self' "
      'blob: https://canny.io https://*.canny.io *.wistia.net https://*.loom.com '
      'https://*.stripe.com https://*.useloom.com https://*.vimeo.com '
      'https://*.youtu.be https://*.youtube.com https://intercom-sheets.com '
      'https://loom.com https://recaptcha.recaptcha.net/recaptcha/ '
      'https://share.intercom.io https://useloom.com https://vimeo.com '
      'https://www.facebook.com https://www.recaptcha.net/recaptcha/ '
      'https://www.intercom-reporting.com https://youtu.be https://youtube.com; '
      "connect-src 'self' https://canny.io https://*.canny.io *.wistia.com "
      '*.wistia.net https://api.zapier.com https://*.analytics.google.com '
      'https://*.clarity.ms https://*.g.doubleclick.net https://*.google.com '
      'https://*.google-analytics.com https://*.googletagmanager.com '
      'https://*.hubspot.com https://*.intercom.io https://*.litix.io '
      'https://*.stripe.com https://bat.bing.com https://cdn.linkedin.oribi.io '
      'https://embedwistia-a.akamaihd.net https://heapanalytics.com '
      'https://pubsub.googleapis.com https://edge.fullstory.com '
      'https://rs.fullstory.com https://api.luckyorange.com '
      'https://api-preview.luckyorange.com https://settings.luckyorange.com '
      'https://api-js.mixpanel.com https://sentry.io '
      'https://stats.g.doubleclick.net https://uploads.intercomcdn.com '
      'https://uploads.intercomusercontent.com https://www.facebook.com '
      'https://www2.profitwell.com wss://*.intercom.io '
      'wss://realtime.luckyorange.com wss://*.visitors.live; font-src * data:; '
      'form-action https://canny.io https://*.canny.io https://api-iam.intercom.io '
      'https://intercom.help https://www.facebook.com; img-src * data: '
      "https://ct.capterra.com; media-src * blob: data:; object-src 'none'; "
      "script-src 'self' 'unsafe-inline' 'unsafe-eval' https://canny.io "
      'https://*.canny.io *.wistia.com cdn.heapanalytics.com '
      'https://*.atl-paas.net https://*.clarity.ms https://*.googletagmanager.com '
      'https://*.hubspot.com https://*.intercom.io https://*.stripe.com '
      'https://*.zdassets.com https://*.zendesk.com https://a.quora.com '
      'https://bat.bing.com https://cdnjs.cloudflare.com https://cdn.zapier.com '
      'https://connect.facebook.net https://edge.fullstory.com '
      'https://tools.luckyorange.com https://g.microsoft.com '
      'https://js.hs-analytics.net https://js.hs-banner.com '
      'https://js.hs-scripts.com https://js.hscollectedforms.net '
      'https://js.intercomcdn.com https://heapanalytics.com '
      'https://public.profitwell.com https://snap.licdn.com '
      'https://www.recaptcha.net/recaptcha/ https://www.google-analytics.com '
      'https://www.googletagmanager.com https://www.gstatic.com/recaptcha/ '
      "https://www.gstatic.cn/recaptcha/; style-src 'self' 'unsafe-inline' "
      'https://canny.io https://*.canny.io https://*.atlassian.com '
      'https://*.zdassets.com https://*.zendesk.com https://cdnjs.cloudflare.com '
      'https://cdn.zapier.com https://heapanalytics.com; worker-src blob:; '
      'report-uri https://canny.io/api/csp/report'),
     ('referrer-policy', 'strict-origin')]
    Nearby:
    {'44.214.112.149': 'ec2-44-214-112-149.compute-1.amazonaws.com.',
     '44.214.112.150': 'ec2-44-214-112-150.compute-1.amazonaws.com.',
     '44.214.112.151': 'ec2-44-214-112-151.compute-1.amazonaws.com.',
     '44.214.112.152': 'ec2-44-214-112-152.compute-1.amazonaws.com.',
     '44.214.112.153': 'ec2-44-214-112-153.compute-1.amazonaws.com.',
     '44.214.112.154': 'ec2-44-214-112-154.compute-1.amazonaws.com.',
     '44.214.112.155': 'ec2-44-214-112-155.compute-1.amazonaws.com.',
     '44.214.112.156': 'ec2-44-214-112-156.compute-1.amazonaws.com.',
     '44.214.112.157': 'ec2-44-214-112-157.compute-1.amazonaws.com.',
     '44.214.112.158': 'ec2-44-214-112-158.compute-1.amazonaws.com.',
     '44.214.112.159': 'ec2-44-214-112-159.compute-1.amazonaws.com.'}
    Found: help.metafy.gg. (172.66.40.90)
    HTTP connected:
    [('Date', 'Tue, 07 Mar 2023 08:27:32 GMT'),
     ('Content-Type', 'text/plain; charset=UTF-8'),
     ('Content-Length', '16'),
     ('Connection', 'close'),
     ('X-Frame-Options', 'SAMEORIGIN'),
     ('Referrer-Policy', 'same-origin'),
     ('Cache-Control',
      'private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, '
      'pre-check=0'),
     ('Expires', 'Thu, 01 Jan 1970 00:00:01 GMT'),
     ('Vary', 'Accept-Encoding'),
     ('Server', 'cloudflare'),
     ('CF-RAY', '7a4175ba9d21ce48-SJC')]
    Found: internal.metafy.gg. (172.66.40.90)
    HTTP connected:
    [('Date', 'Tue, 07 Mar 2023 08:27:52 GMT'),
     ('Content-Type', 'text/plain; charset=UTF-8'),
     ('Content-Length', '16'),
     ('Connection', 'close'),
     ('X-Frame-Options', 'SAMEORIGIN'),
     ('Referrer-Policy', 'same-origin'),
     ('Cache-Control',
      'private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, '
      'pre-check=0'),
     ('Expires', 'Thu, 01 Jan 1970 00:00:01 GMT'),
     ('Vary', 'Accept-Encoding'),
     ('Server', 'cloudflare'),
     ('CF-RAY', '7a4176326d6a97eb-SJC')]


​    



