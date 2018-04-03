---
subtitle: "状压DP神仙题"
tags: 
 - DP-状压
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P95.jpg"
preview-img: "/img/preview/P95.jpg"
---
标签：状压DP

# 题目

[题目传送门](https://cn.vjudge.net/problem/AtCoder-2365)

数轴上有n个点，问从哪些点出发可以按以下方式经过所有点至少一次：

1. 移动到一个距离不超过m的点；
2. 移动到任意一个点，然后m/=2。

n,m<=200000

# 分析

整个过程中m显然只有$$\log m$$种取值，对于每种取值，肯定会使用它连续地走过一段区间

那么我们可以对于给定的m，求出它可以走多长的前缀，多长的后缀

最后枚举哪些操作用来走前缀，哪些操作用来走后缀

这个就可以状压DP解决了

f1[s]表示状态s时从1起向右最远能延伸的位置

f2[s]表示状态s时从n起向左最远能延伸的位置

num[i]为当使用i次操作2后的区间个数

R[i][k],使用i次操作2后的第k个区间的右端点 

最后判断剩下的一段能否在一开始只用操作1走完即可

其实如果开始时就超过$$\log_2 V+1$$个区间，那么不可能遍历所有绿洲

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
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
//**********head by yjjr**********
const int maxn=2e5+6;
int n,v,tot=-1,bin[26],R[26][maxn],num[26],f1[maxn<<2],f2[maxn<<2],a[maxn];
int main()
{
	n=read(),v=read();bin[0]=1;
	rep(i,1,20)bin[i]=bin[i-1]<<1;
	rep(i,1,n)a[i]=read();int x=v*2;
	while(x){
		x>>=1,tot++;
		rep(i,1,n-1){
			if(a[i+1]-a[i]<=x)continue;
			R[tot][++num[tot]]=i;
		}
		R[tot][++num[tot]]=n;
	}
	if(num[0]>tot+1){
		rep(i,1,n)puts("Impossible");
		return 0;
	}
	rep(s,0,bin[tot]-1)f2[s]=n+1;
	rep(s,0,bin[tot]-1)
        rep(i,1,tot){
            if(s&bin[i-1])continue;
            f1[s|bin[i-1]]=max(f1[s|bin[i-1]],*upper_bound(R[i]+1,R[i]+num[i]+1,f1[s]));
            f2[s|bin[i-1]]=min(f2[s|bin[i-1]],R[i][lower_bound(R[i]+1,R[i]+num[i]+1,f2[s]-1)-R[i]-1]+1);
        }
	rep(i,1,num[0]){
        bool flag=0;
        rep(s,0,bin[tot]-1)
            if(f1[s]>=R[0][i-1]&&f2[bin[tot]-1-s]<=R[0][i]+1){flag=1;break;}
        rep(j,R[0][i-1]+1,R[0][i]){
			if(flag)puts("Possible");else puts("Impossible");
		}
    }
	return 0;
}
```