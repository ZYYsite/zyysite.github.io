---
layout: post
title:  "Python的值类型和引用类型"
categories: Python
tags:  python 基础知识 找工作必备 
author: zyy
---

* content
{:toc}


## 前言

Python的变量是没有类型的，但Python却是区分类型的，那类型在哪里呢？事实是，类型是跟着内存中的对象走的。**类型属于对象，变量没有类型。**

##  值类型和引用类型

Python中的变量都是指针，这与强类型语言是有所不同的。**因为变量是指针，所以所有的变量无类型限制，可以指向任意对象。指针的内存空间大小是与类型无关的，其内存空间只是保存了所指向数据的内存地址。**

Python中的对象 = 确定内存空间 + 存储在这块内存空间中的值。

对象分为两类：一类是可修改的，一类是不可修改的。我的理解是把可修改(mutable)的类型叫做引用类型，不可修改(immutable)类型叫做值类型。




**可变数据类型(引用类型)：当该数据类型对应的变量的值发生了变化时，如果它对应的内存地址不发生改变，那么这个数据类型就是 可变数据类型。**

**不可变数据类型(值类型)：当该数据类型对应的变量的值发生了变化时，如果它对应的内存地址发生了改变，那么这个数据类型就是 不可变数据类型。**

不可变(immutable)对象类型包括：**int、float、tuple、**decimal、complex、bool、str、range、frozenset、bytes。

可变(mutable)对象类型包括：**list、dict、set、**bytearray、user-defined classes (unless specifically made immutable)。

## 1. 值类型

在Python中，数值（整型，浮点型），布尔型，字符串，元组属于值类型，本身不允许被修改（不可变类型），数值的修改实际上是让变量指向了一个新的对象（新创建的对象），所以不会发生共享内存问题。 这种方式同Java的不可变对象（String）实现方式相同，原始对象被Python的GC回收。


修改值类型的值，只是让它指向一个新的内存地址，并不会改变变量的值。
类似于Java的字符串常量池。Python在底层做了一定的优化，对于使用过的小整数以及短字符串都会被缓存起来。之所以采用这种优化的方式，是因为python中数字和字符串一经创建都是不可修改的。所以不会出现，因使用了缓存的对象值造成“脏读”的问题。
 
 对不可变数据类型中的int类型的操作，**id()**查看的是当前变量的地址值。当一个变量没有引用指向它时，该对象占用的内存会被回收。
 
不可变数据类型里的不可变可以理解为x引用的地址处的值是不能被改变的，也就是31106520地址处的值在没被垃圾回收之前一直都是1，不能改变，如果要把x赋值为2，那么只能将x引用的地址从31106520变为31106508，相当于x=2这个赋值又创建了一个对象，即2这个对象，所以int这个数据类型是不可变的，如果想对int类型的变量再次赋值，在内存中相当于又创建了一个新的对象，而不再是之前的对象。

不可变数据类型的优点就是内存中不管有多少个引用，相同的对象只占用了一块内存（对于tuple来说不是），但是它的缺点就是当需要对变量进行运算从而改变变量引用的对象的值时，由于是不可变的数据类型，所以必须创建新的对象，这样就会使得一次次的改变创建了一个个新的对象，不过不再使用的内存会被垃圾回收器回收。

## 2. 引用类型

在Python中，列表，集合，字典是引用类型，本身允许修改（可变类型）。用户自定义的类也属于可变对象类型（除非特别声明）。引用类型中的所有元素都是对应对象的地址。

进行两次a = [1, 2, 3]操作，两次a引用的地址值是不同的，也就是说其实创建了两个不同的对象，这一点明显不同于不可变数据类型，所以对于可变数据类型来说，具有同样值的对象是不同的对象，即在内存中保存了多个同样值的对象，地址值不同。

对a进行的操作不会改变a引用的地址值，只是在地址后面又扩充了新的地址，改变了地址里面存放的值，所以可变数据类型的意思就是说对一个变量进行操作时，其值是可变的，值的变化并不会引起新建对象，即地址是不会变的，只是地址中的内容变化了或者地址得到了扩充。

可变数据类型是允许同一对象的内容，即值可以变化，但是地址是不会变化的。但是需要注意一点，对可变数据类型的操作不能是直接进行新的赋值操作，比如说a = [1, 2, 3, 4, 5, 6, 7]，这样的操作就不是改变值了，而是新建了一个新的对象，这里的可变只是对于类似于append、+=等这种操作。


## 3. 不可变的例外：tuple

并非所有的不可变对象都是不可变的。
如前所述，Python容器比如元组，是不可变的。这意味着一个tuple的值在创建后无法更改。但是元组的“值”实际上是一系列名称，它们与对象的绑定是不可改变的。关键点是要注意绑定是不可改变的，而不是它们绑定的对象。

让我们考虑一个元组t =（'holberton'，[1，2，3]）
上面的元组t包含不同数据类型的元素，第一个元素是一个不可变的字符串，第二个元素是一个可变列表。元组本身不可变。即它没有任何改变其内容的方法。同样，字符串是不可变的，因为字符串没有任何可变方法。但是列表对象确实有可变方法，所以可以改变它。这是一个微妙的点，但是非常重要：不可变对象的“值” 不能改变，但它的组成对象是能做到改变的。

其实主要原因是元组内保存的是变量（也就是内存地址）。所以当变量指向对象发生变化时，如果导致变量发生变化（即不可变类型），此时元组保证该不可变类型不能修改。而如果当变量指向对象发生变化时，如果不会导致变量发生变化（即可变类型），此时元组中存储的该可变类型可以修改（因为变量本身并无变化）。


定义两个一模一样的元组，他们的地址是不同的！**因此对于tuple来说，值相同的对象的重复定义的地址不同！**

注意：可变类型和不可变类型的判断标准是，**其中的元素能否发生改变，而不是说相同的对象，其地址是否相同。**对于含有可变类型元素的tuple，虽然其中的可变类型元素可以进行修改，但是也仅限于不会改变其地址的修改。元素地址不改变，tuple的绑定关系就没有发生变化（这就是tuple是不可变类型的原因）。

## 4. 总结

**Python中对象类型的可变与不可变说的是，对象创建后，其值能否发生改变。**

不可变数据类型，不允许变量的值发生变化，如果改变了变量的值，相当于是新建了一个对象，而对于相同的值的对象，在内存中则只有一个对象，内部会有一个引用计数来记录有多少个变量引用这个对象；

可变数据类型，允许变量的值发生变化，即如果对变量进行append、+=等这种操作后，只是改变了变量的值，而不会新建一个对象，变量引用的对象的地址也不会变化，不过对于相同的值的不同对象，在内存中则会存在不同的对象，即每个对象都有自己的地址，相当于内存中对于同值的对象保存了多份，这里不存在引用计数，是实实在在的对象。




## 引用：


- [Python学习系列之值类型与引用类型](https://blog.csdn.net/answer3lin/article/details/86430074)






