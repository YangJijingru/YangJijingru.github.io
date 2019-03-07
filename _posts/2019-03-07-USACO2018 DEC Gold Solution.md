---
subtitle: "靠套路和技巧水出正解"
tags: 
 - 解题报告
 - USACO
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P42.jpg"
preview-img: "/img/preview/P42.jpg"
---

# 洛谷5122 [USACO18DEC]Fine Dining

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5122)
### 题目描述
漫长的一天结束了，饥困交加的奶牛们准备返回牛棚。

农场由 $N$ 片牧场组成（$2\le N\le 5\times 10^4$），方便起见编号为 $1\dots N$。所有奶牛都要前往位于牧场 $N$ 的牛棚。其他 $N-1$ 片牧场中每片有一头奶牛。奶牛们可以通过 $M$ 条无向的小路在牧场之间移动（$1\le M\le 10^5$）。第i条小路连接牧场 $a_i$ 和 $b_i$，通过需要时间 $t_i$。每头奶牛都可以经过一些小路回到牛棚。

由于饥饿，奶牛们很乐于在他们回家的路上停留一段时间觅食。农场里有 $K$ 个有美味的干草捆，第 $i$ 个干草捆的美味值为 $y_i$。每头奶牛都想要在她回牛棚的路上在某一个干草捆处停留，但是她这样做仅当经过这个干草捆使她回牛棚的时间增加不超过这个干草捆的美味值。注意一头奶牛仅仅“正式地”在一个干草捆处因进食而停留，即使她的路径上经过其他放有干草捆的牧场；她会简单地无视其他的干草捆。

### 输入输出格式
#### 输入格式
输入的第一行包含三个空格分隔的整数 $N$，$M$ 和 $K$。以下 $M$ 行每行包含三个整数 $a_i$，$b_i$ 和 $t_i$，表示牧场 $a_i$ 和 $b_i$ 之间有一条需要 $t_i$ 时间通过的小路（$a_i$ 不等于 $b_i$，$t_i$ 为不超过 $10^4$ 的正整数）。

以下 $K$ 行，每行以两个整数描述了一个干草捆：该干草捆所在牧场的编号，以及它的美味值（一个不超过 $10^9$ 的正整数）。同一片牧场中可能会有多个干草捆。

#### 输出格式

输出包含 $N-1$ 行。如果牧场 $i$ 里的奶牛可以在回牛棚的路上前往某一个干草捆并且在此进食，则第 $i$ 行为一个整数 $1$，否则为一个整数 $0$。

### 输入输出样例
#### 输入样例#1
```
4 5 1
1 4 10
2 1 20
4 2 3
2 3 5
4 3 2
2 7
```
#### 输出样例#1
```
1
1
1
```
### 说明
在这个例子中，牧场 $3$ 里的奶牛可以停留进食，因为她回去的时间仅会增加 $6$（从 $2$ 增加到 $8$），而这个增加量并没有超过干草捆的美味值 $7$。牧场 $2$ 里的奶牛显然可以吃掉牧场 $2$ 里的干草，因为这并不会导致她的最佳路径发生变化。对于牧场 $1$ 里的奶牛是一个有趣的情况，因为看起来她的最佳路径（$10$）可能会因为前去进食而有过大的增加量。然而，确实存在一条路径使得她可以前去干草捆处停留：先到牧场 $4$，然后去牧场 $2$（吃草），然后回到牧场 $4$。

## 分析

先跑一遍SPFA最短路，计算没有干草垛的情况下的最短时间

之后再跑一遍DP（代码和状态转移方程和SPFA类似）将不同的干草垛放进队列里进行转移

