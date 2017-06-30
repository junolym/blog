---
title: Vinzor云桌面——追求开放与自由
tags: |-

  - linux
permalink: vinzor-exploit
id: 49
updated: '2017-05-05 07:58:18'
date: 2016-07-06 08:32:08
---

Vinzor云桌面（云晫虚拟课室）采用Ubuntu12.04系统。下面演示在云桌面中传输文件，提权（SU），开放网络访问等操作。另有Linux和Mac远程控制云桌面的方法。

## 一、访问云桌面挂载的smb磁盘
通常云桌面会自动挂载一个samba分享磁盘到本地，而开机自动挂载磁盘的命令一般保存在rc.local等开机启动脚本中，因此只要能查看这个脚本，就能够知道这个samba访问的地址、用户名、密码。

如下图，rc.local中第一行就透露了这些信息。另外还有路由和防火墙的设置命令。![](/content/images/2016/07/1.jpg)

在windows访问这个网络磁盘：  
资源管理器访问`\\172.18.215.5\1433****`，注意是反斜杠，然后输入学号和密码，即上图中pass=后面的一串![](/content/images/2016/07/2.jpg)
Linux和Mac访问地址为`smb://172.18.215.5/1433****`。

## 二、提权
提权是利用Ubuntu的漏洞，由于云桌面系统内核比较老，仍存在提权漏洞。这里利用的是[37292](https://www.exploit-db.com/exploits/37292/)这个漏洞。
其中37292.c这个文件可以传进去，也可以手打，百来行C代码而已。
<a class="btn btn-download" href="https://www.exploit-db.com/download/37292">下载</a>

gcc编译运行：此时已开启一个su终端。提权成功。![](/content/images/2016/07/3.jpg)
<p class="warning">提权成功后可以先修改root密码，如无必需，不要修改默认账户的密码。如果修改后无法进入桌面，请修改`/etc/xrdp/xrdp.ini`中[xrdp1]的`password=ask`为`password=你的密码`。
</p>

## 三、开放网络访问
云桌面中网络访问的障碍一般是删除了默认路由和防火墙。当然，前提是云桌面所在的服务器是连上外网的。

以下命令可以开放防火墙
```
# iptables -P INPUT ACCEPT
# iptables -P OUTPUT ACCEPT
# iptables -P FORWARD ACCEPT
```

用route命令查看路由表，或者用ifconfig查看路由。然后设置默认路由。如果查看路由表时看到已经有默认路由，省略这一步。
```
# route add default gw 路由ip地址
```
![](/content/images/2016/07/7.jpg)

检查网络访问：![](/content/images/2016/07/8.jpg)

## 四、ssh和sftp
云桌面访问时延迟比较大，而且要配置一个舒服的编程环境比较难，所以开启ssh和sftp访问，就像操作VPS一样舒服。需要安装[openssh-server](http://packages.ubuntu.com/precise/amd64/openssh-server/download)和[openssh-client](http://packages.ubuntu.com/precise/amd64/openssh-client/download)。可以传入安装包或者开放网络访问后apt-get。

鼠标悬浮在“云桌面”上可见主机地址。

在windows上可以用winscp和putty连接：![](/content/images/2016/07/9.jpg)

如需使用X11，除参考网上教程设置以外，还需确保`sshd_config`文件中加上了这一行：
```
X11UseLocalhost no
```
客户端使用ssh加上-X参数连接便可以在命令行打开图形界面程序：![](/content/images/2016/07/----20160717014800.png)


## 五、使用其他客户端连接云桌面。

云桌面的客户端实现的功能其实是获取每个云桌面的独立ip地址并调用windows通用的远程桌面连接，传入ip地址和默认账户密码进行登录。所以可以使用各种客户端登录自己的云桌面。

<p class="note">使用其他客户端连接需要知道自己云桌面的独立ip，鼠标悬浮在“云桌面”上可见主机地址。还需要云桌面内的账户名和密码（不是学号）。</p>

### Win10自带的“远程桌面连接”
![](/content/images/2016/07/---1.JPG)

### Linux客户端“Remmina”
![](/content/images/2016/07/----20160712233251.png)

### Mac客户端
<a class="external" href="https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12">Microsoft remote desktop</a>


另还有rdesktop等许多客户端可供使用。