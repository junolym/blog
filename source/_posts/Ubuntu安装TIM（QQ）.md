---
title: Ubuntu安装TIM（QQ）
date: 2017-08-07 11:24:43
tags:
---
> ubuntu上使用qq的方法有很多，可以用wine或者crossover安装，qq版本又分普通版、轻聊版、国际版、TIM。本文介绍的方法是目前我尝试过的最接近完美的方案——crossover安装TIM

## 1. 安装crossover
下载crossover安装包，<a href="https://www.codeweavers.com/products/crossover-linux/download" target="_blank">下载地址</a> 目前最新版本为crossover_16.2.5-1。  
双击deb文件可以调用“软件管理器”安装。 命令行安装则是
```sh
sudo dpkg -i crossover_16.2.5-1.deb
```

## 2. crossover中安装TIM
这里建议使用TIM 1.2.0公测版，已测试可用。<a href="http://dldir1.qq.com/qqfile/qq/TIM1.2.0/19861/TIM1.2.0Trial.exe" target="_blank">下载地址</a>  
双击exe文件，调用crossover安装器打开。如果没有文件关联，就手动打开crossover，添加容器再安装应用。  
![](/content/images/2017/08/2017-08-07-10-57-14----.png)

安装完毕先不要登录，退出以完成剩下的创建图标等步骤。

## 3. 设置TIM
已知的问题：

1. 不能记住密码&自动登录，尚无解决方案。  
2. 双击打开的大图可能无显示，需要滚动一下鼠标滑轮。

建议设置 “接收文件保存目录”  
![](/content/images/2017/08/2017-08-07-11-01-53----.png)

在下载文件之后不要点击打开目录，使用ubuntu的文件管理器打开你设置的目录。

## 4. 激活crossover
试用版有14天的试用期。建议试用满意再购买，购买可先搜索优惠码。  
如想长期试用，下载<a href="https://web.chenjx.cn/crossover/winewrapper.exe.so" target="_blank">winewrapper.exe.so</a>，替换`/opt/cxoffice/lib/wine/`下的同名文件。
