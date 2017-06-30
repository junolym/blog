---
title: 几种VPN的一键安装脚本（Ubuntu VPS）
tags: |-

  - VPS
permalink: vpn-1key
id: 50
updated: '2016-08-01 16:44:13'
date: 2016-07-22 16:59:44
---

搭建各种VPN的方法网络上已经有很多教程了，本文提供的是一键安装脚本，一是为了使用方便，二是稍有基础的同学直接阅读脚本便知步骤。或许这是程序员间最好的沟通方式。

## 一、Shadowsocks
直接安装
```
wget http://chenjx.cn/content/images/other/shadowsocks.sh; sudo sh shadowsocks.sh
```
<a class="btn btn-download" href="/content/images/other/shadowsocks.sh">下载</a>

```bash line-numbers
#!/bin/sh
if [ `id -u` -ne 0 ]; then
    echo "please run it as super user"
    exit 0
fi

apt-get -y install python-pip && pip install shadowsocks || {
    echo "install shadowsocks failed"
    exit 1
}

read -p "shadowsocks port (default is 8388):" port
read -p "shadowsocks password (default is password):" password
if [ -z "$port" ]; then
    port=8388
fi
if [ -z "$password" ]; then
    password="password"
fi

cat >/etc/shadowsocks.json <<END
{
    "server":"::",
    "server_port":$port,
    "local_port":1080,
    "password":"$password",
    "timeout":600,
    "method":"aes-256-cfb"
}
END

ssserver -c /etc/shadowsocks.json -d start

cat >>/etc/rc.local <<END
ssserver -c /etc/shadowsocks.json -d start
END

echo "install success"
echo "port is $port, password is $password"
exit 0
```

## 二、PPTP
直接安装：
```
wget http://chenjx.cn/content/images/other/pptp.sh; sudo sh pptp.sh
```
<a class="btn btn-download" href="/content/images/other/pptp.sh">下载</a>

```bash line-numbers
#!/bin/sh
if [ `id -u` -ne 0 ]; then
    echo "please run it as super user"
    exit 0
fi

apt-get -y install pptpd || {
    apt-get -y update
    apt-get -y install pptpd
} || {
    echo "install pptpd failed" 
    exit 1
}

cat >/etc/ppp/options.pptpd <<END
name pptpd
refuse-pap
refuse-chap
refuse-mschap
require-mschap-v2
require-mppe-128
ms-dns 8.8.8.8
ms-dns 8.8.4.4
proxyarp
lock
nobsdcomp 
novj
novjccomp
nologfd
END

cat >/etc/pptpd.conf <<END
option /etc/ppp/options.pptpd
logwtmp
localip 192.168.2.1
remoteip 192.168.2.10-100
END

cat >> /etc/sysctl.conf <<END
net.ipv4.ip_forward=1
END

sysctl -p

iptables-save > /etc/iptables.down.rules

iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o eth0 -j MASQUERADE

iptables -I FORWARD -s 192.168.2.0/24 -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1300

iptables-save > /etc/iptables.up.rules

cat >>/etc/ppp/pptpd-options<<EOF
pre-up iptables-restore < /etc/iptables.up.rules
post-down iptables-restore < /etc/iptables.down.rules
EOF

read -p "pptp username (default is test):" username
read -p "pptp password (default is test):" password
if [ -z "$username" ]; then
    username=test
fi
if [ -z "$password" ]; then
    password=test
fi

cat >/etc/ppp/chap-secrets <<END
$username pptpd $password *
END

service pptpd restart

echo "install success"
echo "username is $username, password is $password"
exit 0
```

## 三、IKEv2
传送 --> 
<a class="external" href="https://github.com/quericy/one-key-ikev2-vpn">Github</a>
