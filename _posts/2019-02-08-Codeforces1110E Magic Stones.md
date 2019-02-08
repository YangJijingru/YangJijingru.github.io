---
subtitle: "CF日常智商题"
tags: 
 - 数论-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P47.jpg"
preview-img: "/img/preview/P47.jpg"
---

标签：差分，数学

# 题目

[题目传送门](http://codeforces.com/contest/1110/problem/E)

# 题意

给定序列$c_i$和$t_i$，变换操作为$c_i' = c_{i + 1} + c_{i - 1} - c_i$，询问序列$c_i$是否能变为$t_i$

$2 \le n \le 10^5$

$0 \le c_i , t_i \le 2 \cdot 10^9$

# 分析

观察样例后可以发现

两个数列能够互相转化的要求是：

- 首尾位置相同
- 差分序列排序后相同

在CF的时候自己却想着去数学模拟了（摔）

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
const int maxn=2e5+6;
int n,c[maxn],t[maxn],a[maxn],b[maxn];
int main(){
    n=read();
    rep(i,1,n)c[i]=read();
    rep(i,1,n)t[i]=read();
    rep(i,1,n-1)a[i]=c[i+1]-c[i];
    rep(i,1,n-1)b[i]=t[i+1]-t[i];
    sort(a+1,a+n);
    sort(b+1,b+n);
    rep(i,1,n-1)if(a[i]!=b[i]){puts("NO");return 0;}
    if((c[1]!=t[1])||(c[n]!=t[n])){puts("NO");return 0;}
    puts("YES");
    return 0;
}
```