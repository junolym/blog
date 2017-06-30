---
title: 把Ubuntu装进U盘
permalink: ba-ubuntuzhuang-jin-upan
id: 75
updated: '2017-05-15 13:54:18'
tags:
---

把Ubuntu装进U盘，不就是把ISO写进U盘，作为USB-live启动吗？基本上是这样的，但是如果只是如此，就不需要写这篇文章了。

## 先说说大概操作步骤：  

1. 下载ubuntu的ISO镜像文件。
2. 使用YUMI、unetbootin等工具写入U盘。
3. [可选]如果需要持久化修改，在上一步创建用于保持修改的casper-rw文件。
4. [可选]如果需要大可用空间(>4GB)，在U盘创建一个卷标为`casper-rw`的分区，并删除上一步创建的casper-rw文件。
5. [进阶]将casper-rw中的修改合并，得到DIY的ubuntu系统ISO。

1和2没什么好说的。3也没什么特别的，就是在对应工具里选择“创建用于持久化数据的文件”之类的选项。

## 重点来啦。[敲黑板]
### 关于casper-rw分区
前面说过，casper-rw可以是一个文件，其实它也可以是一个卷标为casper-rw的分区。类似的，还有home-rw。

什么意思呢？rw当然是read and write的意思，区别于ro(read only)。casper就是这种持久化数据(Persistent)方法的名称。casper-rw可提供/下所有数据的持久化，home-rw则是/home目录下。

要解释持久化，就要知道什么是临时的。以普通方式启动U盘里的ubuntu，或者从光盘启动，所有原有的文件存储在`filesystem.squashfs`，这个文件是只读的。启动后对整个系统的任何修改，都是暂存在内存中。因此是临时的，重启即丢失。

那么casper-rw(或者home-rw)是怎样保存修改的呢？这里用到的是union filesystem，具体格式通常是aufs或overlayfs，也是Docker常用的文件系统。所谓UnionFS，就是把不同物理位置的目录合并挂载到同一个目录中。在这里，也就是把只读的filesystem.squashfs和可写的casper-rw合并挂载为/。

借docker.com的一张图说明一下。这图原本是说明aufs删除文件（file3）的原理，其实也包括了增加文件（file1、file2）和更新文件（file4）。
![](/content/images/2017/05/aufs_delete.jpg)