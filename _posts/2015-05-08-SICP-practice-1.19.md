---
layout: post
title: "SICP习题1.19"
date: 2015-05-08 22:31
categories: vince
---

**以对数阶求出斐波那契数的巧妙算法！**

如SICP上所言，普通迭代过程fib-iter中，状态变量a和b到变换规则a <- a+b和 b <- a，称该变换为T变换。  
从1，0开始将T反复应用n次，即将T^n （T的n次方）应用与对偶（1，0）得到斐波那契数。  
现将T看作时变换族[T-pq]中p=0和q=1的特殊情况，其中[T-pq]是对于对偶（a，b）按照  
**a <- b * q + a * q + a * p** 和 **b <- b * p + a * q** 规则的变换。  
现欲找到p‘和q’，使得一次[T-p‘q’]变换等价于连续两次[T-pq]变换。

---

解法只涉及到最基本的数学

*  用a0，b0表示出a1，b1  
*  用a1，b1表示出a2，b2
*  最后在a2，b2的表达式中代入a1，b1，即可得到用a0，b0表示的a2，b2.
   整理得到  
   **q‘ = q^2 + 2 * p * q**  
   **p’ = q^2 + p^2**  

因此，只需再根据上公式额外定义过程(new-p p q)和(new-q p q)以获取p‘和q’。此处略去。  
最终过程如下

{% highlight lisp %}
(define (fib n)
    (fib-iter 1 0 0 1 n))
(define (fib-iter a b p q count)
    (cond ((= count 0) b)
          ((even? count)
           (fib-iter a b (new-p p q) (new-q p q) (/ count 2)))
          (else (fib-iter (+ (* b q) (* a q) (* a p))
                          (+ (* b p) (* a q))
                          p
                          q
                          (- count 1)))))
{% endhighlight %}

---

**lisp非常有趣！**
