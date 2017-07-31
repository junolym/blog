---
title: Python学习小记
date: 2017-07-17 20:57:57
tags:
---
> 接触python一月余，踩过一些坑，在这里记录下来。

## 对象 or Dict
再次怀念js...
1. 访问内部元素
 - 对象（python中特指类的实例）访问元素： `object.key` or `getattr(object, "key")`
 - Dict（字典，一系列可嵌套的键值对）: `dict["key"]` or `dict.get("key")`

2. 判断有无某种属性
 - 对象: `hasattr(object, "key")`
 - Dict: `"key" in dict`

## 生成器中return
```
SyntaxError: 'return' with argument inside generator
```
这个报错的意思是，在一个generator中不能有带值的return（不带值可以）。generator其实就是一个函数，里面包含yield。

所以刚看到这个报错时很崩溃，我明明没改这句，只是在别的地方加了个yield，这边的return就报错了。。
