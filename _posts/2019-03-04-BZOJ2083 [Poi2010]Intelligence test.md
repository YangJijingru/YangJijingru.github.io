---
subtitle: "二分懒人写法"
tags: 
 - 基础算法-二分
 - POI
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P44.jpg"
preview-img: "/img/preview/P44.jpg"
---

标签：二分

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=2083)

## Description
霸中智力测试机构的一项工作就是按照一定的规则删除一个序列的数字，得到一个确定的数列。Lyx很渴望成为霸中智力测试机构的主管，但是他在这个工作上做的并不好，俗话说熟能生巧，他打算做很多练习，所以他希望你写一个程序来快速判断他的答案是否正确。

## Input
第一行为一个整数$m$（$1<=m<=1000000$）第二行包括$m$个用空格分开的整数$a_i$（$1<=a_i<=1000000$），组成了最初的序列，第三行为一个整数$n$（$1<=n<=1000000$），表示$n$个Lyx经过一系列删除得到的序列，每个序列两行，第一行给出长度L（$1<=L<=m$），然后下一行为L个由空格分开的整数$b_i$（$1<=b_i<=1000000$）。

## Output
共$n$行，如果Lyx的序列确实是由最初的序列删除一些数得到，就输出TAK，否则输出NIE。

## Sample Input
```
7
1 5 4 5 7 8 6
4
5
1 5 5 8 6
3
2 2 2
3
5 7 8
4
1 5 7 4
```
## Sample Output
```
TAK
NIE
TAK
NIE
```

# 分析

因为要使询问的子序列在原序列中的相对位置不变，所以对于$b$中的每一个数字查找原序列中距离当前数字最近的位置，如果找不到则无解

可以使用二分+vector来实现

# code
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<vector>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
#define ll long long
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
#define pb push_back
const int maxn=1e6+6;
int Q,m,b[maxn],p[maxn];
vector <int> a[maxn];
inline bool solve(){
    int len=read(),now=0;
    rep(i,1,len)b[i]=read(),p[b[i]]=0;
    rep(i,1,len){
        p[b[i]]=upper_bound(a[b[i]].begin()+p[b[i]],a[b[i]].end(),now)-a[b[i]].begin();
        if(p[b[i]]==a[b[i]].size())return 0;
        now=a[b[i]][p[b[i]]];
    }
    return 1;
}
int main(){
    m=read();
    rep(i,1,m){
        int x=read();
        a[x].pb(i);
    }
    Q=read();
    while(Q--){
        if(solve())puts("TAK");else puts("NIE");
    }
    return 0;
}
```