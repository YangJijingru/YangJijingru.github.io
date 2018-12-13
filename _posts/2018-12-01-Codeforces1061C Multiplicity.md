---
subtitle: "调整DP顺序优化空间"
tags: 
 - DP-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P60.jpg"
preview-img: "/img/preview/P60.jpg"
---
标签：DP

# 题目

[题目传送门](http://codeforces.com/contest/1061/problem/C)

# 题意

给出序列$$a_i$$，询问有多少个a的子序列b满足，对于任意$$b_i$$，$$b[i]\mod i=0$$

# 分析

简单DP

$$f[i][j]=f[i-1][j-1]+f[i-1][j] (a[i]\mod j=0)$$

$$f[i][j]=f[i-1][j] (a[i]\mod j\not= 0)$$

这样肯定会MLE

之后考虑优化

对于每个$$i$$而言，只在j为a[i]的因数才更新

因为同一层中不能互相影响，所以需要从大到小处理

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
const int maxn=1e6+6,mod=1e9+7;
int f[maxn],cnt,n,v[maxn];
int main(){
    n=read();
    f[0]=1;
    rep(i,1,n){
        int x=read(),m=sqrt(x);cnt=0;
        rep(j,1,m)
            if(x%j==0){
                v[++cnt]=j;
                if(j*j!=x)v[++cnt]=x/j;
            }
        sort(v+1,v+1+cnt);
        dep(j,cnt,1)f[v[j]]=(f[v[j]]+f[v[j]-1])%mod;
    }
    ll ans=0;
    rep(i,1,n)ans=(ans+f[i])%mod;
    cout<<ans<<endl;
    return 0;
}
```
