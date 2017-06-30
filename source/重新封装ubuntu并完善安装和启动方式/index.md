---
title: 重新封装ubuntu并完善安装和启动方式
permalink: ubuntu-repack
id: 55
updated: '2016-08-24 15:02:43'
date: 2016-08-03 08:58:21
---

<p class="excerpt">将以live permanent方式安装的ubuntu的casper-rw和原系统封装成新的filesystem.squashfs文件，并完善其安装和启动的方式，以适应多种平台</p>

## 背景
* ubuntu live 系统可将用户试用ubuntu时所作的修改保存在卷标为casper-rw的分区，并在下次启动读取。其原理是unionfs文件系统的overlay方式。
* unetbootin、YUMI等软件可将上述所需的casper-rw分区以一个固定大小的文件保存，并在启动ubuntu时自动挂载。但此文件只能保存到Fat32分区。
* 当用户删除原试用系统中的软件时，删除掉的文件仍然占用着空间。并且新加入和更新的文件以不压缩的形式保存在casper-rw文件中。

## 目标
* 将casper-rw中保存的修改合并到filesystem.squashfs文件中，使新加入的文件以SquashFS的高效压缩的形式整合到系统中，使删除掉的文件从系统中消失，不再占用空间。
* 完善YUMI等软件创建USB启动系统的方式，使其适应更多机器环境，既能携带在U盘中，也可以放置在本地的高速硬盘中。

## 技术文档
### 封装系统为filesystem.squashfs
以unetbootin安装方式为例。
<p class="note">以下命令所在目录和casper-rw文件同目录，如果是unetbootin安装的，即是所安装的磁盘的根目录</p>

1、挂载casper-rw和filesystem.squashfs。
```line-numbers
# mkdir /tmp/rw /tmp/ro
# mount casper-rw /tmp/rw
# mount casper/filesystem.squashfs /tmp/ro
```

2、合并挂载overlay
```line-numbers
# mkdir /tmp/merged
# mount -t overlay overlay -olowerdir=/tmp/ro,upperdir=/tmp/rw/upper,workdir=/tmp/rw/work /tmp/merged
```

3、封装到新的filesystem.squashfs  
（执行此命令前请确保所在文件夹里没有原来的filesystem.squashfs文件）
```line-numbers
# mksquashfs /tmp/merged filesystem.squashfs
```

4、清空casper-rw并替换filesystem.squashfs
```line-numbers
# rm -rf /tmp/rw/*
# umount /tmp/rw
# umount /tmp/ro
# mv filesystem.squashfs casper/filesystem.squashfs
```

### 打包ISO
以下是一行命令，在光盘文件夹下执行，得到的ISO文件在上一级目录
```line-numbers
# mkisofs -D -r -V "Ubuntu LJ 16.04.1" -cache-inodes -J -l -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -o ../LJ1.0.iso .
```


## 任务
### 阶段一
* 调整用户账户等系统设置
* 卸载不必要的自带程序
* 执行程序更新（upgrade）
* 完成一次封装，即得到纯净版系统

### 阶段二
* 根据不同的需求定制系统
* 熟悉定制流程
* 以程序员中文版为注，定制病封装得到ISO文件。

### 阶段三
* 完善安装和启动方式，适应多平台

## 记录
已卸载程序列表（不包括自动卸载的依赖）
```
activity-log-manager, aisleriot, unity-webapps-common, gnome-calendar, 
deja-dup, cheese, ubuntu-desktop, checkbox-converged, checkbox-gui, 
gnome-mahjongg, gnome-mines, unity-control-center-signon, 
system-config-printer-gnome, rhythmbox-plugins, rhythmbox-plugin-zeitgeist, 
rhythmbox, gnome-sudoku, xterm, gnome-system-log, libreoffice*, 
thunderbird*, gnome-orca, shotwell-common, shotwell, simple-scan, 
gucharmap, gnome-calculator, unity-scope-calculator, gnome-screensaver, 
gnome-accessibility-themes, mozc-*, transmission-*, xdiagnose
```
封装时遇到的问题：

* 封装的ISO出现碎片过多
* 更新的内核在ISO中启动失败，在USB中启动成功
* Nautilus和Software center异常

解决方法：

* 封装ISO时使用最新的16.04.1的安装盘
* 卸载Software center

程序员中文版所做的修改

 - 安装搜狗拼音输入法
 - 关闭自动检查更新
 - 安装sublime
 - 修改浏览器主页和、搜索引擎

## 成果
Ubuntu中文版（基于16.04.1）
<a class="external" href="http://pan.baidu.com/s/1boPNHaZ">http://pan.baidu.com/s/1boPNHaZ</a>

Ubuntu-Mate中文版（基于16.04.1）
<a class="external" href="http://pan.baidu.com/s/1gfz7vVX">http://pan.baidu.com/s/1gfz7vVX</a>
