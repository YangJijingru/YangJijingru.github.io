---
subtitle: "判断最小生成树唯一性"
tags: 
 - 图论-最小生成树
 - POJ
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P36.jpg"
preview-img: "/img/preview/P36.jpg"
---

标签：最小生成树

# 题目

[题目传送门](http://poj.org/problem?id=1679)

# 题意

给定一个无向图，判断最小生成树是为具有唯一性，如果是唯一的，那么输出值

# 分析

## W1

可以直接求次小生成树，之后判断权值是否相等

可以上网直接搬模板了

## W2

先求出最小生成树之后记录结果，依次删除树中各边，再次求最小生成树，看是否存在相同的结果

用Kruscal写的时候，要判断下删完边之后这个图是否是**树**，如果删去边之后不存在树的话，那么最小生成树依然是唯一的

该代码如下

# Code
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstdlib>
#include<cstring>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define mem(x,num) memset(x,num,sizeof x)
#define ll long long
#define reg(x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=2e5+6;
int fa[maxn],n,m,T,sta[maxn],top=0;
struct edge{int u,v,w;}s[maxn];
inline int find(int x){return fa[x]==x?fa[x]:fa[x]=find(fa[x]);}
inline bool cmp(edge a,edge b){return a.w<b.w;}
inline int kruskal(int flag){
    int num=0,sum=0,tot=0;
    rep(i,1,m){
        if(i==flag)continue;
        int r1=find(s[i].u),r2=find(s[i].v);
        if(r1!=r2){
            fa[r1]=r2;sum+=s[i].w;num++;
            if(flag==-1)sta[++top]=i;
        }
        if(num==n-1)break;
    }
    rep(i,1,n)if(fa[i]==i)tot++;
    if(tot>1)return -1;
    return sum;
}
int main(){
    T=read();
    rep(TT,1,T){
        n=read(),m=read();
        mem(s,0);mem(sta,0);top=0;
        rep(i,0,n+1)fa[i]=i;
        rep(i,1,m)scanf("%d%d%d",&s[i].u,&s[i].v,&s[i].w);//s[i]=(edge){read(),read(),read()};
        sort(s+1,s+1+m,cmp);
        int ans=kruskal(-1),flag=0;
        rep(i,1,top){
            rep(j,0,n+1)fa[j]=j;
            int p=kruskal(sta[i]);
            if(p==ans){flag=1;break;}
        }
        if(flag)puts("Not Unique!");else printf("%d\n",ans);
    }
    return 0;
}
```