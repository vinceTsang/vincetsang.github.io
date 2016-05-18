---
layout: post
title: "理解复杂的数组声明"
date: 2016-05-18 00:03
categories: vince
---

由于数组的维度是紧跟着被声明的名字的，因此阅读数组声明应该由内而外。同在外部的修饰符则从右往左阅读。

{% highlight cpp %}
int arr[10];

//含10个整形指针的数组
int *ptrs[10];  

// Parray是一个指针，指向一个含有10个整数的数组
int (*Parray)[10] = &arr;

// arrRef引用一个含有10个整数的数组
int (&arrRef)[10] = arr;

// array作为一个引用，引用了一个数组，数组的元素是int指针
int *(&array)[10] = ptrs;

{% endhighlight %}

对于函数调用传递数组参数时，如果要传递多维数组，如二维数组(数组的数组)，将形参声明为指向数组的指针 

**int (*matrix)[10]** 

考虑到编译器总是忽略掉数组形参的第一个维度，上面的声明等价于 

**int matrix[][10]** ，这看起来是一个二维数组，实际仍是一个指向含有10个整数的数组的指针。

---

from Cpp Primer 5th

