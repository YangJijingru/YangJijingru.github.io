---
subtitle: "位运算打表找规律"
tags: 
 - 数论-杂题
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P49.jpg"
preview-img: "/img/preview/P49.jpg"
---

标签：数学

# 题目

[题目传送门](http://codeforces.com/contest/1110/problem/C）

# 题意

给出$Q$个询问，每次询问给出一个数$a_i$，请你求出$f(a) = \max_{0 < b < a}{gcd(a \oplus b, a \> \& \> b)}.$

$1 \le q \le 10^3$

$2 \le a_i \le 2^{25} - 1$

# 分析

结论题

两个数的XOR是不可能为0的，AND存在变为0的可能性

于是考虑如何快速找出$b$，使得两数AND变为0

之后打表（其实自己是直接暴力打表找规律的，感觉规律很好找）

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
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int d[25][2]={\{1,0},{3,1},{7,1},{15,5},{31,1},{63,21},{127,1},{255,85},{511,73},{1023,341},{2047,89},{4095,1365},{8191,1},{16383,5461},{32767,4681},{65535,21845},{131071,1},{262143,87381},{524287,1},{1048575,349525},{2097151,299593},{4194303,1398101},{8388607,178481},{16777215,5592405},{33554431,1082401}\};
int Q;
int main(){
    Q=read();
    while(Q--){
        int a=read();bool flag=0;
        rep(i,0,24)if(a==d[i][0]){cout<<d[i][1]<<endl;flag=1;}
        //puts("R");
        if(flag)continue;
        dep(i,23,0)if(a>d[i][0]){cout<<d[i+1][0]<<endl;break;}
    }
}
```