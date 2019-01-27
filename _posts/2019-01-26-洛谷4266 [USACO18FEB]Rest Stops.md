---
subtitle: "奶牛贪心题"
tags: 
 - 基础算法-贪心
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P56.jpg"
preview-img: "/img/preview/P56.jpg"
---
标签：贪心

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P4266)

## 题意简述

Farmer John和Bessie在爬山，高度为$L$，Farmer John需要$R_F$的时间爬一米，Bessie只需要$R_B$的时间爬一米，规定$R_B < R_F$（严格小于）。同时有$N$个休息站，Bessie可以决定在每个休息站休息多长时间（或者不休息），给出所有休息站的位置$X_i$和权值$C_i$，在一个休息站待$T$单位时间，答案可以获得$C_i\times T$的贡献值。请你在保证Bessie领先于Farmer John的情况下，最大化答案。

# 题解
## 中文
贪心的思想：将所有休息站的权值按照$C_i$的从大到小排序，之后尽量选择在$C_i$最大的休息站休息，然后继续在下一个区域内具有最大$C_j$的休息站休息。

贪心的证明：如果没有在当前区域内$C_i$最大的休息站休息，那么就选择其它休息站$k$休息，那么$C_k$一定小于$C_i$，即$C_k\times T < C_i\times T$，那么最终答案一定不是最优解，所以贪心是正确的

## English
We can simply perform a **greedy** algorithm: stop at a rest stop which has the **Maximal** $C_i$ in this area you can arrived, and stay at this rest stop as long as possible. After that, find the next rest stop which also has the **Maximal** $C_j$ in next area. It's supposed $P_j > P_i$. And also stay at rest stop $J$ as long as possible, until Farmer John is coming. 

It's easy to prove the greedy algorithm: If it didn't stay at the maximal $C_i$ rest stop, then changed to another rest stop $C_k$, that means $C_k$ must be less than $C_i$, so that $C_k\times T < C_i\times T$. So that can't be the best answer, so the greedy algorithm is correct.

# Code
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
const int maxn=1e6+6;
ll l,n,f,b;
struct node{ll x,c;}a[maxn];
inline bool cmp(node a,node b){return a.c>b.c;}
int main(){
    l=read(),n=read(),f=read(),b=read();
    rep(i,1,n)a[i].x=read(),a[i].c=read();
    ll t=f-b;
    sort(a+1,a+1+n,cmp);
    ll ans=a[1].c*a[1].x*t,last=1;
    rep(i,2,n)
        if(a[i].x>a[last].x)ans+=a[i].c*((a[i].x-a[last].x)*t),last=i;
    cout<<ans<<endl;
    return 0;
}
```
