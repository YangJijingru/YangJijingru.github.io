---
subtitle: "奶牛套路题"
tags: 
 - 基础算法-模拟
 - 数据结构-链表
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P54.jpg"
preview-img: "/img/preview/P54.jpg"
---

标签：模拟，链表

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P4269)

## 题意简述

给出长度为$N$的雪道，每个位置$i$的雪深为$deep_i$

给出$B$双雪靴，第$i$双靴子最多可以在深度为$S_i$的雪中行走，每步可以使你前进$D_i$的单位距离（可以直接跳到当前位置$X+D_i$的地方）

请你求出哪些靴子可以帮助你走完全程

# 题解

使用双向链表，先在读入的同时记录下每个靴子的编号(id)

之后将雪深度和靴子的深度从大到小进行排序

对于每个靴子，找出来比它深度更深的雪，并在双向链表中删去（因为排序之后的单调性）

之后判断并记录答案就好了

套路题

# code
```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<cstdlib>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=Last[x];i;i=e[i].Next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr ******
const int maxn=1e5+6;
int n,b,mx=1,Last[maxn],Next[maxn];
bool ans[maxn];
struct node1{int dep,id;}f[maxn];
struct node2{int dep,dis,id;}s[maxn];
inline bool cmp1(node1 a,node1 b){return a.dep<b.dep;}
inline bool cmp2(node2 a,node2 b){return a.dep<b.dep;}
void Del(int x){Last[Next[x]]=Last[x];Next[Last[x]]=Next[x];}
int main(){
    n=read(),b=read();
    rep(i,1,n)f[i].dep=read(),f[i].id=i;
    rep(i,1,b)s[i].dep=read(),s[i].dis=read(),s[i].id=i;
    sort(f+1,f+1+n,cmp1);
    sort(s+1,s+1+b,cmp2);
    rep(i,1,n)Last[i]=i-1,Next[i]=i+1;
    int j=n;
    dep(i,b,1){
        while(j>=1&&f[j].dep>s[i].dep){
            Del(f[j].id);
            mx=max(mx,Next[f[j].id]-Last[f[j].id]);
            j--;
        }
        if(s[i].dis>=mx)ans[s[i].id]=1;  
    }
    rep(i,1,b)printf("%d\n",ans[i]);
    return 0;
}
```
