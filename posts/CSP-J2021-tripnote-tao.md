---
title: '€＄₱-J2021游记-by.Tao'
date: 2021-12-03 14:07:49
tags: [游记]
published: true
hideInList: false
feature: 
---
一名小学六年级蒟蒻第一次参加€＄₱，好紧张~

---
# 初赛

初赛不得不说真的很恶心（至少对我来说）。值得庆幸的是，我还是以压线的分数顺利（bushi）进入了复赛（吐槽：GD出分真是太慢了╮(╯▽╰)╭）。这里真的要感谢洛谷有题给了我们练习平台，真的很有用！！！同时也感谢我的教练和同学给予的支持与鼓励。

无论如何，只要过了初赛，复赛不用顾虑那么多了。

---
# 复赛

墙裂推荐《深入浅出程序设计竞赛》！！真的写的很好、很详尽！！

我本来就对复赛没报太大的信心，赛前估分 $<200$，毕竟我也才六年级嘛。不过今年的题确实比以往要简单一点，还没考动规，每道题都能或多或少水一些分。下面我会分享一下我做每道题时的思路、想法、策略、代码等（不过基本都是错误的，就当反面教材来看吧QwQ）。

声明：以下标题后的“自测”都是指洛谷估分。

### T1. 分糖果（[洛谷P7909](https://www.luogu.com.cn/problem/P7909)）$\ \ $自测100分

这是我唯一一道做对的题了。我也看了一下其他一些人的AC代码，发现我的代码好像也是最简单的了。下面我也简单的分析一些我这道题的思路：

1. 答案最大只能是 $n-1$，这是毋庸置疑的。
2. 我们可以先找到小于 $l$ 的最大的 $n$ 的倍数，即$\left\lfloor l\div n \right\rfloor\times n$。这时答案最大只能是$r-\left\lfloor l\div n \right\rfloor\times n$，超过了就拿不了了。
3. 答案就是 $n-1$ 与 $r-\left\lfloor l\div n \right\rfloor\times n$ 中的最小值。

代码非常之简单，没有奇奇怪怪的条件判断或循环什么的。
```cpp
#include<bits/stdc++.h>
using namespace std;
long long n,l,r;
int main()
{
	freopen("candy.in","r",stdin);
	freopen("candy.out","w",stdout);
	cin>>n>>l>>r;
	cout<<min(r-l/n*n,n-1);
	return 0;
}
```

### T2.插入排序（[洛谷P7910](https://www.luogu.com.cn/problem/P7910)）$\ \ $自测52分

比赛时实在想不出什么好了思路了哇哇哇。。。只好暴力水分了。暴力就没啥好说的了，自己看代码就能看出来。
```cpp
#include<bits/stdc++.h>
using namespace std;
long long n,Q,m,x,v;
int a[8005],p[8005];
struct stu{
	int s,p;
}b[8005];
void swap(stu &a,stu &b)
{stu t=a;a=b,b=t;}
inline void sort_()
{
	for(int i=1;i<=n;i++)b[i]=(stu){a[i],i};
	for(int i=1;i<=n;i++)
		for(int j=i;j>1;j--)
			if(b[j].s<b[j-1].s)swap(b[j],b[j-1]);
	for(int i=1;i<=n;i++)p[b[i].p]=i;
}
int main()
{
	freopen("sort.in","r",stdin);
	freopen("sort.out","w",stdout);
	scanf("%lld%lld",&n,&Q);
	for(int i=1;i<=n;i++)scanf("%d",&a[i]);
	sort_();
	for(;Q;Q--)
	{
		scanf("%lld",&m);
		if(m==1)
		{
			scanf("%lld%lld",&x,&v);
			a[x]=v;
			sort_();
		}
		if(m==2)
		{
			scanf("%lld",&x);
			printf("%d%c",p[x],10);
		}
	}
	return 0;
}
```
我在这题尝试了很多其他方法，耗了不少时间，虽然不会超时，但无一例外的都是答案错误。最后还是选择了暴力。

赛时估分$30$以下，自测$52$分也是令我很意外了，希望€€£的数据能更水点吧（逃）。

### T3.网络连接([洛谷P7911](https://www.luogu.com.cn/problem/P7911))$\ \ $自测65分

