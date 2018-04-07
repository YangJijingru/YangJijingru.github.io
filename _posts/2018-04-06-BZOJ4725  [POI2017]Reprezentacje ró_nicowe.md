---
subtitle: "智商题"
tags: 
 - 基础算法-二分
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P88.jpg"
preview-img: "/img/preview/P88.jpg"
---
标签：二分

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=4725)

## Description
给定一个数列a：
当n<=2时,a[n]=n
当n>2，且n是奇数时，a[n]=2a[n-1]
当n>2，且n是偶数时，a[n]=a[n-1]+r[n-1]
其中r[n-1]=mex(|a[i]-a[j]|)(1<=i<=j<=n-1)，mex{S}表示最小的不在S集合里面的非负整数。
数列a的前若干项依次为：1,2,4,8,16,21,42,51,102,112,224,235,470,486,972,990,1980。
可以证明，对于任意正整数x，只存在唯一一对整数(p,q)满足x=a[p]-a[q]，定义为repr(x)。
比如repr(17)=(6,3)，repr(18)=(16,15)。
现有n个询问，每次给定一个正整数x，请求出repr(x)。
## Input
第一行包含一个正整数n(1<=n<=10^5)。
接下来n行，每行一个正整数x(1<=x<=10^9)，表示一个询问。
## Output
输出n行，每行两个正整数p,q，依次回答每个询问。
## Sample Input
```
2
17
18
```
## Sample Output
```
6 3
16 15
```
# 分析

智商题

发现>1e9的询问都无需计算处理，肯定是前后二者的差值

因此只需要暴力算出前面一些位置的a数组
 
后面全部可以二分乱搞求出来

# code
```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<map> 
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
//**********head by yjjr**********
#define mp make_pair
#define fi first
#define se second
const int maxn=1e6+6;
map<int,pair<int,int> >s;
typedef map<int ,pair<int ,int > >::iterator it;
int q,n=3,a[maxn],b[maxn],cnt;
int main()
{
	a[1]=1,a[2]=2;
	s[1]=mp(2,1);
	while(1){
		if(n&1)a[n]=a[n-1]*2;
		else for(int j=1;;j++)if(!s.count(j)){a[n]=a[n-1]+j;break;}
		rep(j,1,n-1)s[a[n]-a[j]]=mp(n,j);
		if((!(n&1))&&a[n]>1e9)break;n++;
	}
	for(it l=s.begin();l!=s.end();l++)b[++cnt]=l->first;
	q=read();
	while(q--){
		int x=read();
		it l=s.find(x);
		if(l!=s.end())printf("%d %d\n",l->se.fi,l->se.se);
		else{
			int y=lower_bound(b+1,b+1+cnt,x)-b-1;
			printf("%d %d\n",n+(x-y)*2,n+(x-y)*2-1);
		}
	}
	return 0;
}
```