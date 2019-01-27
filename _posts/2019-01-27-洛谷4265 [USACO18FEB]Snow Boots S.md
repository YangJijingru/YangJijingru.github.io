---
subtitle: "奶牛模拟题"
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P55.jpg"
preview-img: "/img/preview/P55.jpg"
---

标签：贪心，模拟

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P4265)

## 题意简述

给出长度为$N$的雪道，每个位置$i$的雪深为$deep_i$

给出$B$双雪靴，第$i$双靴子最多可以在深度为$S_i$的雪中行走，最多可以使你前进$D_i$的单位距离（可以直接跳到当前位置$X+D_i$的地方）

在更换雪靴的过程中需要保证更换前和更换后的雪靴深度均小于当前雪深

请你在能够顺利到达终点位置$N$的情况下，最小化雪靴数量。

# 题解

直接暴力更新，或者USACO官方说dfs也行（雾）

时间复杂度$O(N^3)$

# code
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
int dep[maxn],n,b,ans=0;
struct node{int s,d;}a[maxn];
bool f[maxn];
int main(){
    n=read(),b=read();
    rep(i,1,n)dep[i]=read();
    rep(i,1,b)a[i].s=read(),a[i].d=read();
    rep(i,1,n)f[i]=0;
    f[1]=1;
    rep(i,1,b){
        rep(j,1,n)
            if(f[j]&&(a[i].s>=dep[j]))
                rep(k,j,min(n,j+a[i].d))
                    if(a[i].s>=dep[k])f[k]=1;
        if(f[n]){ans=i;break;}
    }
    cout<<ans-1<<endl;
    return 0;
}
```
