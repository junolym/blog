---
title: Arukas.io——免费docker（开放注册）
tags: |-

  - Internet
permalink: arukas-io
id: 67
updated: '2016-12-18 06:37:38'
date: 2016-12-05 12:32:57
---

[Arukas](https://arukas.io/)是一个正在开放公测的容器宿主商，目前完全免费，据称将来也会保留免费的plan。

免费注册，注册验证需等24小时。

教程就不细说了，网上一大堆。比如： http://51.ruyo.net/p/2058.html

可以做什么？  
1. 搭建各种容器
2. VPS搭建练手（重启会恢复默认，所以只是练手）

目前服务的限制比较多，共用ip、NAT端口随机分配，所以只能用来练技术。

秀秀自己的
![](/content/images/2016/12/----20161205201440.png)

服务器IP和端口会不定期更新怎么办？

用nodejs写个小爬虫就可以解决了，登陆拿cookie，然后抓数据再格式化一下。
```js
var superagent = require('superagent');

require('http').createServer(function(req, res) {
    superagent.post("https://app.arukas.io/api/login")
    .send({email: "yourmail@gmail.com", password: "yourArukasPassword"})
    .end((err, sres) => {
        var cookie = sres.header['set-cookie'];
        superagent.get("https://app.arukas.io/api/containers")
        .set("Cookie", cookie)
        .end((err, sres) => {
            var t = sres.text;
            var i = t.indexOf('"container_port":1234');
            var port = t.slice(i+37, i+42);
            var ip = t.slice(i+57, t.indexOf(".jp", i+57)).replace(/-/g, '.');
            res.write("ip :  " + ip + "\n");
            res.write("port: " + port + "\n");
            res.write("pass: 1024\n");
            res.end("crypto: aes-256-cfb\n");
        });
    });
}).listen(1080);
```

<div style="display:none">

以上程序只是粗略版的，可以加上base64编码的url和二维码。

然后就如下图了
![](/content/images/2016/12/----20161212024002.png)

好吧，东西在这里 --> https://sharess.arukascloud.io/

</div>
