---
subtitle: "思维题"
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P99.jpg"
preview-img: "/img/preview/P99.jpg"
---
标签：模拟

## 题意

给了n个初始状态为正面的煎饼~~（雾？？反正可以这样理解）~~，每次可以选择区间$$[i-k,i+k]$$内的煎饼翻一面，问最少可以花费多少次把所有煎饼都翻成反面

## 题解

分类讨论

- 当$$k=0$$时，需要翻n次
- 当$$n\leq2\times k+1$$时，只需要中间的翻一次
- 当$$n>2\times k+1$$时，要具体确定关系，判断从哪里开始翻

## code
```cpp
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
#define reg(x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
//******head by yjjr******
const int maxn=1e3+6;
int n,k,a[maxn],ans,kk;
int main(){
	n=read(),k=read(),kk=2*k+1;
	if(!k){ans=n;rep(i,1,n)a[i]=i;}
	else if(n<=kk){ans=1;a[1]=n/2+1;}
	else if(n>kk){
		if(n%kk>k||!(n%kk)){ans=0;for(int i=k+1;i<=n;i+=kk)a[++ans]=i;}
		else{ans=0;for(int i=1;i<=n;i+=kk)a[++ans]=i;}
	}
	printf("%d\n",ans);
	rep(i,1,ans)printf("%d ",a[i]);
	return 0;
}
```