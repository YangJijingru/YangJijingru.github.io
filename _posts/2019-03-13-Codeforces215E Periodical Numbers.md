---
subtitle: "细节超多的数位DP"
tags: 
 - DP-杂题
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P33.jpg"
preview-img: "/img/preview/P33.jpg"
---

# 题目

[题目传送门](http://codeforces.com/problemset/problem/215/E)

# 题意

定义$01$周期字符串为存在周期性子串的字符串，比如$100100$和$111$

给出两个十进制整数，请求出在两个整数之间有多少个数的二进制为$01$周期字符串

# 分析

数位DP

枚举二进制长度和循环周期长度进行转移

注意要去重（因为枚举周其长度为6的时候已经包含了周期为2和3的）

当枚举到最大二进制长度时要特殊处理最大的循环

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
const int maxn=106;
int a[maxn];ll f[maxn],n,m;
inline ll cal(int len,int j,ll x){
    ll c=0,b=1;
    rep(i,1,j)c+=(a[len-i+1]<<(j-i));
    b=c;
    rep(i,1,len/j-1)b<<=j,b+=c;
    return (ll)(c-(ll)(1<<(j-1))+(b<=x));
}

inline ll solve(ll x){
    int len=0;ll tmp=x,ans=0;
    while(tmp){
        a[++len]=tmp&1;
        tmp>>=1;
    }
    rep(i,2,len){
        mem(f,0);
        rep(j,1,i/2){
            if(i%j!=0)continue;
            if(i<len)f[j]+=(1<<(j-1));else f[j]+=cal(len,j,x);
            rep(k,1,j-1)if(j%k==0)f[j]-=f[k];
            ans+=f[j];
        }
    }
    return ans;
}
int main(){
    n=read(),m=read();
    cout<<(ll)(solve(m)-solve(n-1))<<endl;
    return 0;
}
```