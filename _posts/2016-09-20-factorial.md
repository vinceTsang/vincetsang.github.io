---
layout: post
title: "用万进制优化大整数乘法"
date: 2016-09-20 14:00
categories: vince
---

**从求阶乘说起**

求阶乘数看似简单，5! = 120, 6! = 720, ... 10! = 3628800， ... 100!就已经是好几十位的大数了，计算只能通过普通的数组来模拟，每个数组存一个十进制数位

但常规的字符数组模拟相对低效，N为10000时，就需要开36000的数组，耗时2k ms+，oj哪怕再友好也是勉强过，太不能满足猿们的追求了吧。

如果拿万进制做，数组规模不到1w，耗时260 ms，猿们开心地点点头 :D

---

**万进制是什么鬼？**

每一种进制的发明都是为了一定的用途，2进制是电平开关，万进制算是为了大数而生的。

比如65536这个数，在十进制意义下，用数组存，从低位到高位分别存6,3,5,5,6，而在万进制意义下，低位存5536，高位存6，搞定。

做乘法时，如65536 * 8，则低位5536 * 8 = 44288，大于10000的进位，余数4288寸如结果，然后进4。高位继续运算6 * 8 = 48，加进位得52。

最终结果为524288。显然，如果低位的4288为288等不足4位的数，输出时应补齐为0288，即结果为520288。

---

{% highlight cpp %}
#include <iostream>
#include <memory.h>
#include <cstdio>
#include <algorithm>
#include <iomanip>
using namespace std;
const int maxn = 9000;
int f[maxn];
int main()
{
    int n;
    int i,j;
    while(cin>>n) {
        memset(f,0,sizeof(f));
		f[0]=1;
		int place=1;
		for(i=2;i<=n;i++)
		{
			int carry=0;
			for(j=0;j<place;j++)
			{
				int tmp=f[j]*i+carry;
				carry = tmp/10000;
				f[j] = tmp%10000;
			}
			if(carry>0){
                f[place]=carry;
                ++place;
			}
		}
		cout<<f[--place];
		for(int i=place-1;i>=0;i--){
            cout<<setw(4)<<setfill('0')<<f[i];
		}
		cout<<endl;
    }
    return 0;
}

{% endhighlight %}

---

**此处是万进制的一个大数结果在不断累乘一个较小的数(其实也是一个万进制数，只不过小于1万故只有一个低位元素)，当两个乘数均为大数时，参考十进制的大数乘法即可模拟出万进制的乘法。同理，亿进制一样有趣哈哈，但万进制已经极其够民用的了=，=**
