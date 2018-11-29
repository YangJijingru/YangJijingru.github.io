---
subtitle: "区间求交集的转化"
tags: 
 - 数论-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P61.jpg"
preview-img: "/img/preview/P61.jpg"
---
标签：数学

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=4377)

## Description

给定$$n,a,b,p$$，其中$$n,a$$互质。定义一个长度为$$n$$的$$01$$串$$c[0..n-1]$$，其中$$c[i]==0$$当且仅当$$(ai+b) mod n < p$$。
给定一个长为$$m$$的小$$01$$串，求出小串在大串中出现了几次。

## Input

第一行包含整$$n,a,b,p,m(2<=n<=10^9,1<=p,a,b,m<n,1<=m<=10^6)$$。n和a互质。
第二行一个长度为$$m$$的$$01$$串。

## Output
一个整数，表示小串在大串中出现了几次

## Sample Input
```
9 5 6 4 3
101
```
### Sample Output
```
3
HINT
```

# 分析

因为$$a$$和$$n$$互质，所以对于$$0$$到$$n-1$$里面的每个$$i$$，$$a\times i \mod p$$都是不相同的，相当于一个全排列

设$$x$$为小串第一个数实际值，那么即可得到小串第$$i$$个数实际的值为$$(x+(i-1)\times a)\mod p$$

我们可以根据小串实际的值是$$0$$ or $$1$$，列出化简后形如$$x_1≤x\mod p≤x_2$$的$$m$$个不等式

答案只需要求出这些的交集就可以了，为了更加方便，可以将所有的区间取反，然后按左端点排序求补集大小

# code
```cpp
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
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
//******head by yjjr******
const int maxn=1e6+6;
int n,a,b,p,m,cnt=0,ans,r;char w[maxn];
struct node{int x,y;}e[maxn<<2];
inline bool cmp(node a,node b){return a.x<b.x;}
void add(int x,int y,int z,int r){
	if(x)e[++cnt]=(node){0,x};
	if(y<z)e[++cnt]=(node){y,z};
	if(r<n)e[++cnt]=(node){r,n};
}
int main(){
	n=read(),a=read(),b=read(),p=read(),m=read();cin>>w;
	rep(i,0,m-1){
		b=(b+a)%n;
		if(w[i]=='0')add(0,max(p-b,0),n-b,min(p-b+n,n));
		else add(max(p-b,0),n-b,min(p-b+n,n),n);
	}
	b=n-a;
	dep(i,n-1,n-m+1){
		b=(b-a+n)%n;
		e[++cnt]=(node){b,b+1};
	}
	e[++cnt]=(node){n,n+1};
	sort(e+1,e+1+cnt,cmp);
	rep(i,1,cnt){
		if(e[i].x>r)ans+=e[i].x-r;
		if(e[i].y>r)r=e[i].y;
	}
	cout<<ans<<endl;
	return 0;
}
```
