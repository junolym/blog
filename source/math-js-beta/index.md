---
title: math.js (beta)
permalink: math-js
id: 56
updated: '2016-10-27 09:58:31'
date: 2016-08-29 04:30:11
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/3.6.0/math.min.js"></script>

## 测试版使用说明
打开控制台，然后尽情调用math.js吧。官网：[mathjs.org](http://mathjs.org/) 。

## 例子
```javascript
// functions and constants
math.pow(2,10) //1024
math.round(math.e, 3);            // 2.718
math.atan2(3, -3) / math.pi;      // 0.75
math.log(10000, 10);              // 4
math.sqrt(-4);                    // 2i
math.pow([[-1, 2], [3, 1]], 2);
     // [[7, 0], [0, 7]]

// expressions
math.eval('1.2 * (2 + 4.5)');     // 7.8
math.eval('5.08 cm to inch');     // 2 inch
math.eval('sin(45 deg) ^ 2');     // 0.5
math.eval('9 / 3 + 2i');          // 3 + 2i
math.eval('det([-1, 2; 3, 1])');  // -7

// chaining
math.chain(3)
    .add(4)
    .multiply(2)
    .done(); // 14

for (var i = 0, ans = 0; i < 10; i++) ans += math.combinations(40, i);
//373585604
```