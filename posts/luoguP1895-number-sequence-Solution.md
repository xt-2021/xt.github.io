---
title: 'P1895 数字序列 Solution'
date: 2021-12-03 14:13:18
tags: [题解]
published: true
hideInList: false
feature: 
---
思路比较简单，题解如有写的不好的地方请指正。写这篇题解是为了巩固一下，因为查错查了很久。
<!-- more -->

我们可以把 “$112123123412345123456……n$” 分解成：

$1$ $12$ $123$ $1234$ $12345$ $123456$ $......$ $123$ $……$ $n$

那我们首先想到的是**对每一个子序列的长度进行求解**：

经过实践，我们可以发现一个规律：
**当这个数是 $n$ 位数时：$len_i=len_{i-1}+n$**

这个预处理是非常简单的，代码如下：

```cpp
for(int j=1; j<=9; j++) 
	len[j]=len[j-1]+1;
for(int j=10; j<=99; j++) 
	len[j]=len[j-1]+2;
for(int j=100; j<=999; j++) 
	len[j]=len[j-1]+3;
for(int j=1000; j<=9999; j++) 
	len[j]=len[j-1]+4;
for(int j=10000; j<=99999; j++)
	len[j]=len[j-1]+5;
```


------------


### 总体思路：
---
1. 判断 $n$ 处于哪一个子序列中（假设在第 $i$ 个子序列中）。
2. 计算 $n$ 是第 $i$ 个子序列的第几个字符。
3. 每次给第 $i$ 个子序列添加一个数（原来为空），如果该子序列的长度已经达到了 $n$ ，那么就输出该子序列的第 $n$ 个字符。

---
### 完整代码：
---
```cpp 
#include<bits/stdc++.h>
using namespace std;
long long t,n,sum=0,i=0;
int len[100010];
string s;
inline string zs(int a)//把一个 int 数值转换成 string 类 
{
	string s="\0";
	while(a>0)
	{
		s+=a%10+48;
		a/=10;
	}
	for(int i=0; i<s.size()/2; i++)
		swap(s[i],s[s.size()-1-i]);
	return s;
}
int main()
{
	cin>>t;
	//------预处理------// 
	for(int j=1; j<=9; j++)
		len[j]=len[j-1]+1;
	for(int j=10; j<=99; j++) 
		len[j]=len[j-1]+2;
	for(int j=100; j<=999; j++)
		len[j]=len[j-1]+3;
	for(int j=1000; j<=9999; j++) 
		len[j]=len[j-1]+4;
	for(int j=10000; j<=99999; j++) 
		len[j]=len[j-1]+5;
		
	for(int g=1; g<=t; g++)
	{
		cin>>n;
		s="",sum=0,i=0;//记得把要重复使用的变量清零 
		while(sum<n) 
        		sum+=len[++i];//Step1 
		n-=sum-len[i];//Step2
		i=1;
		while(1)//Step3
		{
			string s2=zs(i);
			s=s+s2;
			if(s.size()>=n)
			{
				cout<<s[n-1]<<"\n";
				break;
			}
			i++;
		}	
	}

	return 0;
}
```