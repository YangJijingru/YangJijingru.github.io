---
subtitle: "扫描线的简单应用"
tags: 
 - 特殊-扫描线
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P57.jpg"
preview-img: "/img/preview/P57.jpg"
---

标签：扫描线

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P4264)

## 题意简述

给出$$N$$堆牛粪，每个都需要从$$A_i$$移动到$$B_i$$，代价为$$\vert A_i-B_i\vert$$。同样你可以选择一个坐标建立传送门，从$$X=0$$到传送门坐标$$Y$$不需要代价。请你最小化所有移动的代价。

#题解

可以用图象宏观的推测答案，原本的答案应该会是一个山峰上凸状的函数，现在要挖去其中一段

同时可以列出来函数$$F(Y)$$表示在$$Y$$坐标建立传送门，最终所需要的代价

$$F(Y)=\sum_{i=1}^n min(|A_i-B_i|, |A_i|+|Y-B_i|)$$

这样可以用扫描线来解决此题

扫描线中加入每个线段的起止点，之后按照坐标大小排序

在扫描的过程中不停的计算并覆盖之前所产生的函数值，将其与原始答案比较

详见代码

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
const int maxn=3e5+6;
ll ans=0;int n,tot=0;
struct node{ll c;int t;}s[maxn];
inline bool cmp(node a,node b){return a.c<b.c;}
int main(){
    n=read();
    rep(i,1,n){
        int a=read(),b=read();
        ans+=abs(a-b);
        if(abs(a-b)>abs(a)){
            ll tmp=abs(a-b)-abs(a);
            s[++tot]={b-tmp,-1};s[++tot]={b+tmp,-1};
            s[++tot]={b,2};
        }
    }
    sort(s+1,s+1+tot,cmp);
    int pre=s[1].c,sum=s[1].t;
    ll re=ans;
    rep(i,2,tot){
        re+=(s[i].c-pre)*sum;
        pre=s[i].c;
        sum+=s[i].t;
        ans=min(ans,re);
    }
    cout<<ans<<endl;
    return 0;
}
```



