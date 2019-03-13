---
subtitle: "矩阵离散化"
tags: 
 - 特殊-bitset
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P35.jpg"
preview-img: "/img/preview/P35.jpg"
---

标签：离散化

# 题目

[题目传送门](http://codeforces.com/contest/1137/problem/A)

# 题意

给定$n\times m$的矩阵，离散化矩阵，使得对于点$(x,y)$在其自己的行和列的$n+m-1$个数中的相对位置不变（并不是整体不变）

# 分析

二维离散化（使用lower_bound和unique）

之后贪心判断大小输出

# Code
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
//******head by yjjr******
#include<vector>
#define pb push_back
const int maxn=1e3+6;
int n,m,h[maxn][maxn];
vector<int> r[maxn],c[maxn];
int main(){
    n=read(),m=read();
    rep(i,1,n)rep(j,1,m)h[i][j]=read();
    rep(i,1,n){
        rep(j,1,m)r[i].pb(h[i][j]);
        sort(r[i].begin(),r[i].end());
        r[i].erase(unique(r[i].begin(),r[i].end()),r[i].end());
    }
    rep(j,1,m){
        rep(i,1,n)c[j].pb(h[i][j]);
        sort(c[j].begin(),c[j].end());
        c[j].erase(unique(c[j].begin(),c[j].end()),c[j].end());
    }
    rep(i,1,n){
        rep(j,1,m){
            int t1=lower_bound(r[i].begin(),r[i].end(),h[i][j])-r[i].begin();
            int t2=lower_bound(c[j].begin(),c[j].end(),h[i][j])-c[j].begin();
            int t3=r[i].end()-lower_bound(r[i].begin(),r[i].end(),h[i][j]);
            int t4=c[j].end()-lower_bound(c[j].begin(),c[j].end(),h[i][j]);
            printf("%d ",max(t1,t2)+max(t3,t4));
        }
        puts("");
    }
    return 0;
}
```