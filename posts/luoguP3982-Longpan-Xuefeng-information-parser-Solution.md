---
title: 'P3982  龙盘雪峰信息解析器 Solution'
date: 2021-12-03 14:11:53
tags: [题解]
published: true
hideInList: false
feature: 
---
首先，我们来分析一下每一个规则：

1. 若【前三个字符】为 $101$ 时表示需要转换为字母 $A-Z$ ，字母 $A$ 代码为  $10100000$ ，字母 $C$ 为 $10100010$ ， $26$ 个大写字母以字母表顺序按照这种规律顺序排列，分别对应一个**二进制代码**。

对于第 $1$ 个规则，该单元是用**第 $4$ 位到第 $8$ 位**来表示这一个字母的二进制代码（ $00000-11010$ 分别代表 $A-Z$）。那么，重点来了： **如果第 $4$ 位到第 $8$ 位所表示的 $2$ 进制数比 $11010$ 大，那么就是“Error”**。

2. 若【前三个字符】为 $111$ ，则该单元翻译为空格。

第 $2$ 个规则，非常好判断。

3. 若【第一个字符】为 $0$ ，则该单元表示一个数，待定与 下一个单元所表示的数做加法。加法过程中，这两个单元应转换为十进制，然后除以 $2$ 取整再相加，加法结束后，这两个单元做加法得到的结果即为这两个单元的翻译结果（翻译结果用十进制表示，这两个单元**就都翻译完毕了**）。

对于第 $3$ 个规则，如果出现加法单元，那么它必定是**成对出现**的，如果没有满足条件，那么就是“Error”。

---
因为我们可能在判断某一个单元的途中发现“Error”，所以我们需要一个字符串 $ ans $ 来储存答案。

---
---
## ${\color{SkyBlue}General}$  ${\color{SkyBlue}Ideas}$
**重点是判断“Error”的情况。**

那么如何判断是否“Error”呢？如下：

- Error-1. 某个单元不完整。
- Error-2. 代码中出现别的奇奇怪怪的字符。
- Error-3. 加法单元没有成对出现。
- Error-4. 字母单元所表示的字符不在 $A-Z$ 范围内。

---
---
## ${\color{SkyBlue}Code}$
```cpp
#include<bits/stdc++.h>
using namespace std;
string s,ans; 
int a,b;
bool p=false;
inline string zs(int x)
{
	string s="\0";
	if(x==0) return "0";
	while(x>0) s+=x%10+48,x/=10;
	reverse(s.begin(),s.end());//用 reverse 让 s 倒序 
	return s;
}
int main()
{
	#error\
	温馨提示：禁止抄题解
	cin>>s;
	if(s.size()%8!=0) 
		{printf("Error");return 0;}//Error-1
	while(s.size()>0)
	{
		string st=s.substr(0,8);//提取出一个单元 
		for(int i=0; i<8; i++)
			if(s[i]!='1' && s[i]!='0')//Error-2
				{printf("Error");return 0;}
		if(s[0]=='1' && p==true) //Error-3
			{printf("Error");return 0;}
		if(s[0]=='1' && s[1]=='1' && s[2]=='1') ans+=" ";//空格 
		if(s[0]=='1' && s[1]=='0' && s[2]=='1')//字母 
		{
			int wq=1,sum=65;
			for(int i=8-1; i>=3; i--)
				sum+=(s[i]-'0')*wq,wq=wq+wq;
			if(sum>90) {printf("Error");return 0;}//Error-4
			ans+=char(sum);
		}
		if(s[0]=='0')//数字
		{
			int wq=1,sum=0;
			for(int i=8-1; i>=0; i--)//把这个单元转换成数字 
				sum+=(s[i]-'0')*wq,wq=wq+wq;
			if(p==true)//它是一组加法单元中的第 2 个 
			{
				b=sum;
				a/=2,b/=2;
				ans+=zs(a+b);
				a=0,b=0;
			}
			else//它是一组加法单元中的第 1 个 
				a=sum;
			p=1-p;
		}
		s.erase(0,8);//删除一个单元 
	} 
	if(p==true) {printf("Error");return 0;}//最后记得特判！ 
	cout<<ans;
 	return 0;
}
```
---
说明：By Xin。本人乃2016级小学生，思路比较简单，题解如有写的不好的地方请指正。