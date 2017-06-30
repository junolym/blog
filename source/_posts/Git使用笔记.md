---
title: Git使用笔记
permalink: git-notes
id: 79
updated: '2017-06-30 02:37:32'
date: 2017-06-28 03:35:31
tags:
---

## 重置：撤销最近的n个提交，或重新提交
> 问题情景：提交的东西有问题，但是不想留下黑历史，或者东西是不可告人的（例如把用户名密码交上去了）

使用git的重置（reset）功能，默认是软重置（--soft）只撤销commit，不改变本地文件。加上`--hard`则是把本地文件也重置。后面传入的是**最后一次正确提交的hash**。

## 变基：在很多提交中挑选有用的，废弃错误的修改
> 问题情景：n多个提交中，有一部分是有用的，一部分是错误的。而且错误的比较多，用fixup会比较难看。

使用git的变基功能（rebase），然后`pick`有用的提交，`drop`用的。还可以标记`fixup`。具体看文档吧。这里只是给一个解决方案的思路。

## 修改过去的commit记录的名字和邮箱
> 问题情景：git全局配置的用户名和邮箱是github用的，和在公司gitlab上用的不一样，然后commit完才发现名字格式跟别人不一样，邮箱也不是公司邮箱。

Learned from https://lmbj.net/blog/how-do-i-change-the-author-of-a-commit-in-git/
```shell
# !/bin/sh

git filter-branch --env-filter '

an="$GIT_AUTHOR_NAME"
am="$GIT_AUTHOR_EMAIL"
cn="$GIT_COMMITTER_NAME"
cm="$GIT_COMMITTER_EMAIL"

if [ "$GIT_COMMITTER_EMAIL" = "your@email.to.match" ]
then
    cn="Your New Committer Name"
    cm="Your New Committer Email"
fi
if [ "$GIT_AUTHOR_EMAIL" = "your@email.to.match" ]
then
    an="Your New Author Name"
    am="Your New Author Email"
fi

export GIT_AUTHOR_NAME="$an"
export GIT_AUTHOR_EMAIL="$am"
export GIT_COMMITTER_NAME="$cn"
export GIT_COMMITTER_EMAIL="$cm"
'
```

## git指定commit的时间
> 问题情景：过了交作业的ddl了。。。

git commit加上参数`--date`，并设置GIT_COMMITTER_DATE变量。传入时间支持Git internal format、RFC 2822、ISO 8601。
```
MYDATE=`date --rfc-2822 --date="2 days ago"`
GIT_COMMITTER_DATE=$MYDATE git commit --amend --date "$MYDATE"
```

或者使用filter-branch
```
git filter-branch --env-filter \
 'if [ $GIT_COMMIT = 119f9ecf58069b265ab22f1f97d2b648faf932e0 ]
 then
 export GIT_AUTHOR_DATE="Fri Jan 2 21:38:53 2009 -0800"
 export GIT_COMMITTER_DATE="Sat May 19 01:01:01 2007 -0700"
 fi'
```

## 参考文章
https://blog.louie.lu/2017/06/01/%E5%85%A8%E9%9D%A2%E7%AB%84%E6%94%B9-git-commit-%E6%AD%B7%E5%8F%B2%E8%A8%98%E9%8C%84/