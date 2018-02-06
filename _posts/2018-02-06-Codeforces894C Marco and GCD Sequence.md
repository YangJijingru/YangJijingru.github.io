---
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P31.jpg"
preview-img: "/img/preview/P71.jpg"
---
标签：模拟，数学

# 题目

[题目传送门](http://codeforces.com/problemset/problem/894/C)

In a dream Marco met an elderly man with a pair of black glasses. The man told him the key to immortality and then disappeared with the wind of time.

When he woke up, he only remembered that the key was a sequence of positive integers of some length n, but forgot the exact sequence. Let the elements of the sequence be a1, a2, ..., an. He remembered that he calculated gcd(ai, ai + 1, ..., aj) for every 1 ≤ i ≤ j ≤ n and put it into a set S. gcd here means the greatest common divisor.

Note that even if a number is put into the set S twice or more, it only appears once in the set.

Now Marco gives you the set S and asks you to help him figure out the initial sequence. If there are many solutions, print any of them. It is also possible that there are no sequences that produce the set S, in this case print -1.
Input

The first line contains a single integer m (1 ≤ m ≤ 1000) — the size of the set S.

The second line contains m integers s1, s2, ..., sm (1 ≤ si ≤ 106) — the elements of the set S. It's guaranteed that the elements of the set are given in strictly increasing order, that means s1 < s2 < ... < sm.
Output

If there is no solution, print a single line containing -1.

Otherwise, in the first line print a single integer n denoting the length of the sequence, n should not exceed 4000.

In the second line print n integers a1, a2, ..., an (1 ≤ ai ≤ 106) — the sequence.

We can show that if a solution exists, then there is a solution with n not exceeding 4000 and ai not exceeding 106.

If there are multiple solutions, print any of them.
Examples
Input

4
2 4 6 12

Output

3
4 6 12

Input

2
2 3

Output

-1

Note

In the first example 2 = gcd(4, 6), the other elements from the set appear in the sequence, and we can show that there are no values different from 2, 4, 6 and 12 among gcd(ai, ai + 1, ..., aj) for every 1 ≤ i ≤ j ≤ n.

# 题意

给定序列A的任意区间的gcd，许多gcd组成集合S，求还原任意一种原序列

# 分析

典型的CF题目啊，乱搞

判断下最小的$ S_0 $是否能够被其他S整除，如果不能那么就直接输出-1，无解

否则将$ S_0$插空输出就好了，道理自己意会一下，有点考智商


# code

```
#include<cstdio>
#include<iostream>
#include<cstdlib>
#include<cmath>
#include<cstring>
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
const int maxn=1006;
int a[maxn],n;

int main()
{
	n=read();
	rep(i,1,n)a[i]=read();
	rep(i,2,n)
	    if(a[i]%a[1]){printf("-1\n");return 0;}
	printf("%d\n",2*n-1);
	rep(i,1,n-1)
	    printf("%d %d ",a[i],a[1]);
	printf("%d\n",a[n]);
	return 0;
}
```