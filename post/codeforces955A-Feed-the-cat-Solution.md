---
title: 'codeforces955A Feed the cat Solution'
date: 2021-12-03 14:09:37
tags: [题解]
published: true
hideInList: false
feature: 
---

[可能更好的食用体验](https://www.luogu.com.cn/blog/523641/cf955a-feed-the-cat-ti-xie)

[题目传送门](https://www.luogu.com.cn/problem/CF955A)

------------
这题很简单，直接上代码

```cpp

#pragma G++ optimize(2)
#include<bits/stdc++.h>
using namespace std;
double hh,mm,h,d,c,n;
int main()
{
	ios::sync_with_stdio(false);
	cin>>hh>>mm>>h>>d>>c>>n;
	if(hh>=20)//如果已经过了20:00，直接以折扣后的价格购买 
	{
		double p=ceil(h/n)*c*0.8;//计算折扣后的价格，其中ceil()函数表示向上取整 
		printf("%.4lf",p);//使用printf保留4位小数输出 
	}
	else //还没过20:00时，分情况计算 
	{
		double p1=ceil(h/n)*c;//计算醒来后直接去买面包的价钱
		h+=((20-hh)*60-mm)*d;//如果等到20:00再去买面包，增加猫的饥饿值
		double p2=ceil(h/n)*c*0.8;//计算折扣后的价格
		printf("%.4lf",min(p1,p2));//输出两个价格之间的较小值 
	}
	return 0;
}

```
唯一要注意的是翻译中的一句话：
>“因此从 20:00 开始，面包有 $20$ 的特别折扣”

其中有 $20$ 的特别折扣指价格降低$20$%，即将价格乘$0.8$

${\color{#FFFFFF}\colorbox{#AD8800}{题解千万条，理解第一条。直接粘题解，棕名两行泪。} }$