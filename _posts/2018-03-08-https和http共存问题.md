---
layout: post
title: Nginx 配置https和http共存
key: 20180308
tags: Nginx
---

### 出现问题

由于https安全和现在大趋势(谷歌浏览器把所有非https网站标注为不安全)。为了满足大趋势(安全趋势)，网站和API接口都需要修改为https。为了平稳过度需要http和https同时存在。

配置上https SSL证书

<pre>
server {
    charset utf-8;
    client_max_body_size 10M;

    listen     80;
    listen     443;
    server_name www.maihill.com;

    ssl on;
    ssl_certificate   /usr/local/nginx/etc/http_ssl/www.maihill.com/214530152140852.pem;
    ssl_certificate_key  /usr/local/nginx/etc/http_ssl/www.maihill.com/214530152140852.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    root /data/www/maihill/public/;
    index  index.php;

    access_log /data/store/logs/www/maihill_access.log;
    error_log /data/store/logs/www/malhill_lite_api_error.log;

</pre>

访问网站

浏览器访问https://www.maihill.com正确，http://www.maihill.com报错如下
![http_error](https://blog.maihill.com/assets/images/pic/nginx/http_error.png "http_error")

### 问题解决

Nginx配置SSL证书之后，https可以正常访问，http所有网站访问都显示400错误。报错信息的意思是说http的请求被发送到https的端口上去了，所以才会出现这样的问题。

决绝办法

把ssl on；这行去掉或者注释掉，ssl写在443端口后面。这样http和https的链接都可以用，完美解决。
<pre>
charset utf-8;
    client_max_body_size 10M;

    listen     80;
    listen     443 ssl;
    server_name www.maihill.com;

    # ssl on;
    ssl_certificate   /usr/local/nginx/etc/http_ssl/www.maihill.com/214530152140852.pem;
    ssl_certificate_key  /usr/local/nginx/etc/http_ssl/www.maihill.com/214530152140852.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    root /data/www/maihill/public/;
    index  index.php;

    access_log /data/store/logs/www/maihill_access.log;
    error_log /data/store/logs/www/malhill_lite_api_error.log;
</pre>