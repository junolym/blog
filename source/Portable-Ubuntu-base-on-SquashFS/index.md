---
title: Portable Ubuntu base on SquashFS
permalink: portable-ubuntu
id: 53
updated: '2016-08-03 05:48:42'
date: 2016-08-02 13:09:18
---

<p class="excerpt">基于SquashFS建立的Ubuntu永久便携系统</p>
## 大致目标：
创建一个可以携带在U盘的易用易修改，数据永久保持的Ubuntu。

## 面向对象：
* 偶尔使用ubuntu但不想安装第二系统或虚拟机
* 作为紧急维护系统使用
* 在无硬盘小内存的机器上运行完整ubuntu
* 在公共电脑上运行自己的ubuntu

## 项目阶段：
* 一、基于ubuntu 16.04构建系统，可运行，可保持数据。
* 二、完善安装方式，可轻松放进U盘或者本地磁盘。
* 三、维护系统内应用，完善不同场景下的使用。

## 项目难点：
* 建立可读写的Linux文件系统（Ubuntu live系统的SquashFS只读不可写）
* 简单快速地从文件系统启动，且能适应传统BIOS、UEFI BIOS、嵌入式设备。
* 文件系统的自动扩大和自动缩小，尽量节约磁盘空间。
* U盘格式局限（从NTFS/EXFAT启动或者分文件存储应对FAT32的文件大小限制）

## 参考资料
http://tldp.org/HOWTO/SquashFS-HOWTO/creatingandusing.html

## 项目进度：
### 阶段一
*2016年8月2日~2016年8月4日*

<br><br><br><br>
<div id="uyan_frame"></div>  
<script type="text/javascript">uyan_loaded = uyan_loadover = undefined</script>  
<script type="text/javascript" src="http://v2.uyan.cc/code/uyan.js?uid=2097190"></script>