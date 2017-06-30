---
title: Nodejs 爬虫工具
tags: |-

  - code
permalink: superagent-cheerio
id: 28
updated: '2016-05-15 17:38:17'
date: 2016-05-08 09:28:36
---

##superAgent
模拟客户端发出请求和处理结果

##cheerio
Node.js 版的 jquery，装载入网页后，使用方式跟 jquery 一样一样的。

##相关文档
https://github.com/alsotang/node-lessons/tree/master/lesson3

[https://cnodejs.org/topic/5378720ed6e2d16149fa16bd](https://cnodejs.org/topic/5378720ed6e2d16149fa16bd)

##能干嘛
爬取教务系统数据，发出选课请求。
让抢课脱离浏览器，直接在node里没日没夜地跑^_^

##成果记录
实现从教务系统获取课程表和成绩数据。

Api工作模式：
1、客户端get方法获取验证码，服务端向源服务器请求验证码，然后向客户端返回验证码的同时记录下cookie。
2、客户端post方法发送用户名密码和验证码，服务端加上之前的记录下的cookie，向源服务器发送登陆请求。成功之后记录下更多的cookie并get想要的数据，然后整理并返回客户端。

##后续阅读
[《Node.js 包教不包会》](https://github.com/alsotang/node-lessons)

[JavaScript Promise](http://liubin.org/promises-book/)

[Node.js 中用 Q](http://www.ghostchina.com/promises-in-node-js-with-q-an-alternative-to-callbacks/)

[在Node.js中使用promise摆脱回调金字塔](http://nya.io/Node-js/promise-in-nodejs-get-rid-of-callback-hell/)