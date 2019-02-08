---
subtitle: "真的神仙DP题"
tags: 
 - DP-杂题
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P48.jpg"
preview-img: "/img/preview/P48.jpg"
---
标签：DP

# 题目

[题目传送门](http://codeforces.com/contest/1110/problem/D)

# 题意

给出$n$张卡牌，每张卡牌上的数字为$x\in [1,m]$，三张数字相同的卡牌或者数字递增的卡牌能够组成一个三元组，问最多能够组成多少个三元组

$1 \le n, m \le 10^6$

# 分析

比赛的时候看完题感觉是个对我很难的DP，就直接放弃了

---

设计状态$f[i][j][k]$表示存在$j$个$[i-1,i,i+1]$，$k$个$[i,i+1,i+2]$组合的卡牌时最大的组合数

状态转移方程为$f[i+1][k][l]=max(f[i+1][k][l],f[i][j][k]+l+(a[i+1]-j-l-k)/3)$

非常巧妙的状态设计和转移，很神仙了

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
int n,m,a[maxn],f[maxn][3][3];
int main(){
    n=read(),m=read();
    rep(i,1,n)a[read()]++;
    rep(i,0,m)rep(j,0,2)rep(k,0,2)rep(l,0,2){
        if(l+k+j>a[i+1])break;
        f[i+1][k][l]=max(f[i+1][k][l],f[i][j][k]+l+(a[i+1]-j-l-k)/3);
    }
    cout<<f[m+1][0][0]<<endl;
    return 0;
}
```