新建一个源点$n+1$，把它和每个干草垛之间建一条有向边，然后以该点出发，跑最短路，新建的边权设为$f[i]-v[i]$，这样就不需要去了解具体是哪个草垛了

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<cstdlib>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'&&ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
#define inf 1e9
const int maxn=2e5+6;
int n,m,K,dis[maxn],inq[maxn],last[maxn],ndis[maxn],S,cnt=0,que[maxn];
struct edge{int to,next,w;}e[maxn<<1];
void insert(int u,int v,int w){e[++cnt]=(edge){v,last[u],w};last[u]=cnt;}
void SPFA(){
    int head=0,tail=1;
    que[head]=S;dis[S]=0;inq[S]=1;
    while(head<tail){
        int now=que[head++];
        reg(now)
            if(dis[e[i].to]>dis[now]+e[i].w){
                dis[e[i].to]=dis[now]+e[i].w;
                if(!inq[e[i].to])inq[e[i].to]=1,que[tail++]=e[i].to;
            }
        inq[now]=0;
    }
}
int main(){
    n=read(),m=read(),K=read();
    rep(i,1,m){
        int u=read(),v=read(),w=read();
        //puts("R");
        insert(u,v,w);
        insert(v,u,w);
    }
    rep(i,0,n+2)dis[i]=inf;dis[S=n]=0;mem(inq,0);
    SPFA();
    rep(i,1,n)ndis[i]=dis[i];
    rep(i,1,K){
        int x=read(),y=read();
        insert(n+1,x,ndis[x]-y);
    }
    rep(i,0,n+2)dis[i]=inf;dis[S=n+1]=0;mem(inq,0);
    SPFA();
    rep(i,1,n-1)
        if(ndis[i]>=dis[i])puts("1");else puts("0");
    return 0;
}
```

# 洛谷5123 [USACO18DEC]Cowpatibility

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5123)

### 题目描述
研究证明，有一个因素在两头奶牛能否作为朋友和谐共处这方面比其他任何因素都来得重要——她们是不是喜欢同一种口味的冰激凌！

Farmer John 的 $N$ 头奶牛（$2\le N\le 5\times 10^4$）各自列举了她们最喜欢的五种冰激凌口味的清单。为使这个清单更加精炼，每种可能的口味用一个不超过 $10^6$ 的正整数 $\texttt{ID}$ 表示。如果两头奶牛的清单上有至少一种共同的冰激凌口味，那么她们可以和谐共处。

请求出不能和谐共处的奶牛的对数。

### 输入输出格式
#### 输入格式

输入的第一行包含 $N$。以下 $N$ 行每行包含 $5$ 个整数（各不相同），表示一头奶牛最喜欢的冰激凌口味。

#### 输出格式

输出不能和谐共处的奶牛的对数。

### 输入输出样例
#### 输入样例#1
``` 
4
1 2 3 4 5
1 2 3 10 8
10 9 8 7 6
50 60 70 80 90
```
#### 输出样例#1
```
4
```
### 说明
在这里，奶牛 $4$ 不能和奶牛 $1$、$2$、$3$ 中的任一头和谐共处，奶牛 $1$ 和奶牛 $3$ 也不能和谐共处。

## 分析

容斥原理

不和谐对数=总对数-和谐对数

------

枚举子集，用特殊符号分隔来判重

Map十分辣鸡的常数

速度垫底了qwq

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<cstdlib>
#include<algorithm>
#include<map>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'&&ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
map<string,ll>f;

ll n,ans=0,sum;string a[16],s;
int main(){
    n=read();ans=1ll*n*(n-1)/2;
    rep(i,1,n){
        rep(j,1,5)cin>>a[j];
        sum=0;sort(a+1,a+6);
        rep(i,1,(1<<5)-1){
            int tot=0;s="";
            rep(j,1,5)
                if(i&(1<<(j-1))){tot++;s+='?'+a[j];}
            if(tot&1)sum+=f[s]++;else sum-=f[s]++;
        }
        ans-=sum;
    }
    cout<<ans<<endl;
    return 0;
}
```

# 洛谷5124 [USACO18DEC]Teamwork

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5124)

### 题目描述
在 Farmer John 最喜欢的节日里，他想要给他的朋友们赠送一些礼物。由于他并不擅长包装礼物，他想要获得他的奶牛们的帮助。你可能能够想到，奶牛们本身也不是很擅长包装礼物，而 Farmer John 即将得到这一教训。

Farmer John 的 $N$ 头奶牛（$1\le N\le 10^4$）排成一行，方便起见依次编号为 $1\dots N$。奶牛 $i$ 的包装礼物的技能水平为 $s_i$。她们的技能水平可能参差不齐，所以 FJ 决定把她的奶牛们分成小组。每一组可以包含任意不超过 $K$ 头的连续的奶牛（$1\le K\le 10^3$），并且一头奶牛不能属于多于一个小组。由于奶牛们会互相学习，这一组中每一头奶牛的技能水平会变成这一组中水平最高的奶牛的技能水平。

请帮助 FJ 求出，在他合理地安排分组的情况下，可以达到的技能水平之和的最大值。

### 输入输出格式
#### 输入格式

输入的第一行包含 $N$ 和 $K$。以下 $N$ 行按照 $N$ 头奶牛的排列顺序依次给出她们的技能水平。技能水平是一个不超过 $10^5$ 的正整数。

#### 输出格式

输出 FJ 通过将连续的奶牛进行分组可以达到的最大技能水平和。

### 输入输出样例
#### 输入样例#1
```
7 3
1
15
7
9
2
5
10
```
#### 输出样例#1
```
84
```
### 说明

在这个例子中，最优的方案是将前三头奶牛和后三头奶牛分别分为一组，中间的奶牛单独成为一组（注意一组的奶牛数量可以小于 $K$）。这样能够有效地将 $7$ 头奶牛的技能水平提高至 $15$、$15$、$15$、$9$、$10$、$10$、$10$，和为 $84$。

## 题意

给出长度为$n$的序列$a_i$，将序列分为若干个长度不超过$K$的小组，每个小组内的数列权值可以变为该组内的最大值，询问最大和

## 分析

设$f[i]$为前$i$头奶牛可以达到的最大权值和

$f[i]=\max (f[j]+(i-j)\times \max (a[j])) \ \ \ (\max (0,i-k)\leq j < i)$

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<cstdlib>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'&&ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=2e4+6;
int n,m,a[maxn],f[maxn],mx;

int main(){
    n=read(),m=read();
    rep(i,1,n)a[i]=read();
    rep(i,1,n){
        mx=a[i];
        f[i]=f[i-1]+a[i];
        rep(j,1,m-1){
            if(i<=j)break;
            mx=max(mx,a[i-j]);
            f[i]=max(f[i-j-1]+mx*(j+1),f[i]);
        }
    }
    cout<<f[n]<<endl;
    return 0;
}
```