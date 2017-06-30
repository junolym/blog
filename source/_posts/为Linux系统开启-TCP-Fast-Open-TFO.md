---
title: 为Linux系统开启 TCP Fast Open (TFO)
tags: |-

  - VPS
  - linux
permalink: linux-tfo
id: 71
updated: '2017-05-06 13:01:55'
date: 2017-03-11 08:35:36
---

## What
TCP快速打开（TCP Fast Open，TFO）是对TCP的一种简化握手手续的拓展，用于提高两端点间连接的打开速度。简而言之，就是在TCP的三次握手过程中传输实际有用的数据。这个扩展最初在Linux系统实现，Linux服务器，Linux系统上的Chrome浏览器，或运行在Linux上的其他支持的软件（例如shadowsocks）可以受益。（注:Shadowsocks是一款优秀的Socks代理开源软件。）

它通过握手开始时的SYN包中的TFO cookie来验证一个之前连接过的客户端。如果验证成功，它可以在三次握手最终的ACK包收到之前就开始发送数据，这样便跳过了一个绕路的行为，更在传输开始时就降低了延迟。这个加密的Cookie被存储在客户端，在一开始的连接时被设定好。然后每当客户端连接时，这个Cookie被重复返回。(参考：[维基百科](https://zh.wikipedia.org/wiki/TCP%E5%BF%AB%E9%80%9F%E6%89%93%E5%BC%80))

![](https://chenjx.cn/content/images/2017/05/devconf-2014-kernel-networking-walkthrough-16-638.jpg)

## How
为Linux系统开启TFO实际上非常简单，这里参考了[Shadowsocks的wiki上对此的说明](https://github.com/shadowsocks/shadowsocks/wiki/TCP-Fast-Open)。

## TFO要求
Linux内核版本在Linux 3.7以上

## Linux开启TFO方法
以root权限执行一下命令
```bash
# echo 3 > /proc/sys/net/ipv4/tcp_fastopen
```
并在开机自启动脚本（/etc/rc.local）添加。

在/etc/sysctl.conf中添加
```
net.ipv4.tcp_fastopen = 3
```
## Shadowsocks开启支持

在shadowsocks的config.json中设置`fast_open`为`true` 或 在启动命令后面添加：`--fast-open`。

