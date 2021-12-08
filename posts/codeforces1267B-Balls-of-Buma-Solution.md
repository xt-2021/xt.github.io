---
title: 'codeforces1267B Balls of Buma Solution'
date: 2021-12-03 14:04:48
tags: [题解]
published: true
hideInList: false
feature: 
---
[可能更差的食用体验（？）](https://www.luogu.com.cn/blog/523641/CF1267B)

[题目传送门](https://www.luogu.com.cn/problem/CF1267B)

~~作为翻译人来水一篇题解。~~

鄙人不才，参考了[@gyh20大佬](https://www.luogu.com.cn/blog/gyh20/solution-cf1267b)的思路，并加以优化与说明。

------------
### ${\color{RoyalBlue} {样例解释}}$

```txt
WWWOOOOOOWWW
```
这个样例可以在第 $3$ 到第 $9$ 任意一个字符后插入一个“O”，共 $9-3+1=7$ 种方式。

以在第 $5$ 位插入一个“O”为例：第一次中间连续一段的“O”的长度因插入而变长且长度大于等于 $3$ ，满足消除条件。当中间的“O”被消除后，前后两段连续的“B”连成一段（即长度因“O”的消除而变长）且长度大于等于 $3$ ，再次消除。此时所有字符都被消除，即为一种成功的方案。读者可以尝试模拟在其他位置插入其他字符时游戏的过程。


------------

### ${\color{RoyalBlue} {主要思路}}$
从上面的模拟不难看出，前面的字符必定只能与后面对应的字符连成一段并消除，例如：形如“AABBAAA”、“ABBCCBAA”等的字符串都是合法的，而形如“AABBAABB”、“AABBCCAABB”等的字符串都是不合法的。可以通过比较前后字符的方法从外向内进行消除。不难想到可以维护一个双向队列。

以第一个样例“BBWWBB”为例：

`deque<char>st`
|B|B|W|W|B|B|
|-|-|-|-|-|-|

第一次，队列将两头的“B”弹出，并统计出个数为 $4$。个数大于等于 $3$，程序继续进行消除操作。

此时队列为：
|W|W|
|-|-|

第二次，队列将两头的“W”弹出，并统计出个数为 $2$。此时队列为空，个数只要大于等于 $2$ 就为合法。此时可以在这一串“W”之间和前、后任意位置插入一个“W”，故答案为 $2+1=3$。

这一段消除的过程可以用以下代码实现：
```cpp
long long cnt=0; 
while(!st.empty())
{
	ch=st.front(),st.pop_front(),cnt++;
	if(st.front()!=ch)break;
}
while(!st.empty())
{
	ch=st.back(),st.pop_back(),cnt++;
	if(st.back()!=ch)break;
}
if(st.empty()&&cnt>=2)
{
	cout<<cnt+1;
	return 0;
}
if(cnt<3)break;
```
还有一点要注意：如果队列的队首和队尾字符不相同或者队列的长度只有 $1$，都是不合法的字符串。只需在每次操作前判断一下即可。
```cpp
if(st.size()==1||st.front()!=st.back())break;
```
总的时间复杂度为 $O(\ st.size()\ )$，非常优秀。

------------

### ${\color{RoyalBlue} {完整代码}}$

```cpp
#pragma G++ optimize(2)
#include<bits/stdc++.h>
#error\ 
禁止抄题解 
using namespace std;
char ch;
deque<char>st;
long long l,r; 
int main()
{
	ios::sync_with_stdio(false);
	while(cin>>ch)st.push_back(ch);
	while(!st.empty())
	{
		if(st.size()==1||st.front()!=st.back())break;
		long long cnt=0; 
		while(!st.empty())
		{
			ch=st.front(),st.pop_front(),cnt++;
			if(st.front()!=ch)break;
		}
		while(!st.empty())
		{
			ch=st.back(),st.pop_back(),cnt++;
			if(st.back()!=ch)break;
		}
		if(st.empty()&&cnt>=2)
		{
			cout<<cnt+1;
			return 0;
		}
		if(cnt<3)break;
	}
	cout<<0;
	return 0;
}
```

${\color{#FFFFFF}\colorbox{#AD8800}{题解千万条，理解第一条。直接粘题解，棕名两行泪。} }$
