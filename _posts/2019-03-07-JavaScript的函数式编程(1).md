---
layout: post
title:  "2019-03-07-JavaScript的函数式编程(1)"
date:   2019-03-07 21:00:00 +0800
categories: 前端
tag: 笔记
---

* content
{:toc}



## 函数式编程 Funtional programming

函数式编程是一种编程范式，也就和如何写程序的方法论，属于结构化编程的一中，主要思想是把运算过程尽量写成一系列嵌套的函数调用

**学习函数式编程需要了解什么？**

- 了解什么时候范畴论
- 数学模型和范畴之间的关系
- 范畴与函数式编程的关系

## 什么是范畴论？

函数式编程的起源，是一门叫范畴论（Caregory Theory）的数学分支

理解函数式编程的关键，就是理解范畴论。它是一门复杂的数学，认为世界上的所有的概念体系，都可以抽象成一个的‘范畴’

**范畴就是使用箭头连接的物体**

通俗的讲，就是彼此之间存在某种关系的概念、事务、对象等等，都构成范畴。

![image](https://ws1.sinaimg.cn/large/006tKfTcgy1g0ujbfe6jej30hq0dbjrg.jpg)

箭头表示范畴成员之间的关系，正式名称叫做“态射”（morphism）。范畴论任务，同一个范畴的所有成员，就是不同状态的“形变”。通过“态射”就可以从一个成员形变成另一个成员。

### 数学模型
既然“范畴“是满足某种形变关系的所有对象，就可以总结出来它的数学模型。

```
1.所有成员是一种集合
2.形变关系是函数
```
也就是说，范畴论是集合论更上层的抽象，简单的理解就是“集合+函数”

理论上通过函数，就可以从范畴的一个成员，算出其他所有成员

### 范畴与容器

如果把“范畴”想象成一个容器，里面包含两样东西。
```
值（value）
值得变形关系，（函数）
```
下面我们来定义一个简单的范畴

```
    class Category {
        // 实例化时，传过来的参数
        constructor(a, b, c) {
            this.val = a + b + c;
        }

        addOn(x) {
            return x + 1;
        }
    }

    var a = new Category(12, 3, 4);

    console.log(a); // Category {val: 19}
```

上面代码中，Category是一个类，也是一个容器，里面包含了一个值(this.val)和一种变形关系(addOne)。该关系就是彼此之间都相差1的数字。

### 范畴论和函数式编程的关系

> 范畴论使用函数，表达范畴之间的关系

伴随着范畴论的发展，就发展除了一整套函数的运算方法。方法起初只是适用于数学算法，后来有人将它在计算机上实现了，成为了今天的“函数式编程”。

**本质上，函数式编程只是范畴论的预算方法，跟数学逻辑、微积分、行列式都是一类东西，都是数学方法，只是正好用它来写程序。**

## 函数的合成与柯里化

函数式编程有2个最基本的运算
- 合成
- 柯里化


### 合成

如果一个值要经过多个函数，才能变成另外一个值，就可以把所有的中间步骤合并成一个函数，叫做“函数的合成”

![image](https://ws1.sinaimg.cn/large/006tKfTcgy1g0ujbv9nszj309608swec.jpg)

上图中，X和Y之间的变形关系是函数f，Y和Z的变形关系是g，那么X和Z之间的关系，就是g和f的合成函数g·f。

```
    const compose = function (f, g) {
        return function (x) {
            return f(g(y));
        }
    }
```

![image](https://ws4.sinaimg.cn/large/006tKfTcgy1g0ujc6qw2fj30m808pdfz.jpg)

函数的合成还必须要满足结合律

```
compose(f,compse(g.h));
// 等同于
compose(compose(f,g),h);
// 等同于
compose(f,g,h);
```
### 柯里化
f(x)和g(x)合成为f(g(x)),有一个隐藏的前提，就是f和g都只能接受一个参数。如果接受多个参数，比如f(x,y)和g(a,b,c)，函数合成就很麻烦。
所以需要函数柯里化，把一个多参数的函数，转化为一个单参数的函数；

```
    "use strict";

    // 未柯里化
    function add(x, y) {
        return alert(x + y);
    }

    // 柯里化后
    function Xadd(x) {
        return function (y) {
            return alert(x + y);
        }
    }

    Xadd(2)(3);// 5
```
有了柯里化以后，我们就能做到，所以函数只能接受一个参数。

> 这里面包含了ES6的class、constructor等特性

### JavaScript的class

在ES5的时候，我们编写JavaScript很多时候只能使用构造函数和原型链进行方法属性，实现class的功能

```
    // ES5环境
    'use strict';
    // Box是一个构造函数
    function Box(val) {
        this.type = 'double';
        this.color = val;
    }
    // 我们通过prototype的方式来添加一条属性
    Box.prototype.hello = function () {
        console.log('hello,' + this.type + "," + this.color);
    }

    // 对于私有属性（static method），  我们当然不能放在原型链上，我们直接放在构造函数上面。
    Box.fn = function () {
        console.log('static');
    }

    //通过new来创建
    var circle = new Box('red');
    circle.hello(); // hello,double,red
```

但是在ES6的规范中，可以使用class语法，ES6的class可以看做只是一个语法糖，它绝大部分都可以做到，新的class写法只是让对象原型的写法更加清晰、更想面对对象编程的语法。

```
    // es6
    class BoxES6 {
        constructor(val) {
            this.type = 'double';
            this.color = val;
        }
        hello() {
            console.log('hello,' + this.type + ',' + this.color);
        }
    }
    
    //通过new来创建
    var helloBlue = new BoxES6('blue');

    helloBlue.hello(); // hello,double,blue
```
