---
subtitle: "铂金毒瘤题"
tags: 
 - 解题报告
 - USACO
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P45.jpg"
preview-img: "/img/preview/P45.jpg"
---

# 洛谷5202 [USACO19JAN]Redistricting

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5202)

### 题目背景

USACO 19年一月月赛铂金组第一题

### 题目描述
奶牛们的特大城市，牛都，要进行重新分区了！——这总是一个在居住在这里的两大主要种族（荷斯坦牛和更赛牛）之间富有争议的政治事件，因为两大种族都想要在牛都政府中保持足够的影响力。
牛都的大都市圈由一列N块牧草地（$1≤N≤3\times 10^5$）组成，每块里有一头奶牛，均为荷斯坦牛和更赛牛之一。

牛都政府想要将大都市圈划分为若干个连续的区，使得每个区至多包含K块牧草地（$1≤K≤N$），并且每块牧草地恰好属于一个区。由于政府当前由荷斯坦牛控制，她们想要找到一种分区方式能够最小化更赛牛较多或者均势的区的数量（如果更赛牛的数量与荷斯坦牛的数量相等那么这个区就是均势的）。

有一个关心政治的更赛牛团体想要知道政府的分区计划可能会对她们造成多少损害。帮助她们求出最坏情况，也就是更赛牛较多或是均势的区的最小可能的数量。

### 输入输出格式

#### 输入格式
输入的第一行包含两个空格分隔的整数$N$和$K$。第二行包含一个长度为N的字符串。每个字符均为'H'或者'G'，表示荷斯坦牛（Holstein）或者更赛牛（Guernsey）。

#### 输出格式
输出更赛牛较多的或者均势的分区的最小可能数量。

### 输入输出样例
#### 输入样例#1
```
7 2
HGHGGHG
```
#### 输出样例#1
```
3
```

## 题意

给出一个有两种不同颜色的序列，现在把这个序列划分成若干段，每个连续段的长度小于$K$，问最少有多少段第二种颜色的数量大于等于第二种颜色的数量

## 分析

可以将序列不同的颜色分别记为$+1,-1$，做完前缀和之后在二维平面上表示，将原序列转换为另一个子问题：平面上存在$n$个点，横坐标相邻的两个点之间，纵坐标绝对值最多相差$1$，每一步可以跳跃不超过$K$个点，问最少有多少次跳跃之后坐标会下降

------

暴力DP的做法就是直接枚举点的坐标进行转移

如果点$j$的坐标大于$i$的坐标，那么存在状态转移方程$f[j]=min(f[i],f[j]);$，否则$f[j]=min(f[i]+1,f[j]);$

------

观察上述状态转移方程，即最优转移要么是当前DP的区间内最小值，要么是比$f[i]$增加一个新的区间

前者可以使用单调队列维护

后者可以使用额外的数组记录状态中最靠右的数值（玄学操作请参见代码）

## Code
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
int n,K,g[maxn],que[maxn<<1],f[maxn];
char s[maxn];
inline bool cal(int x,int y){
    int tmp=g[y]-g[x];
    if((tmp<<1)>=y-x)return 1;else return 0;
}
int main(){
    n=read(),K=read();scanf("%s",s+1);
    rep(i,1,n)if(s[i]=='G')g[i]=g[i-1]+1;else g[i]=g[i-1];
    int head=1,tail=1;que[1]=0;
    rep(i,1,n){
        while(head<=tail&&que[head]<i-K)head++;
        f[i]=f[que[head]]+cal(que[head],i);
        while(head<=tail&&(f[i]<f[que[tail]]||(f[i]==f[que[tail]]&&cal(que[tail],i)!=0)))tail--;
        que[++tail]=i;
    }
    cout<<f[n]<<endl;
    return 0;
}
```
