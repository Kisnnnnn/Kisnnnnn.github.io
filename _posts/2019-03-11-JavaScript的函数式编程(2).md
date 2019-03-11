---
layout: post
title:  "2019-03-07-JavaScript的函数式编程(1)"
date:   2019-03-07 21:00:00 +0800
categories: 前端
tag: 笔记
---

* content
{:toc}



> 本文中的知识点，我也不是很懂，比较难理解。

## 涵子

### 什么是涵子

涵子是函数式编程里面最重要的数据类型，也是最基本的运算单位和功能单位。

它是一种范畴（也是一种容器），包含了值和变形关系。比较特殊的是，它的变形关系可以依次作用于每一个值，**将当前容器变形为另一个容器**。

![image](https://ws2.sinaimg.cn/large/006tKfTcgy1g0yw1xe4azj30of081q34.jpg)

上图中，左侧的圆圈就是一个汉字，表示人名的范畴。外部传入函数f，会转成右边标示早餐的范畴。

### 涵子的代码实现

任何具有map方法的素具结构，都可以当做涵子的实现。
```
class Functor{
    constructor(val){
        this.val=val;
    }
    map(f){
        return new Functor(f(this.val));
    }
}
```
在上述代码中，FUnctor是一个涵子，它的map方法接受函数f作为参数，最后返回一个新的涵子，里面包含的值是被f处理过的(f(this.val));
```
(new Functor(2)).map(function (a) {
    // 由函数式表面，这是Funtor的return返回执行f(this.val)
    return a + 5; // 7
});

(new Functor("mynameisKisn")).map(function (a) {
    return a.toUpperCase(); //MYNAMEISKISN
});

var res = (new Functor('bombs')).map(function (a) {
    return a.concat(' away');
}).map(function (a) {
    console.log(a); // bombs away
    return a.length;
});

console.log(res); // Functor(10),this.val = 10;
```

上面的栗子说明，函数式编程里面的运算，都是通过了涵子完成，即运算不直接针对值，而是针对这个值的容器-涵子，涵子本身具有对外接口（map方法），各种函数就是运算符，通过接口接入容器， 引发容器值的变形。

> 其实后面还有一些别的涵子介绍，但是我就不对这个方面继续整理了，整体对能力、数据分析、逻辑要求都比较高，我比较菜就不深入学习，主要为了了解基础原理以及基础逻辑即可，后期等我学习完VUE，我会继续深入学习。