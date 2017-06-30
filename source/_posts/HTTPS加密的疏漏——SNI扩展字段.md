---
title: HTTPS加密的疏漏——SNI扩展字段
tags: |-

  - Internet
permalink: https-sni
id: 68
updated: '2016-12-20 09:14:23'
date: 2016-12-20 08:41:10
---

## SNI是什么？
<a class="external" href="https://zh.wikipedia.org/wiki/%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%90%8D%E7%A7%B0%E6%8C%87%E7%A4%BA" >维基百科（服务器名称指示）</a>

**服务器名称指示**（Server Name Indication 简称 SNI）是一个扩展的TLS计算机联网协议，这允许在握手过程开始时通过客户端告诉它正在连接的服务器的主机名称。

**作用**：允许在相同的IP地址和TCP端口号的服务器上使用多个证书，而不必所有网站都使用同一个证书。在概念上等同于HTTP/1.1基于域名的虚拟主机，只不过这是在HTTPS上实现的。

## SNI信息的传输是明文的？
SNI信息是在握手的过程中，确切说是在客户端发送给服务端的第一个握手包中传递的信息。  
这时候SSL连接还未建立起来，因此SNI信息是明文传输的。

如下图所示，SNI信息即在第一个SSL握手包中（Client Hello）
![](/content/images/2016/12/1.PNG)

具体的SNI信息如下，显然是明文的：
![](/content/images/2016/12/2.PNG)

## 明文的SNI会带来什么后果？
SNI是明文的，而且是不可能被加密的，因为秘钥还没协商好呢。  
实际会带来的后果并不涉及安全问题，仅仅只是所请求的服务器泄露出去了，连接可能受到干扰。  
如果网络防火墙的包过滤系统检查SNI信息的话，就可以屏蔽某些域名的https访问，就像屏蔽http一样简单。