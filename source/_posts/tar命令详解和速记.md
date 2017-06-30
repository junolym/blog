---
title: tar命令详解和速记
tags: |-

  - linux
permalink: tar-command
id: 33
updated: '2016-08-02 06:11:32'
date: 2016-05-26 05:31:38
---

Linux下最常用的打包程序就是tar了，使用tar程序打出来的包我们常称为tar包，tar包文件的命令通常都是以.tar结尾的。生成tar包后，就可以用其它的程序来进行压缩了。
##tar命令的基本用法
　　tar命令的选项有很多(用man tar可以查看到)，但常用的就那么几个选项，下面来举例说明一下： 
```
    $ tar -cf all.tar *.jpg 
```
　　这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名。 
```
    $ tar -rf all.tar *.gif 
```
　　这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件。 
```
    $ tar -uf all.tar logo.gif 
```
　　这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。 

```
    $ tar -tf all.tar
```
　　这条命令是列出all.tar包中所有文件，-t是列出文件的意思 
```
    $ tar -xf all.tar
```
　　这条命令是解出all.tar包中所有文件，-x是解开的意思 
 　为了方便用户在打包解包的同时可以压缩或解压文件，tar提供了一种特殊的功能。这就是tar可以在打包或解包的同时调用其它的压缩程序，比如调用gzip、bzip2等。 

##调用其他压缩算法

###1) tar调用gzip 
　　gzip是GNU组织开发的一个压缩程序，.gz结尾的文件就是gzip压缩的结果。与gzip相对的解压程序是gunzip。tar中使用-z这个参数来调用gzip。下面来举例说明一下 ： 
```
    $ tar -czf all.tar.gz *.jpg
```
　　这条命令是将所有.jpg的文件打成一个tar包，并且将其用gzip压缩，生成一个gzip压缩过的包，包名为all.tar.gz 
```
    $ tar -xzf all.tar.gz
```
　　这条命令是将上面产生的包解开。 
###2) tar调用bzip2 
　　bzip2是一个压缩能力更强的压缩程序，.bz2结尾的文件就是bzip2压缩的结果。与bzip2相对的解压程序是bunzip2。tar中使用-j这个参数来调用gzip。下面来举例说明一下： 
```
    $ tar -cjf all.tar.bz2 *.jpg
```
　　这条命令是将所有.jpg的文件打成一个tar包，并且将其用bzip2压缩，生成一个bzip2压缩过的包，包名为all.tar.bz2 
```
    $ tar -xjf all.tar.bz2
```
　　这条命令是将上面产生的包解开。 
###3)tar调用compress 
　　compress也是一个压缩程序，但是好象使用compress的人不如gzip和bzip2的人多。.Z结尾的文件就是bzip2压缩的结果。与 compress相对的解压程序是uncompress。tar中使用-Z这个参数来调用compress。下面来举例说明一下： 
```
    $ tar -cZf all.tar.Z *.jpg
```
　　这条命令是将所有.jpg的文件打成一个tar包，并且将其用compress压缩，生成一个uncompress压缩过的包，包名为all.tar.Z 
```
    $ tar -xZf all.tar.Z
```
　　这条命令是将上面产生的包解开 

##命令参数速记

###操作类别

-c新建；-x解压；-t查看；-r追加；-u更新。

###压缩格式
-z：gzip（.tar.gz，.tgz）
-j：bz2 （.tar.bz2）
-Z：compress （.tar.Z）

###额外功能
-v：显示所有过程 
-O：将文件解开到标准输出 

###指定文件名
-f （这个参数是最后一个参数，后面只能接档案名。）

___
*转载自http://www.jb51.net/LINUXjishu/43356.html 稍加整理*