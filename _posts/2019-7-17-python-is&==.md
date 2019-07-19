---
layout: post
title:  "Python中的'is'和'=='的异同"
categories: Python
tags:  python 基础知识 找工作必备
author: zyy
---

* content
{:toc}

本文将简要介绍Python中的'is'和'=='的异同。

## 重点

Python中的对象包含三个基本要素，分别是：**id**(身份标识，也就是对象的地址)、**type**(数据类型)和**value**(值)。

'is'和'=='都是**对对象进行比较判断**作用的，但对对象比较判断的**内容并不相同**。




'=='是python标准操作符中的**比较操作符**，用来比较判断两个对象的**value(值)是否相等**。

'is'也被叫做**同一性运算符**，这个运算符比较判断的是**对象间的唯一身份标识，也就是id是否相同，即是否是指向同一地址。**

如果是int，float，bool，str等**不可变类型**，他们的值相同的对象一般存储在**同一个地址**中，因此'is'和'=='的判断结果**一般是相同的**。

如果是list，set，dict或自定义对象等**可变类型**，他们的值相同的对象可能存储在**不同的地址中**，因此'is'和'=='的结果**可能是不同的**。


这里就需要对python的拷贝机制以及变量、对象组成有一定的了解。

```js
a = 1
b = 1
c = copy.copy(a)
d = copy.deepcopy(a)
a is b               输出True
a == b               输出True
c is a               输出True
c == a               输出True
d is a               输出True
d == a               输出True

p = [1,2]
q = [1,2]
r = copy.copy(p)
s = copy.deepcopy(p)
p is q               输出False
p == q               输出True
r is p               输出False
r == p               输出True
s is p               输出False
s == p               输出True
```

## 还有一件事

对于tuple，定义两个相同的元组，他们所在的**地址不相同**！
注意：可变类型和不可变类型的判断标准是，**其中的元素能否发生改变，而不是说相同的对象，其地址是否相同。**对于含有可变类型元素的tuple，虽然其中的可变类型元素可以进行修改，但是也仅限于不会改变其地址的修改。元素地址不改变，tuple的绑定关系就没有发生变化（这就是tuple是不可变类型的原因）。
```js
a = (1,2,3)
b = (1,2,3)
a is b          输出False
```