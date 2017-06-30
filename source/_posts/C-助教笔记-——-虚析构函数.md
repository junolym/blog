---
title: C++助教笔记 —— 虚析构函数
tags: |-

  - code
permalink: cpp-ta-note-1
id: 74
updated: '2017-05-06 13:01:37'
date: 2017-04-30 10:11:38
---

在作业题Product and Factory发现的问题。Product and Factory应用了工厂模式，通过调用不同工厂的生产函数，返回不同的对象指针。因此，对象的指针声明为基类对象指针。使用过后，自然要delete。问题就出在delete基类指针这里。

以下是部分同学的代码：(在main函数delete laptop)
```CPP
class Laptop {
public:
  virtual void compileCppProgram() = 0;
  ~Laptop() {}
};

class AppleLaptop : public Laptop {
public:
  void compileCppProgram() {
    cout << "Apple Laptop compiles a cpp program." << endl;
  }
};
```
静态检查报错如下：
![](/content/images/2017/04/----20170430161045.png)

如果按照第一个提示，将析构函数放到protected里，如下
```CPP
class Laptop {
public:
  virtual void compileCppProgram() = 0;
protected:
  ~Laptop() {}
};

class AppleLaptop : public Laptop {
public:
  void compileCppProgram() {
    cout << "Apple Laptop compiles a cpp program." << endl;
  }
};
```

则发生了编译错误
![](/content/images/2017/04/----20170430161208.png)

## 问题

### 1、为什么编译错误？
protected的函数不可以在外部被调用。

### 2、为什么静态检查引导你写出编译错误的代码？
把运行时可能发生的错误提前到编译时。

### 3、可能发生什么错误？
delete基类指针，只会调用基类的析构函数，并释放基类大小的内存。而派生类的析构函数不会被调用，造成派生类独有的资源没有被释放，即“内存泄露”。

### 4、正确做法：virtual析构函数
同样是静态检查给出的另一种建议。所以不要吐槽静态检查没用了。
```CPP
class Laptop {
public:
  virtual void compileCppProgram() = 0;
  virtual ~Laptop() {}
};

class AppleLaptop : public Laptop {
public:
  void compileCppProgram() {
    cout << "Apple Laptop compiles a cpp program." << endl;
  }
};
```

## 编程法则
*翻译自Effective C++ 第三版 条款7*

>  带多态性质的基类应该声明一个virtual析构函数。如果类带有任何virtual函数, 它就应该拥有一个virtual析构函数。

> 如果一个类不作为基类使用，或不带多态性质, 就不应该声明virtual析构函数。

### 小实验
不遵循这个法则，可以编译运行，但是造成了内存泄露。
改变的部分代码如下：
```CPP
class Laptop {
public:
  virtual void compileCppProgram() = 0;
  ~Laptop() {}
};

class AppleLaptop : public Laptop {
public:
  AppleLaptop() {
    model = new char[20];
  }
  ~AppleLaptop() {
    delete [] model;
  }
  void compileCppProgram() {
    cout << "Apple Laptop compiles a cpp program." << endl;
  }
private:
  char* model;
};
```
正常编译、运行、过了全部测试。除了静态检查报错，内存检查也不过。matrix给出内存错误：
![](/content/images/2017/04/----20170430182952.png)

### 后记
之前我出过一题Employee就是想考这个知识点，不过比这个题简单。这题应用的工厂模式也能给同学们带来一些启发，以及引出用基类指针指向派生类实例的实用性。

参考链接：http://codecloud.net/92703.html （原文关于effective C++的条款部分翻译有误）
