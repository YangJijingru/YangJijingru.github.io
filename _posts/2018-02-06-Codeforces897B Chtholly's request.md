---
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P34.jpg"
preview-img: "/img/preview/P74.jpg"
---
标签：模拟，数学

# 题目

[题面传送门](http://codeforces.com/contest/897/problem/B)

 — Thanks a lot for today.

— I experienced so many great things.

— You gave me memories like dreams... But I have to leave now...

— One last request, can you...

— Help me solve a Codeforces problem?

— ......

— What?

Chtholly has been thinking about a problem for days:

If a number is palindrome and length of its decimal representation without leading zeros is even, we call it a zcy number. A number is palindrome means when written in decimal representation, it contains no leading zeros and reads the same forwards and backwards. For example 12321 and 1221 are palindromes and 123 and 12451 are not. Moreover, 1221 is zcy number and 12321 is not.

Given integers k and p, calculate the sum of the k smallest zcy numbers and output this sum modulo p.

Unfortunately, Willem isn't good at solving this kind of problems, so he asks you for help!
Input

The first line contains two integers k and p (1 ≤ k ≤ 105, 1 ≤ p ≤ 109).
Output

Output single integer — answer to the problem.
Examples
Input

2 100

Output

33

Input

5 30

Output

15

Note

In the first example, the smallest zcy number is 11, and the second smallest zcy number is 22.

# 题意

规定一种zcy数的定义为：回文数且位数为偶数。询问前k小的zcy数的和并取模。

# 数据范围

$1 ≤ k ≤ 10^5, 1 ≤ p ≤ 10^9$

# 分析

显然枚举前k小个数，然后翻转求回文并求和取模就可以了，比如这样：

12->1221

329->329923

# code

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
int num[100];
ll k,p,ans;
int main()
{
	k=read(),p=read();
	rep(i,1,k){
		ll t=i,s=0;
		int cnt=0;
		mem(num,0);
		while(t){
			num[++cnt]=t%10;
			t/=10;
		}
		dep(j,cnt,1)s=(s+num[j])*10;
		rep(j,1,cnt)s=(s+num[j])*10;
		s/=10;
		ans=(ans+s)%p;
		//cout<<s<<' '<<cnt<<endl;
	}
	cout<<ans<<endl;
	return 0;
}
```