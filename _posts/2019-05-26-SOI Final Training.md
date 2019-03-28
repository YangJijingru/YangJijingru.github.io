---
subtitle: "以梦为马，不负韶华"
tags: 
 - dairy
 - 知识学习
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P73.jpg"
preview-img: "/img/preview/P49.jpg"
---

**本文长文/高能预警**

由于博主过于懒惰和菜鸡，打算把未来50天内的大多数训练题目全部放在此博文中，将会不定时更新

# DP

## CF802K 

无意中做到了瑞士HC2 EPFL的题目，果然很SOI，很侧重思维性

------

题意：给定一棵树，可以从一个点开始在树上行走，每条边上有一个宝物价值为$w$，每个节点最多访问$k$次，求能够得到的最大价值

$f[x][1]$表示访问完以$x$节点为根的子树后**返回**到$x$的父节点所能得到的最大价值

$f[x][0]$表示访问玩以$x$节点为根的子树后**不返回**$x$的父节点所能得到的最大价值

Code
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#include<vector>
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
#define pb push_back
const int maxn=1e5+6;
int n,K,cnt=0,last[maxn],f[maxn][2];
struct edge{int to,next,w;}e[maxn<<1];
void insert(int u,int v,int w){e[++cnt]=(edge){v,last[u],w};last[u]=cnt;}
void dfs(int x,int fa,int Val){
    f[x][0]+=Val,f[x][1]+=Val;
    vector< pair<int , int> >tmp;
    reg(x){
        if(e[i].to==fa)continue;
        dfs(e[i].to,x,e[i].w);
        tmp.pb(make_pair(f[e[i].to][1],e[i].to));
    }
    
    sort(tmp.begin(),tmp.end());
    reverse(tmp.begin(),tmp.end());
    int ans=0,mx=0,len=tmp.size()-1;
    rep(i,0,min(K-2,len))f[x][1]+=tmp[i].first;
    rep(i,0,min(K-1,len))ans+=tmp[i].first;
    rep(i,0,len){
        if(i<K)mx=max(mx,ans-tmp[i].first+f[tmp[i].second][0]);
        else mx=max(mx,ans-tmp[K-1].first+f[tmp[i].second][0]);
    }//puts("R");
    f[x][0]+=mx;
}
int main(){
    n=read(),K=read();
    rep(i,1,n-1){
        int u=read(),v=read(),w=read();
        insert(u,v,w);insert(v,u,w);
    }
    dfs(0,-1,0);
    cout<<f[0][0]<<endl;
    return 0;
}
```