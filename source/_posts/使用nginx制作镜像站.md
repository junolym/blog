---
title: 使用nginx制作镜像站
tags: |-

  - Internet
permalink: make-google-mirror2
id: 60
updated: '2016-12-18 06:24:59'
date: 2016-09-12 09:59:09
---

## 前言
利用反向代理工具nginx可以为一些需要的网站制作镜像站，以加快其访问速度。本篇以谷歌为例，意在交流其中的互联网知识，不提供面向大众的服务。

## 基础
镜像站在数据传输层面和CDN相似，都是基于反向代理，简单说客户浏览器访问镜像站，镜像站将request转发到源站，再将源站的response发送给客户。其中，request的数据包需要根据源站的要求作出改变，至少需要将host替换为源站的host。Useragent也同样需要传给源站，以便其提供适合用户客户端的网页。

```js
location / {
    proxy_set_header  Host  "www.google.com.hk";
    proxy_set_header  User-Agent $http_user_agent;
    proxy_set_header  Connection "";
    proxy_http_version 1.1;
    proxy_pass https://www.google.com.hk;
}
```
完成以上的基础配置，就得到了一个可以访问的镜像站。

## 完善（负载均衡）【已失效】
16年12月份起，负载均衡似乎毫无作用。
~~
如果你的镜像站访问频率较高，这时谷歌会返回一个可能带有验证码的403页面。利用负载均衡可以有效的减少这种情况发生的频率。首先，你需要找到多个谷歌的ip地址。使用nslookup可以查找到几个ip地址，如果还不够，可以试着在同个ip段探测。使用nmap、wget即可批量探测。推荐一个扫描google ip的神器：[gscan](https://github.com/yinqiwen/gscan)。然后将得到的ip地址作为upstream。详细教程请搜索“nginx负载均衡”。注意：如果一开始就出现403页面，应该考虑更换你的服务器的ip地址。
~~

```js
upstream google {
    server www.google.com.hk:443 backup;
    server 74.125.130.17:443;
    server 74.125.130.18:443;
    server 74.125.130.19:443;
}
location / {
    proxy_set_header  Host  "www.google.com.hk";
    proxy_set_header  User-Agent $http_user_agent;
    proxy_set_header  Connection "";
    proxy_http_version 1.1;
    proxy_pass https://google;
}
```

## 进阶（CDN和HTTPS）
使用https为搜索加密是极为重要的，否则你的域名很难长期使用，甚至可能因为搜索了某个关键词而被屏蔽。使用cdn则能够为你的镜像站做ip保护和加速。目前免费的cdn不少，[cloudflare](https://www.cloudflare.com/)虽然速度不是特别快，但是综合体验不错。并且其“flexible”模式的https不要求你拥有ssl证书，只要域名规范即可开启。成功开启https后，建议强制使用https。做法可以在nginx里面写个rewrite，也可以直接在cloudflare的Page Rules添加一条规则。将`http://example.com/*` 转发（建议301）到`https://example.com/$1`。

## 高级
谷歌镜像站已经做好了，但是搜索出来的例如维基百科却仍然无法打开。稍微修改一下返回的页面，就能将维基百科跳转到自己的维基镜像。这里推荐使用网页内容替换的方法，用到了nginx第三方模块`ngx_http_substitutions_filter_module`。安装和使用方法请看<a class="external" href="https://github.com/yaoweibin/ngx_http_substitutions_filter_module">Github</a>。这里重点提一下，替换的内容必须是没有压缩的，而不压缩会消耗更多流量。两全的方法是将压缩的内容利用nginx的gunzip模块解压。参考：https://www.v2ex.com/t/234923 。

## 流量限制的临时解决办法——Cookie
前面说到，为了避免出现谷歌的流量限制，应该寻找多个（我用了一千多个）谷歌ip作为upstream。但是如果还是超过了限制，下面有一个小技巧可以在出现限制的时候继续访问。出现限制的时候应该是跳转到一个`https://ipv4.google.com/sorry/index?continue=`开头的网址。首先，使用**和代理服务器相同ip**的vpn打开这个网址，出现验证码，输入验证码，成功弹出搜索。然后记录下此时的cookie（打开浏览器控制台，输入document.cookie回车）。最后，把这个cookie配置到nginx反代的header里，类似如下：
```js
proxy_set_header  Cookie  "GOOGLE_ABUSE_EXEMPTION=ID=<...>";
```
最后，别忘了在Cookie失效后更新Cookie或去掉这句话。一次流量限制持续6小时。

## 配置文件
最后附上完整配置文件，稍微修改即可使用
```js line-numbers
worker_processes  1;
worker_rlimit_nofile 65536;
error_log  logs/error.log;
pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    charset  utf-8;
    include       mime.types;
    default_type  application/octet-stream;
    access_log  logs/access.log;
    sendfile        on;
    keepalive_timeout  65;
    gzip on;
    gzip_min_length 1k;
    gzip_buffers    4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 6;
    gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
    gzip_vary on;
    client_max_body_size   10m;
    client_body_buffer_size   256k;
    proxy_buffer_size   64k;
    proxy_buffers   4 512k;
    proxy_busy_buffers_size   1024k;
    server {
        listen       443 ssl;
        server_name  guso.ml;
        ssl_certificate /your/dir/to/pemOrCrt;
        ssl_certificate_key /your/dir/to/key;
        access_log off;
        error_log off;

        location / {
            subs_filter 'https://maps.google.com' 'http://www.google.cn';
            subs_filter 'https:\/\/maps.google.com' 'http:\/\/www.google.cn';
            subs_filter 'translate.google.com' 'translate.google.cn';
            subs_filter_types application/json;

            # remove the line below when your server is in Amarica
            set $args "${args}&gws_rd=cr";
            proxy_cookie_domain google.com guso.ml;
            proxy_redirect https://www.google.com https://guso.ml;
            proxy_redirect http://www.google.com https://guso.ml;
            proxy_set_header  Accept-Encoding "";
            proxy_set_header  Host  "www.google.com";
            proxy_set_header  User-Agent $http_user_agent;
            proxy_set_header  Connection "";
            proxy_http_version 1.1;
            proxy_pass http://unix:/var/run/google.sock;
        }
    }

    upstream google {
        server 118.98.106.212:443;
        server 118.98.106.217:443;
        server 118.98.106.231:443;
        # add more servers here
    }

    server {
        listen unix:/var/run/google.sock;
        server_name www.google.com;
        proxy_set_header Accept-Encoding gzip;
        proxy_set_header  Host  "www.google.com";
        gunzip on;

        # 使用ipv4.google.com避免走ipv6导致出错
        location / {
            proxy_pass https://ipv4.google.com;
        }
        # 只对/search做负载均衡，兼具速度和稳定
        location /search {
            proxy_pass  https://google;
        }
    }
}
```

--更新于12月7日--