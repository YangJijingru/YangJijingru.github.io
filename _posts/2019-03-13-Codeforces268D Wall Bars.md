---
subtitle: "五维神仙加速DP"
tags: 
 - DP-杂题
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P32.jpg"
preview-img: "/img/preview/P32.jpg"
---
# 题目

[题目传送门](http://codeforces.com/problemset/problem/268/D)

# 题意

给出楼（或者可以说是模具）的高度$n$以及一次最大能够上升的高度$h$，模具存在四个不同的方向（$\frac {360} 4 = 90$），每个方向上有若干个阶梯，无法在跳跃过程中更换方向（且需要满足两个阶梯之间的距离小于$h$）

求出有多少种模具能够满足从底上到顶

---

尽最大能力去描述题意了qwq

# 分析

设计DP状态$f[i][a][b][c][d]$表示当前高度为$i$，选定一个方向为主要面$a$（连续为$1$，否则为$0$），其余三面现在距离分别为$b,c,d$时的方案数

非主要面超过h的话就直接记为h，因为如果不作为主要面的话，高度差值大于$h$之后无影响（和等于$h$等效）

转移时在四个方向依次旋转一遍，表示分别在四个方向构造主要面

出题人高智商转移%%%

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
const int mod=1e9+9,maxn=1e3+6;
int n,h,f[maxn][2][36][36][36],ans;
int main(){
    n=read(),h=read();
    f[0][0][0][0][0]=1;
    rep(i,1,n)
        rep(a,0,1)
            rep(b,0,h)
                rep(c,0,h)
                    rep(d,0,h){
                        int tmp=f[i-1][a][b][c][d];
                        (f[i][a<1?0:1][b<h?b+1:h][c<h?c+1:h][d<h?d+1:h]+=tmp)%=mod;
                        (f[i][b<h?0:1][c<h?c+1:h][d<h?d+1:h][a<1?a+1:h]+=tmp)%=mod;
                        (f[i][c<h?0:1][d<h?d+1:h][a<1?a+1:h][b<h?b+1:h]+=tmp)%=mod;
                        (f[i][d<h?0:1][a<1?a+1:h][b<h?b+1:h][c<h?c+1:h]+=tmp)%=mod;
                    }
    rep(a,0,1) 
        rep(b,0,h)
            rep(c,0,h)
                rep(d,0,h)
                    if(a<1||b<h||c<h||d<h)(ans+=f[n][a][b][c][d])%=mod;
    cout<<ans<<endl;
    return 0;
}
```