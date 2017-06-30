---
title: Ubuntu Grub引导修复
tags: |-

  - linux
permalink: fixgrub
id: 48
updated: '2016-07-25 14:35:57'
date: 2016-07-05 13:20:28
---

Grub引导修复的方法有几种：LiveCD/USB修复、Grub恢复模式修复、Grub4Dos修复等。根据系统启动方式的不同（MBR or EFI），每种方法具体操作不同。本文记录下个人修复的经验。

## MBR模式——进入Live系统重安装Grub
### 1、找到原系统所在分区
`fdisk -l`命令列出分区，找到原系统所在分区，下面例子中的/dev/sda7。如果安装系统时单独分出了/boot，也需要找到该分区。

### 2、执行以下命令
```
# mount /dev/sda7 /media/mnt
# mount /dev/sda8 /media/mnt/boot （如果boot单独分区）
# chroot /media/mnt
# grub-install -root-directory=/mnt /dev/sda
```


## EFI模式——进入Live系统替换EFI文件
**注：本方法要求系统可以启动进入windows，适用于无法手动添加EFI启动项时。**
### 1、挂载EFI分区
命令`fdisk -l`列出分区，找到EFI分区号（下面例子中的sda2）并挂载：
```
# mount /dev/sda2 /media/mnt
```

### 2、备份windows的efi文件
```
# cd /media/mnt/EFI/Microsoft/Boot/
# cp bootmgfw.efi win.efi
```

### 3、替换ubuntu的efi文件
```
# cd /media/mnt/EFI/
# cp ubuntu/grubx64.efi /Microsoft/Boot/bootmgfw.efi
```

**此时重启，应该可以看到熟悉的Grub画面了。**

### 4、找回windows启动项
如果刚刚的Grub启动选择菜单中已经有windows启动项，但是无法启动，修改/boot/grub/grub.cfg，将bootmgfw.efi替换为win.efi。

如果Grub启动菜单中没有windows项，可以尝试`update-grub`命令。

___

*等待补充*
