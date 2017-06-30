---
title: deepin隐藏火狐浏览器最上面一条窗口
tags: |-

  - linux
permalink: deepin-tips
id: 8
updated: '2016-05-21 08:09:59'
date: 2016-04-06 12:01:55
---

安装一个插件即可，安装完无需重启即可看到效果，如果不行，可能是当前主题不支持，尝试更换一个。

插件名称：Hide Caption Titlebar Plus 

插件下载地址： 
https://addons.mozilla.org/zh-CN/firefox/addon/hide-caption-titlebar-plus-sma/

使用前：
![before](http://chenjx.cn/content/images/2016/04/----20160428004630.png)

使用后：
![after](http://chenjx.cn/content/images/2016/04/----20160428004641.png)



**Bug：**

Firefox 46 版本发现装了此插件以后关闭浏览器的按钮失效，只能关闭当前窗口。要退出浏览器需通过退出按钮。
（这个Bug也有一个好处，就是避免经常关闭火狐重开，因为火狐的启动速度实在不能忍，干脆就别关了）

___

**nautilus 添加 exfat 格式的支持**

比较新的debian系统只需安装一个插件就可以了。

执行命令：

`sudo apt-get install exfat-utils`

此时nautilus应该已经支持了，否则尝试重启。