这题其实就是大模拟啊！但是由于我在第二题花了太多时间，以至于我第三题没有太多时间写，现在想来还有些不甘。我在这题的策略就是不考虑不符合规范的情况，这样可以省不少代码，还能水到不少分数。我的代码如下：
```cpp
#include<bits/stdc++.h>
using namespace std;
long long n;
string op,ad;
char ch;
struct stu{
	int a,b,c,d,e;
	friend bool operator< (stu s1,stu s2)
	{
		if(s1.a==s2.a)
			if(s1.b==s2.b)
				if(s1.c==s2.c)
					if(s1.d==s2.d)
						return s1.e<s2.e;
					else return s1.d<s2.d;
				else return s1.c<s2.c;
			else return s1.b<s2.b;
		else return s1.a<s2.a;
	}//这个重载写死我了
};
map<stu,int>ser;
int main()
{
	freopen("network.in","r",stdin);
	freopen("network.out","w",stdout);
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>op;
		int a,b,c,d,e;
		scanf("%d.%d.%d.%d:%d",&a,&b,&c,&d,&e);//不考虑不符合规范的情况，格式化输入就行
		if(a>255||b>255||c>255||d>255||e>65535){cout<<"ERR\n";continue;}
		stu s=(stu){a,b,c,d,e};
		if(op=="Server")
			if(ser[s]>0)cout<<"FAIL\n";
			else cout<<"OK\n",ser[s]=i;
		else
			if(ser[s]>0)cout<<ser[s]<<endl;
			else cout<<"FAIL\n";
	}
	return 0;
}
```
赛时估分其实也不高，民间数据总能带给人惊喜，不过你谷民间数据一直都不咋靠谱（逃）。

### T4.网络连接([洛谷P7912](https://www.luogu.com.cn/problem/P7912))$\ \ $自测10分

这道题请求大佬的帮助。我先来分享一下我的思路：

首先是读入。读入我使用了一个队列数组存储每一个“块”，只要每次读入到的水果与上一个不同，就存储到下一个队列中。

读入代码如下：
```cpp
struct stu
{int x,p;};//x是水果的种类，p是该水果的位置
queue<stu>a[200005];

cin>>n;a[0].front().x=2,x[0]=-1;
for(int i=1;i<=n;i++)
{
	scanf("%d",&x[i]);
	if(x[i]!=x[i-1])k++;
	a[k].push((stu){x[i],i});
}
```

接着，我循环枚举每一个队列，只要队列不空就输出队列的队头的位置，并弹出队头。直到所有队列都为空为止。
```cpp
p=0;//p记录是否有至少一个队列不空
for(int i=1;i<=k;i++)
{
	if(!a[i].empty())
	{
		printf("%d ",a[i].front().p);
		a[i].pop();p=1;
	}
}
```

如何解决两个“块”合并的问题？可以使用一个变量记录上一个不空的队列编号，只要当前队列的水果与上一个相同，就将当前队列连接到上一个队列的后面。即：每次将当前队列的队头弹出并入到上一个队列中。
```cpp
la=0;//la记录上一个不空的队列编号
for(int i=1;i<=k;i++)
	if(!a[i].empty())
	{
		if(a[i].front().x==a[la].front().x) 
			while(!a[i].empty())
				a[la].push(a[i].front()),a[i].pop();
		la=i;
	}
```

主要思路到这里就介绍完了。下面还是附上完整代码：
```cpp
#include<bits/stdc++.h>
using namespace std;
long long n,k,la;
int x[200005];
struct stu
{int x,p;};
queue<stu>a[200005];
int main()
{
	freopen("fruit.in","r",stdin);
	freopen("fruit.out","w",stdout);
	cin>>n;a[0].front().x=2,x[0]=-1;
	for(int i=1;i<=n;i++)
	{
		scanf("%d",&x[i]);
		if(x[i]!=x[i-1])k++;
		a[k].push((stu){x[i],i});
	}
	bool p=1;
	while(p)
	{
		p=0;
		for(int i=1;i<=k;i++)
		{
			if(!a[i].empty())
			{
				printf("%d ",a[i].front().p);
				a[i].pop();p=1;
			}
		}la=0;
		for(int i=1;i<=k;i++)
			if(!a[i].empty())
			{
				if(a[i].front().x==a[la].front().x) 
					while(!a[i].empty())
						a[la].push(a[i].front()),a[i].pop();
				la=i;
			}
		printf("\n");
	}
	return 0;
}
```
测评结果如下图（用小号测的，莫见怪）：

[![](https://cdn.luogu.com.cn/upload/image_hosting/o2lxb82z.png)](https://www.luogu.com.cn/record/60979581)

TLE还能理解，为什么会WA呢？请求大佬指出代码/思路的错误。


- $upd\ on\ 2021.10.27\ $：现已解决，感谢大佬 ~~（还是我太蒻了啊）~~

---


洛谷估分 $100+52+65+10=227$

小图灵 $100+36+65+10=211$

###### 好蒻啊，2=都悬 o(╥﹏╥)o

---

- $upd\ on\ 2021.11.01\ $：€££公布分数 $100+52+65+30=247$，竟然比估分都高（窃喜）！看来我还是低估民间数据了。~~话说为甚么申诉都要收费啊！！！€££硼化硫！！！~~