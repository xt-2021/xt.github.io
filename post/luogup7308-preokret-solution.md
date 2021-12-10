---
title: 'luoguP7308 Preokret Solution'
date: 2021-09-29 17:50:38
tags: [题解]
published: true
hideInList: false
feature: /post-images/luogup7308-preokret-solution.png
---
posted on 2021-09-29 17:50:38 | under 题解 |
[题目传送门](https://www.luogu.com.cn/problem/P7308)

---
### ${\color{SkyBlue}Analysis}$ 
我们需要得到**两个问题**的答案：

1. 上半场总共得了多少分？（整场比赛持续 $4 \times 12$ 分钟。）
2. 发生多少次反超？（一次反超定义为一支队伍在得分**小于对方**后，经过投球使得**得分超过对方**。）

---
### ${\color{SkyBlue}Ideas}$
（1）我们首先来解决第一个问题：
由于整场比赛持续 $4 \times 12$ 分钟，所以上半场比赛共有 $2*12*60=1440(s)$ 。那么只要 $t_i<=1440$ ，就要增加 $ans$ 。

（2）我们接着解决第二个问题：
**我们可以先计算每一个时刻的分数**。如果有队伍在之前的分数是小于对方，现在的分数是大于对方，那么这就是反超！~~（其实真的很简单！！）~~
但是，我们需要注意这种情况：

|A队分数|B队分数|
| :----------: | :----------: |
| 3 | 4 |
| 4 | 4 |
| 4 | 5 |

这是不算反超的！！（因为B队一直都是领先A队的。）

---
### ${\color{SkyBlue}Code}$
```cpp
#include<bits/stdc++.h>
using namespace std;
long long n,m,ans=0,anss=0,ld;//ld是记录当前谁领先（a=>1,b=>2)
int a[3000],b[3000],jsa[3000],jsb[3000];
int main()
{
	scanf("%lld",&n);
	for(int i=1; i<=n; i++)
	{
		scanf("%d",&a[i]);
		if(a[i]<=1440) ans++;//在上半场得分
		jsa[a[i]]=1;
	}
	scanf("%lld",&m);
	for(int i=1; i<=m; i++)
	{
		scanf("%d",&b[i]);
		if(b[i]<=1440) ans++;//在上半场得分
		jsb[b[i]]=1;
	}
	printf("%lld\n",ans);

	for(int i=1; i<=3000; i++)
		jsa[i]+=jsa[i-1],jsb[i]+=jsb[i-1];//记录每一个时刻的两队分数

	for(int i=1; i<=3000; i++)
	{
		if(jsa[i]>jsb[i] && jsa[i-1]==jsb[i-1] && ld==2) //A队反超B队，且在之前是B队领先
			anss++;
		if(jsa[i]<jsb[i] && jsa[i-1]==jsb[i-1] && ld==1) //B队反超A队，且在之前是A队领先
			anss++;
		if(jsa[i]!=jsb[i]) //更新领先队伍
		{
			if(jsa[i]>jsb[i]) ld=1; //A队领先
			else ld=2; //B队领先
		}
	}
	printf("%lld",anss);
	return 0;
}
```

---
说明：By Xin。本人乃2016级小学生，思路比较简单，题解如有写的不好的地方请指正。