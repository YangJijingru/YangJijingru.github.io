---
tags: 
 - 数论-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P5.jpg"
preview-img: "/img/preview/P45.jpg"
---
标签：数学，找规律

# 题目

[题目传送门](http://codeforces.com/contest/899/problem/D)

There are n shovels in Polycarp's shop. The i-th shovel costs i burles, that is, the first shovel costs 1 burle, the second shovel costs 2 burles, the third shovel costs 3 burles, and so on. Polycarps wants to sell shovels in pairs.

Visitors are more likely to buy a pair of shovels if their total cost ends with several 9s. Because of this, Polycarp wants to choose a pair of shovels to sell in such a way that the sum of their costs ends with maximum possible number of nines. For example, if he chooses shovels with costs 12345 and 37454, their total cost is 49799, it ends with two nines.

You are to compute the number of pairs of shovels such that their total cost ends with maximum possible number of nines. Two pairs are considered different if there is a shovel presented in one pair, but not in the other.
Input

The first line contains a single integer n (2 ≤ n ≤ 109) — the number of shovels in Polycarp's shop.
Output

Print the number of pairs of shovels such that their total cost ends with maximum possible number of nines.

Note that it is possible that the largest number of 9s at the end is 0, then you should count all such ways.

It is guaranteed that for every n ≤ 109 the answer doesn't exceed 2·109.
Examples
Input

7

Output

3

Input

14

Output

9

Input

50

Output

1

Note

In the first example the maximum possible number of nines at the end is one. Polycarp cah choose the following pairs of shovels for that purpose:

    2 and 7;
    3 and 6;
    4 and 5. 

In the second example the maximum number of nines at the end of total cost of two shovels is one. The following pairs of shovels suit Polycarp:

    1 and 8;
    2 and 7;
    3 and 6;
    4 and 5;
    5 and 14;
    6 and 13;
    7 and 12;
    8 and 11;
    9 and 10. 

In the third example it is necessary to choose shovels 49 and 50, because the sum of their cost is 99, that means that the total number of nines is equal to two, which is maximum possible for n = 50.

# 题意

询问1->n中有多少对数字的和x+y末尾9的个数可以取到范围内最大值

# 分析

打表找规律，发现9s一共有9种情况，然后分每种情况讨论

代码真的很丑

这题我竟然在CF结束后5秒内调好了，如果交上去了可以涨不少rating啊qwq

# code

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
using namespace std;
inline int read()
{
	int f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
int n,ans=0,t;

int main()
{
	n=read();
	if(n==2)ans=1;
	if(n==3)ans=3;
	if(n==4)ans=6; 
	if(n>=5&&n<=49){t=(n-4)/10;rep(i,1,t)ans+=(i*10-1);rep(i,5+t*10,n){ans+=(t+1);if(i%10==9)ans--;}}
	if(n>=50&&n<=499){t=(n-49)/100;rep(i,1,t)ans+=(i*100-1);rep(i,50+t*100,n){ans+=(t+1);if(i%100==99)ans--;}}
	if(n>=500&&n<=4999){t=(n-499)/1000;rep(i,1,t)ans+=(i*1000-1);rep(i,500+t*1000,n){ans+=(t+1);if(i%1000==999)ans--;}}
	if(n>=5000&&n<=49999){t=(n-4999)/10000;rep(i,1,t)ans+=(i*10000-1);rep(i,5000+t*10000,n){ans+=(t+1);if(i%10000==9999)ans--;}}
	if(n>=50000&&n<=499999){t=(n-49999)/100000;rep(i,1,t)ans+=(i*100000-1);rep(i,50000+t*100000,n){ans+=(t+1);if(i%100000==99999)ans--;}}
	if(n>=500000&&n<=4999999){t=(n-499999)/1000000;rep(i,1,t)ans+=(i*1000000-1);rep(i,500000+t*1000000,n){ans+=(t+1);if(i%1000000==999999)ans--;}}
	if(n>=5000000&&n<=49999999){t=(n-4999999)/10000000;rep(i,1,t)ans+=(i*10000000-1);rep(i,5000000+t*10000000,n){ans+=(t+1);if(i%10000000==9999999)ans--;}}
	if(n>=50000000&&n<=499999999){t=(n-49999999)/100000000;rep(i,1,t)ans+=(i*100000000-1);rep(i,50000000+t*100000000,n){ans+=(t+1);if(i%100000000==99999999)ans--;}}
	if(n>=500000000&&n<=4999999999){t=(n-499999999)/1000000000;rep(i,1,t)ans+=(i*1000000000-1);rep(i,500000000+t*1000000000,n){ans+=(t+1);if(i%1000000000==999999999)ans--;}}
	cout<<ans<<endl;return 0;
} 
		
```

