---
subtitle: "模拟水题"
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P100.jpg"
preview-img: "/img/preview/P100.jpg"
---

标签：模拟

CSDN失踪人口回归了，毕竟还是要准备SOI的，从今天开始重新写题解了

## 题意

给出一串由0,1,2表示的数字，0和1分别表示不同的颜色，2表示无色，和两种变色的花费，现在使这串数字变为回文串，使得总花费最小

## 题解

从左到右，分几种情况讨论下就好了，分类讨论

## code

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
const int maxn=1006;
int a[maxn],n,Wh,Bl,ans;
int main()
{
	n=read();Wh=read(),Bl=read();
	rep(i,1,n)a[i]=read();
	bool flag=1;ans=0;
	rep(i,1,n/2){
		if(a[i]!=2&&a[n-i+1]!=2&&a[i]!=a[n-i+1]){flag=0;break;}
		if(a[i]==2&&a[n-i+1]==2){ans=ans+2*min(Wh,Bl);continue;}
		if(a[i]==2&&a[n-i+1]==0){ans=ans+Wh;continue;}
		if(a[i]==0&&a[n-i+1]==2){ans=ans+Wh;continue;}
		if(a[i]==2&&a[n-i+1]==1){ans=ans+Bl;continue;}
		if(a[i]==1&&a[n-i+1]==2){ans=ans+Bl;continue;}
	}
	if(n%2==1&&a[n/2+1]==2)ans=ans+min(Wh,Bl);
	if(!flag)puts("-1");else cout<<ans<<endl;
	return 0;
}
```