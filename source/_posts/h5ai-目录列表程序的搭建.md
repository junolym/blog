---
title: h5ai 目录列表程序的搭建
permalink: h5ai
id: 64
updated: '2016-10-28 13:26:58'
date: 2016-10-28 12:27:18
tags:
---

[h5ai](https://larsjung.de/h5ai/)是一个强大美观的目录列表程序。我第一次看到它的时候就喜欢上了，这不就是我想要实现的东西吗？
![](/content/images/2016/10/----20161028202839.png)

好了，造轮子不如搬过来直接用。

要求PHP环境，如果没有php而系统是ubuntu16.04，可以直接`apt install php`装一个。注意h5ai只是一个列出文件和文件夹的程序，获取文件需要文件服务器。这里使用nginx，其他服务器可看官方教程。

## 下载h5ai
官网地址： https://larsjung.de/h5ai/ 。点击右边的下载按钮得到压缩包，解压到你的网站根目录下。文件列表如下图：
```
DOC_ROOT
 ├─ _h5ai
 ├─ your files
 └─ and folders
```
## 配置nginx
php部分需要根据php-fpm的配置来设置，这里使用的是apt安装得到的默认配置。php版本：7.0.8 。_h5ai/private这个文件夹最好保护起来。
```js line-numbers
server {
    listen       80;
    server_name  example.com；

    location / {
        root         html;
        index        index.html /_h5ai/public/index.php;
    }
    location /_h5ai/private {
        return 403;
    }
    location ~ \.php$ {
        root           html;
        fastcgi_pass   unix:/run/php/php7.0-fpm.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```

## 其他设置
修改 _h5ai/private/conf/options.json 进一步设置h5ai。例如修改passhash，默认为空密码的hash。以及修改默认语言为中文。如下：
```
"l10n": {
    "enabled": true,
    "lang": "zh-cn",
    "useBrowserLang": true
},
```

## Demo
成果 --> https://download.chenjx.cn
