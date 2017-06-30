---
title: http-proxy模块内存泄露？(nodejs)
tags: |-

  - code
permalink: http-proxy-memory-leak
id: 35
updated: '2016-08-02 06:08:28'
date: 2016-05-30 08:20:30
---

此前服务器代码大量使用http-proxy模块，一方面是此模块确实简单易用，一方面是想让nodejs干所有的活，以证明它是服务端的全能工具。使用http-proxy模块的主要分为两种作用：

1、转发其他端口到80端口。
2、转发google网站实现不翻墙使用谷歌搜索。

##内存泄露？

后来无意中使用top命令查看了一下内存占用（shift+M按内存排序）。我看到了这个：
![](/content/images/2016/05/1.jpg)

512M内存的DO VPS。此时nodejs进程只使用了http-proxy模块用于转发谷歌。占用内存约300M！

贴上部分服务端代码
```js line-numbers
var httpProxy = require('http-proxy');
var proxy = httpProxy.createProxyServer();

function server(req, res) {
    req.headers.host = 'www.google.com.hk';
    proxy.web(req, res, {
        target: 'https://www.google.com.hk'
    });

    process.on('uncaughtException', function(e) {
        // error handle
    });
}
```

nodejs版本 v0.10.25 （v5.10.1情况类似）。
http-proxy版本：1.13.2

##原因

原因待查，日后补充。

##解决方法

由于nodejs读取静态文件也有内存占用大的现象，于是干脆改变一下观念，把转发流量和静态文件管理交给其他工具来完成。nginx是这方面的利器。而且使用nginx配合nodejs可以很方便地实现负载均衡和多线程。

###nginx安装
从[官网](http://nginx.org/en/download.html)下载源码，然后编译安装。

注意如果需要使用https或者转发来自https的流量，需要加参数编译：
```
$ ./configure --with-http_ssl_module
```

###nginx配置
实现转发谷歌部分的配置如下：（完整配置请参照文末链接）
```bash line-numbers
server {
    listen       80;
    server_name  mygoogle.com;

    location / {
        proxy_redirect off;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  Host  "www.google.com.hk";
        proxy_set_header   X-NginX-Proxy    true;
        proxy_set_header   Connection "";
        proxy_http_version 1.1;
        proxy_pass https://www.google.com.hk;
    }
}
```


###效果
使用nginx一段时间后看到占用内存极少。
![](/content/images/2016/05/2.jpg)

另外，处理静态文件的表现也极佳。

##总结
nodejs虽然能干很多方面的事情，但是把一些其他工具擅长的事情交给其他工具去做效果会更好。

##参考
###nginx配置
https://segmentfault.com/a/1190000002797601