---
subtitle: "LIS的巧妙转化"
tags: 
 - DP-杂题
 - 数据结构-树状数组
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P97.jpg"
preview-img: "/img/preview/P97.jpg"
---
标签：LIS，DP，树状数组

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=5442)

## Description

给定$$n(n\leq 200,000)$$,你可以将任意$$a[l]$$至$$a[r](1\leq l\leq r\leq n)$$每一个元素加上一个$$d(-x\leq d\leq x)$$,求$$a$$数组的最大严格上升子序列长度。

## Input

第1行 两个整数$$n,x;$$

第2行 $$n$$个整数表示$$a[1]$$至$$a[n]$$

## Output

一个数,即$$a$$数组的最大严格上升子序列长度。

## Sample Input
```
8 10
7 3 5 12 2 7 3 4
```
## Sample Output
```
5
```

# 题解

看数据范围2e5，时间复杂度应该就是$$O(n\log n)$$了

首先你要知道几个性质：

- 如果在$$[l,r]+num$$，不如在$$[l,n]+num$$
- 如果在$$[l,r]-num$$，不如在$$[1,r]-num$$，不如在$$(r,n]+num$$
- 如果在$$[l,r]+1$$，不如在$$[l,r]+max$$，即选定区间加最大值

问题转化为：给你一个序列，可以在$$[x,n]$$范围内加上$$m$$，求LIS

之后就正着跑和反着跑，各求一遍LIS，最后用树状数组合并

# code
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
#define lowbit(x) (x&(-x))
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//**********head by yjjr**********
const int maxn=2e5+6;
int top,sta[maxn],sum[maxn<<1],n,x,a[maxn],b[maxn<<1],c[maxn],f1[maxn],f2[maxn],ans=0,pos;
void modify(int x,int val){for(int i=x;i<=2*n;i+=lowbit(i))sum[i]=max(sum[i],val);}
inline int getmax(int x){int re=0;for(int i=x;i;i-=lowbit(i))re=max(re,sum[i]);return re;}
int main(){
    n=read(),x=abs(read());
    rep(i,1,n)a[i]=b[i]=c[i]=read(),b[i+n]=a[i]+x-1;
    sort(b+1,b+1+n*2);
    rep(i,1,n)a[i]=lower_bound(b+1,b+1+2*n,a[i])-b; 
    rep(i,1,n)
        if(a[i]>sta[top])sta[++top]=a[i],f1[i]=top;
        else{pos=lower_bound(sta+1,sta+1+top,a[i])-sta;sta[pos]=a[i],f1[i]=pos;}
    top=0;
    dep(i,n,1){
        int tmp=2*n+1-a[i];
        if(tmp>sta[top])sta[++top]=tmp,f2[i]=top;
        else{pos=lower_bound(sta+1,sta+1+top,tmp)-sta;sta[pos]=tmp,f2[i]=pos;}
    }
    modify(a[1],f1[1]);
    rep(i,2,n){
        int t=lower_bound(b+1,b+1+2*n,c[i]+x-1)-b,post=getmax(t);
        if(f2[i]+post>ans)ans=f2[i]+post,pos=t;
        modify(a[i],f1[i]);
    }
    cout<<ans<<endl;
    return 0;
}
```
