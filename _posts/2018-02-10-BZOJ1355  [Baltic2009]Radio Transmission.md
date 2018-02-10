---
subtitle: "这题告诉你什么是KMP"
tags: 
 - 字符串-KMP
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P24.jpg"
preview-img: "/img/preview/P64.jpg"
---
标签：KMP

# 题目

[题目传送门](http://www.lydsy.com/JudgeOnline/problem.php?id=1355)

Description
给你一个字符串，它是由某个字符串不断自我连接形成的。 但是这个字符串是不确定的，现在只想知道它的最短长度是多少.
Input
第一行给出字符串的长度,1 < L ≤ 1,000,000. 第二行给出一个字符串，全由小写字母组成.
Output
输出最短的长度
Sample Input
8

cabcabca
Sample Output
3
HINT

对于样例,我们可以利用"abc"不断自我连接得到"abcabcabc",读入的cabcabca,是它的子串

# 分析

KMP的入门题

ans=n-fail[n]

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
#define reg(x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=1e6+6;
int n,fail[maxn];
char st[maxn];
int main()
{
	n=read();
	scanf("%s",st+1);
	int j=0;
	rep(i,2,n){
		while(st[j+1]!=st[i]&&j)j=fail[j];
		if(st[j+1]==st[i])j++;
		fail[i]=j;
	}
	printf("%d\n",n-fail[n]);
	return 0;
}
```


