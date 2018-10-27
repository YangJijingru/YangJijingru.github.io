---
subtitle: "数论水题"
tags: 
 - 数论-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P78.jpg"
preview-img: "/img/preview/P78.jpg"
---
标签：数论

# 题目

给定 m 个不同的正整数 a1, a2, ..., am，请对 0 到 m 每一个 k 计算，在区间 [1, n] 里有多少正整数是 a 中恰好 k 个数的约数。

对于 $$100%$$ 的数据，$$m ≤ 200, n,ai ≤ 10^9$$。

# 分析

30pts暴力随便写，时间复杂度$$O(nm)$$

100pts正解

把m个数约数求出来，然后排序去重操作，只有这些约数才会对答案产生贡献，将这些约数求出答案

当$$k=0$$时，$$ans[k]=n-\sum_{i=1}^m ans[i]$$

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
//**********head by yjjr**********
const int maxn=1e6+6;
int n,m,a[maxn],num=0,b[maxn],f[maxn];
void insert(int x){rep(i,1,x/i)if(x%i==0)b[++num]=i,b[++num]=x/i;}
inline int cal(int d){
	int t=0;
	rep(i,1,m)if(a[i]%d==0)t++;
	return t;
}
int main()
{
	freopen("div.in","r",stdin);
	freopen("div.out","w",stdout);
	n=read(),m=read();
	rep(i,1,m)a[i]=read(),insert(a[i]);
	sort(b+1,b+1+num);
	rep(i,1,num)if(b[i]<=n&&b[i]!=b[i-1])f[cal(b[i])]++;
	f[0]=n;
	rep(i,1,m)f[0]-=f[i];
	rep(i,0,m)cout<<f[i]<<endl;
	return 0;
}
```
