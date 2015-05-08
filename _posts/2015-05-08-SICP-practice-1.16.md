---
layout: post
title: "SICP习题1.16"
date: 2015-05-08 15:17
categories: vince
---

题目要求定义一个按迭代方式求幂到过程，对数阶。

先看原递归解法

{% highlight lisp %}
(define (fast-expt b n)
    (cond ((= n 0) 1)
    ((even? n) (square (fast-expt b (/ n 2))))
    (else ( * b (fast-expt b (- n 1))))))
{% endhighlight %}
该解法中第三行的square操作和第四行的*操作均是由于递归计算而延后执行，与转化为迭代计算，
则应该在递归调用前完成他们。

* 对于square操作，利用公式( b^(n/2) )^2 = ( b^2 )^(n/2)  
  即改相应语句为
{% highlight lisp %}
(fast-expt (square b) (/ n 2))
{% endhighlight %}
* 对于 * 操作，需要借助一个附加状态变量a，用于暂存那些原本被推后执行的**乘以b**操作  
  将它们累乘至a，故为过程fast-expt增加一个参数a，同时改相应语句为
{% highlight lisp %}
(fast-expt b (- n 1) ( * b a))
{% endhighlight %}

整个过程变为：
{% highlight lisp %}
(define (fast-expt b n a)
    (cond ((= n 0) a )
    ((even? n) (fast-expt (square b) (/ n 2) a))
    (else (fast-expt b (- n 1) ( * b a)))))
{% endhighlight %}

调用时a初值为1即可.**(fast-expt b n 1)**

---
原书提到的不变量。实际上就是从一个状态转移到另一个状态时，过程信息不丢失，通过临时变量保存起来，计算最终结果时结合该临时变量即可。

---

**lisp非常有趣！**


