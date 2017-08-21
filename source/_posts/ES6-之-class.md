---
title: ES6 之 class
date: 2017-08-21 20:40:40
tags:
---
class是ES6新增的关键字，迟来的类啊。在此之前，我们是这样定义和使用“类”的
## 定义类
```js
function Point(x, y) {
  this.x = x;
  this.y = y;
}
Point.prototype.print = function () {
  console.log("(%d, %d)", this.x, this.y);
};

var p = new Point(1, 2);
p.print();
```
无力吐槽，语义真的差。用了ES6的class，就可以下面这样子
```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  print() {
    console.log("(%d, %d)", this.x, this.y);
  }
}

let p = new Point(1, 2);
p.print();
```
记几点简单的使用注意：

1. 不存在变量提升，即必须先声明，再使用。
2. `constructor`如没有显式定义，则表示空的`constructor`。
3. 支持表达式的形式，如`const Foo = class { /* ... */ }`
4. 无私有方法/属性，有静态方法（用`static`修饰），无静态属性（可在类外直接赋值实现）
5. 支持取值（getter）和存值（setter）方法，分别使用`get`和`set`修饰（栗子1）
6. 类的所有实例共享一个原型对象，即`__proto__`相同。
7. `name`属性：类的名字，尤其注意当使用表达式形式时，name优先使用`class`关键字后面的名字。（栗子2）
8. 如果某个方法之前加上星号`*`，就表示该方法是一个 `Generator` 函数。（栗子3）
9. `new.target`属性：表示构造函数被调用的方法。（栗子4）

## 类的继承
`extends`关键字。下面这段代码继承了`Array`创造一个`Pool`类，并增加一个`next`方法用于遍历这个pool，每次调用返回一个值。
```js
class Pool extends Array {
    constructor(...values) {
        super(...values);
        this.cursor = 0;
    }
    next() {
        let value = this[this.cursor++];
        if (this.cursor >= this.length) {
            this.cursor = 0;
        }
        return value;
    }
}

const pool = new Pool(1, 2); // 类似Array的构造函数
pool.push(3); // 使用Array的方法
pool.next(); // 1
pool.next(); // 2
pool.next(); // 3
pool.next(); // 1
pool // Pool [ 1, 2, 3, cursor: 0 ]
```
几个细节：

1. `super`可表示父类的构造函数。注意必须调用`super`之后才可以使用`this`。
2. `super`还可作为对象。在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
3. 通过`super`调用父类的方法时，`super`会绑定子类的`this`。

---

栗子1： getter 和 setter
```js
class Foo {
    constructor(v) {
        this.value = v;
    };
    get bar() {
        console.log('got value:', this.value);
        return this.value;
    };
    set bar(v) {
        console.log('before set:', this.value);
        this.value = v;
        console.log('after set:', this.value);
    }
}
const foo = new Foo(0);
foo.bar += 1;
/*
got value: 0
before set: 0
after set: 1
*/
```
栗子2： `name`属性
```js
class Foo {}
let foo = new Foo();
console.log(Foo.name); // Foo
console.log(foo.constructor.name); // Foo
const Bar1 = class {};
const Bar2 = class T {};
console.log(Bar1.name); // Bar1
console.log(Bar2.name); // T
```
栗子3： 生成器函数，`Symbol.iterator`方法
```js
class Foo {
  constructor(...args) {
    this.args = args;
  }
  * [Symbol.iterator]() {
    for (let arg of this.args) {
      yield arg;
    }
  }
}

const foo = new Foo('hello', 'world');
for (let x of foo) {
  console.log(x);
}
// hello
// world
```
栗子4：利用`new.target`实现抽象类
```js
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    // ...
  }
}

let x = new Shape();  // 报错
let y = new Rectangle(3, 4);  // 正确
```
