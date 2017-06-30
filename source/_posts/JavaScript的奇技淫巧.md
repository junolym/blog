---
title: JavaScript的奇技淫巧
permalink: small-skills
id: 66
updated: '2017-06-20 17:18:44'
date: 2016-12-02 13:00:31
tags:
---

## 1. 百度网盘自定义分享密码

*学习自[老D](https://laod.cn/black-technology/baidu-wangpan-tiquma.html)* 

百度网盘私密分享的文件不容易被取消，可是提取的时候要输入4个字符的提取码。  
如果能自定义提取码是不是很美妙呢？ //给妹子分享文件告诉她提取码是她生日^^

方法在这里，按照步骤来：

#### 1. 选中文件，点击分享。先不要点私密分享！
#### 2. Ctrl+Alt+I（或F12）打开浏览器的控制台，在`Console`页粘贴如下代码并回车。
```js
javascript:require(["function-widget-1:share/util/service/createLinkShare.js"]).prototype.makePrivatePassword=function(){return prompt("请输入提取码","1024")}
```
#### 3. 点击私密分享，在弹出框中输入4个字符，中文占3个字符。

实在不明白可以看老D的[原文](https://laod.cn/black-technology/baidu-wangpan-tiquma.html)，有动图。  
不要问我为什么默认是1024。不要想歪。

## 2. 使用多行字符串

学习自： http://www.cnblogs.com/ziyunfei/archive/2012/10/04/2711551.html
```js line-numbers
function hereDoc(f) {　
    return f.toString().replace(/^[^\/]+\/\*!?\s?/, '').replace(/\*\/[^\/]+$/, '');
}
var string = hereDoc(function () {/*
我
你
他
*/});
console.log(string);
```

我的简化版
```js line-numbers
var multiline = (function() {/*
"shaungyinhao"
'danyinhao'
*/}).toString().slice(16, -3);

console.log(multiline);
```

将-3改为-5则去掉最后的换行。

javascript函数转字符串的格式固定，这里使用硬编码没有问题。

## 3. 获取中文日期和时间
这不是什么难题，但是传统的方法是通过Date对象的getYear等函数得到日期时间的各部分数字，然后再拼凑起来。其实有更简便的方法，利用JavaScript的toLocaleString格式化成不同区域的标准。如果还需要进一步格式化，使用字符串替换也可以实现大多数需求。如下
```js
// 普通格式
(new Date()).toLocaleString('zh-CN', { hour12 : false })
// 进一步替换
(new Date()).toLocaleString('zh-CN', { hour12 : false }).replace(/[\/|-]/, '年').replace(/[\/|-]/, '月').replace(/ /, '日 ')
```

得到结果如下：（可在不同浏览器访问本页检查兼容性）  
<a href="javascript:$('#date1').html((new Date()).toLocaleString('zh-CN', { hour12 : false }));$('#date2').html((new Date()).toLocaleString('zh-CN', { hour12 : false }).replace(/[\/|-]/, '年').replace(/[\/|-]/, '月').replace(/ /, '日 '))">显示结果</a>  
普通格式：<span id="date1"></span>  
替换后：<span id="date2"></span>
</script>

## 4. 生成随机字符串
 - 生成随机数用`Math.random()`就可以了。得到的是0~1的随机数。
 - 如果需要得到一定范围内`[minNum, minNum+range)`的随机整数，用
```js
Math.floor(Math.random() * range) + minNum;
```
 - 如果需要得到随机字符串呢？一种非常简便的做法是
```js
Math.random().toString(36).substr(2)
```
得到的是数字和小写字母组成的随机字符串。其中36表示进制，支持2~36，默认10。机智的你应该很快发现，当这个数为奇数时，生成的字符串会特别长。这种方法最大的局限就是最多只能包括小写字母，不能大小写混合。

 - 手动实现，支持任意字符集。代码就很难看了，不如一行风骚。
```js
var chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXTZabcdefghiklmnopqrstuvwxyz'.split('');
var key = "";
for (var i = 0; i < config.length; i++) {
    key += chars[Math.floor(Math.random()*chars.length)];
}
```
这段代码是我为了生成随机的二维码的key写的，类似短网址，为了尽量缩短字符串长度，使用了62个字符。

## 5. Excel列名
Excel表格里面，列的名字是A~Z，但又不是简单的26进制，因为Z的下一个是AA，因此A既不是0也不是1。于是写了个函数，传入列的数字编号，得到Excel风格的列名。 传入0或负数不会出错。
```js
// 1 => A, 26 => Z, 27 = AA
function excelColName(n) {
    var c = '';
    do {
        c = String.fromCharCode(--n % 26 + 65) + c;
    } while(n = Math.floor(n / 26) > 0);
    return c;
}
```