---
title: ES6 之 let
date: 2017-08-20 17:59:17
tags:
---
## 块级作用域
在let和const出现之前，var声明的变量的作用域是函数体。  
先来一段ES5代码，每隔一秒钟打印一个数字（1-5）。
```js
for (var i = 1; i <= 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, i * 1000);
}
```
执行上述代码，如愿以偿地看到每秒出一个数字，但是打印的都是6。  
原因是在timeout回调函数里面调用的i，是外面的i的指针，而其值在循环结束时是6，因此回调函数读到的是6。  
在ES6以前，解决上述问题的方案叫做IIFE（立即执行函数表达式），如下
```js
for (var i = 1; i <= 5; i++) {
    (function(i) {
        setTimeout(function() {
            console.log(i)
        }, i * 1000);
    })(i);
}
```
而ES6的解决方案是，使用let声明i，使得i的作用域在for这个代码块，那么传到timeout回调函数的是i的值而不是其指针。
```js
for (let i = 1; i <= 5; i++) {
    setTimeout(function() {
        console.log(i)
    }, i * 1000);
}
```
## 不存在变量提升
var的变量提升：a在后面的语句声明，但其实a的作用域提升到整个函数体，只是在赋值语句之前a的值是1。
```js
console.log(a); // undefined
var a = 1;
```
let不存在变量提升，以下代码报错。
```js
console.log(a); // ReferenceError: a is not defined
let a = 1;
```

## 死区
一旦某个变量在代码块中用let声明，那么在这个代码块里面，let语句之前，就是这个变量的死区。访问会出错，哪怕是typeof也不安全。
```js
console.log(typeof(b)); // undefined
console.log(typeof(a)); // ReferenceError: a is not defined
let a = 1;
```
以上代码，b从头到尾没有声明，不存在死区，typeof没报错。而typeof(a)则报错了。

## 全局变量和顶层对象属性脱钩
顶层对象，在浏览器中是`window`，在node里是`global`。以往的全局变量就是顶层对象的一个属性。
```js
// 浏览器中
var a = 1;
console.log(window.a); // 1
```
> 顶层对象的属性与全局变量挂钩...带来了几个很大的问题，首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道...；其次，程序员很容易不知不觉地就创建了全局变量...；最后，顶层对象的属性是到处可以读写的，这非常不利于模块化编程。另一方面，...顶层对象是一个有实体含义的对象。
—— 阮一峰《ES6标准入门》

好了，ES6规定，全局变量和顶层对象脱钩了。
```js
let a = 1;
console.log(window.a); // undefined
```
