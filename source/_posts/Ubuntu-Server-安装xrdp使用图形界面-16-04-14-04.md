---
title: Ubuntu Server 安装xrdp使用图形界面 (16.04/14.04)
tags: |-

  - VPS
permalink: ubuntu-server-xrdp
id: 52
updated: '2016-08-06 06:01:42'
date: 2016-07-29 12:17:34
---

## 一、安装xrdp
Ubuntu 16.04只需安装xrdp即可。
```
# apt-get install xrdp
```
Ubuntu 14.04还需安装vnc4server
```
# apt-get install xrdp vnc4server
```
此时使用客户端连接，应该能够连接成功。

Ubuntu 16.04出现命令行画面。

Ubuntu 14.04画面为空白。

## 二、安装兼容的桌面环境
兼容的桌面环境有gnome-classic，xfce，mate等

Unity不兼容。

以下两种任选其一。
### 1、精简小巧的xfce
```
# apt-get install xfce4
```
![](/content/images/2016/07/---2.JPG)
这是xubuntu的桌面核心部分。

### 2、精美易定制的mate
```
# apt-get install mate-core mate-desktop-environment mate-notification-daemon
```
![](/content/images/2016/08/--.JPG)

## 三、使用客户端连接
win10可用自带的“远程桌面连接”。

Linux推荐使用Remmina。

## 四、解决其他问题
### 1、中文字体缺失
Ubuntu14.04安装fonts-droid:
```
# apt-get install fonts-droid
```
Ubuntu16.04安装fonts-noto-cjk:
```
# apt-get install fonts-noto-cjk
```

### 2、xfce中terminal中tab键失效
在菜单中找到“Window Manager”或终端执行`xfwm4-settings`打开窗口管理器，在`keyboard`标签下将`Switch window for same application`的快捷键清除。
