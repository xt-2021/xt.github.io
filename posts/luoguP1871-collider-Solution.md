---
title: 'P1871 对撞机 Solution'
date: 2021-12-03 14:10:32
tags: [题解]
published: true
hideInList: false
feature: 
---

[可能更好的食用体验](https://www.luogu.com.cn/blog/523641/solution-p1871)

[题目传送门](https://www.luogu.com.cn/problem/P1871)

------------
## ${\color{00CD00}{样例解释}}$

```
10 10 
+ 6
+ 10
+ 5
- 10
- 5
- 6
+ 10 
+ 3
+ 6
+ 3
```
题目已经讲得很清楚了，就是每次打开的对撞机编号一定要与之前所有打开的对撞机编号互质。如第 $2$ 个询问要打开 $10$ 号对撞机，而 $10$ 与前面的 $6$ 不互质，故输出 “ Conflict with $6$ ” 。其他的也就不用我多说了。还有一点要注意，在第 $9$ 个询问中 $6$ 既与 $3$ 冲突也与 $10$ 冲突，不要被样例忽悠了，**输出任意一个都是对的**，我的程序输出就是“ Conflict with $10$ ” 。


------------

## ${\color{00CD00}{主要思路}}$

既然要使每次打开的对撞机编号与之前所有都互质，也就是要使每次打开的对撞机的编号的质因数中不包含之前出现过质因数。我们可以每次将编号进行分解质因数，如果其中的质因数在之前被标记过，则不能开启。如果其中的质因数在之前都没出现过，就开启这个对撞机，并记录这些质因数。开启的代码如下：
```cpp

bool o[100005];//记录是否被开启 
bool s[100005];//记录质数
int f[100005];//记录每个质因数属于哪个数
void prime(int r)//筛选质数
{
	s[0]=true,s[1]=true;
	for(int i=2;i<=r;i++)
		if(s[i]==false)
			for(int j=i+i;j<=r;j+=i)s[j]=true;
	return;
}
int open(int x)
{
	if(o[x])return -1;//-1表示已经开启
	int t[1000],g=0;
	for(int i=1;i<=sqrt(x);i++)//分解质因数
	{
		if(x%i==0&&!s[i])//如果i是x的质因数
		{
			if(f[i]!=0)return f[i];//如果质因数已经存在，返回其所属的数
			t[++g]=i;//否则将质因数存入t数组
		}
		if(x%i==0&&i*i!=x&&!s[x/i])//同上
		{
			if(f[x/i]!=0)return f[x/i];
			t[++g]=x/i;
		}
	}
	o[x]=true;
	for(int i=1;i<=g;i++)f[t[i]]=x;//将x的质因数标记为属于x
	return 0;//0表示可以开启
}
```
关闭也很简单，分解其质因数并删除标记即可。
```cpp

int close(int x)
{
	if(!o[x])return -2;//-2表示已经关闭
	for(int i=1;i<=sqrt(x);i++)//分解质因数
		if(x%i==0)
			f[i]=0,f[x/i]=0;//删除标记
	o[x]=false;
	return 0;//0表示可以关闭
}
```
然后就在主程序中判断函数的返回值并输出即可。

------------

## ${\color{#00CD00}{完整代码}}$

```cpp

#include<bits/stdc++.h>
using namespace std;
long long n,m,p,ans;
bool o[100005];//记录是否被开启 
bool s[100005];//记录质数 
int f[100005];//记录每个质因数属于哪个数
#error\
禁止抄题解
void prime(int r)
{
	s[0]=true,s[1]=true;
	for(int i=2;i<=r;i++)
		if(s[i]==false)
			for(int j=i+i;j<=r;j+=i)s[j]=true;
	return;
}
int open(int x)
{
	if(o[x])return -1;
	int t[1000],g=0;
	for(int i=1;i<=sqrt(x);i++)
	{
		if(x%i==0&&!s[i])
		{
			if(f[i]!=0)return f[i];
			t[++g]=i;
		}
		if(x%i==0&&i*i!=x&&!s[x/i])
		{
			if(f[x/i]!=0)return f[x/i];
			t[++g]=x/i;
		}
	}
	o[x]=true;
	for(int i=1;i<=g;i++)f[t[i]]=x;
	return 0;
}
int close(int x)
{
	if(!o[x])return -2;
	for(int i=1;i<=sqrt(x);i++)
		if(x%i==0)
			f[i]=0,f[x/i]=0;
	o[x]=false;
	return 0;
}
int main()
{
	cin>>n>>m;
	prime(n);//筛选质数
	for(int i=1;i<=m;i++)
	{
		char ch;cin>>ch>>p;
		if(ch=='+')ans=open(p);
		else ans=close(p);
		switch(ans)//根据函数返回值输出结果 
		{
			case 0:cout<<"Success";break;
			case -1:cout<<"Already on";break;
			case -2:cout<<"Already off";break;
			default:cout<<"Conflict with "<<ans;break;
		}
		cout<<endl;
	}
	return 0;
}

```

## 温馨提示
## ${\color{#FFFFFF}\colorbox{#AD8800}{题解千万条，理解第一条。直接粘题解，棕名两行泪。} }$