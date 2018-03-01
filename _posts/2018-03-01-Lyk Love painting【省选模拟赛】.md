---
subtitle: "法法！"
tags: 
 - 基础算法-二分
 - DP-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P8.jpg"
preview-img: "/img/preview/P8.jpg"
---
标签：二分，DP

# 题目

![这里写图片描述](http://img.blog.csdn.net/20180301092843255?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXdlcnR5MTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

n<=1e6 m<=100

# 题意

给定2*N的网格，每个网格含有权值

将其分为M的区域

使其M个区域的最大权值和最小

# 分析

一眼二分！

然后大概要DP检验

设f[i]表示在限制为lim的情况下最少要多少块区域

s1,s2分别表示第一行第二行前缀和

s3=s1+s2，即总的前缀和

$$pre1[i]表示s1[i]-s1[pre1[i]]<=x且s1[i]-s1[pre1[i]-1]>x$$

$$pre2[i]表示s2[i]-s2[pre2[i]]<=x且s2[i]-s2[pre2[i]-1]>x$$

$$pre3[i]表示s1[i]-s1[pre1[i]]+s2[i]-s2[pre2[i]]<=x且s1[i]-s1[pre1[i]-1]+s2[i]-s2[pre2[i]-1]>x$$

$$f[i]=min(f[pre3[i]],f[findpre1(i)]+Step,f[findpre2(i)]+Step)$$

每次在转移的时候选取位置靠后的findpre(1),findpre(2)，如图所示

![这里写图片描述](http://img.blog.csdn.net/20180301094921611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXdlcnR5MTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

因为最多可能会跳M次，所以时间复杂度

$$O(NM\log Sum)$$

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
#define inf 0x3f3f3f
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=1e5+6;
ll s1[maxn],s2[maxn],s3[maxn],l,r;
int pre1[maxn],pre2[maxn],pre3[maxn],n,m,f[maxn];
inline bool check(ll lim){
	pre1[0]=0,pre2[0]=0,pre3[0]=0;
	rep(i,1,n){
		pre1[i]=pre1[i-1],pre2[i]=pre2[i-1],pre3[i]=pre3[i-1];
		while(s1[i]-s1[pre1[i]]>lim)pre1[i]++;
		while(s2[i]-s2[pre2[i]]>lim)pre2[i]++;
		while(s3[i]-s3[pre3[i]]>lim)pre3[i]++;
	}
	mem(f,inf);f[0]=0;
	rep(i,1,n){
		f[i]=min(f[i],f[pre3[i]]+1);
		int findpre1=i,findpre2=i,s=0;
		while(s<m&&(findpre1||findpre2)){
			if(findpre1>findpre2)findpre1=pre1[findpre1];else findpre2=pre2[findpre2];
			s++;
			f[i]=min(f[i],f[max(findpre1,findpre2)]+s);
		}
		if(f[i]>m)return 0;
	}
	return 1;
}
int main()
{
	n=read(),m=read();
	rep(i,1,n){int x=read();s1[i]=s1[i-1]+x;l=max(l,(ll)x);r+=x;}
	rep(i,1,n){int x=read();s2[i]=s2[i-1]+x;l=max(l,(ll)x);r+=x;}
	rep(i,1,n)s3[i]=s1[i]+s2[i];
	while(l<r){
		ll mid=(l+r)>>1;
		if(check(mid))r=mid;else l=mid+1;
	}
	cout<<l<<endl;
	return 0;
}
```


