---
subtitle: "金组智商拼盘"
tags: 
 - 解题报告
 - USACO
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P46.jpg"
preview-img: "/img/preview/P46.jpg"
---

# 洛谷5196 [USACO19JAN]Cow Poetry

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5196)

### 题目背景

USACO19年一月金组第一题

### 题目描述
不为Farmer John所知的是，Bessie还热衷于资助艺术创作！最近，她开始研究许多伟大的诗人们，而现在，她想要尝试创作一些属于自己的诗歌了。
Bessie认识$N（1≤N≤5000）$个单词，她想要将她们写进她的诗。Bessie已经计算了她认识的每个单词的长度，以音节为单位，并且她将这些单词划分成了不同的“韵部”。每个单词仅与属于同一韵部的其他单词押韵。

Bessie的每首诗由M行组成$（1≤M≤10^5）$，每一行必须由$K（1≤K≤5000）$个音节构成。此外，Bessie的诗必须遵循某个指定的押韵模式。

Bessie想要知道她可以写出多少首符合限制条件的不同的诗。

### 输入输出格式
#### 输入格式

输入的第一行包含$N,M,K$。
以下N行，每行包含两个整数$s_i（1≤s_i≤K）$和$c_i（1≤c_i≤N）$。这表示Bessie认识一个长度（以音节为单位）为$s_i$、属于韵部$c_i$的单词。

最后M行描述了Bessie想要的押韵模式，每行包含一个大写字母$e_i$。所有押韵模式等于$e_i$的行必须以同一韵部的单词结尾。不同$e_i$值的行并非必须以不同的韵部的单词结尾。

#### 输出格式

输出Bessie可以写出的满足这些限制的不同的诗的数量。由于这个数字可能非常大，请计算这个数对$1,000,000,007$取余的结果。

### 输入输出样例
#### 输入样例#1
``` 
3 3 10
3 1
4 1
3 2
A
B
A
```
#### 输出样例#1
```
960
```
### 说明
在这个例子中，Bessie认识三个单词。前两个单词押韵，长度分别为三个音节和四个音节，最后一个单词长度为三个音节，不与其他单词押韵。她想要写一首三行的诗，每行包含十个音节，并且第一行和最后一行押韵。共有$960$首这样的诗。以下是一首满足要求的诗（其中$1,2,3$分别代表第一个、第二个、第三个单词）：$121 123 321$。

## 分析

又是一道神仙DP题

设状态$f[i][j]$表示长度为$i$的句子，最后结尾的韵律为$j$的组合方案数

最后一个单词为$j$，上一个单词的韵律为$k$

$f[i][Y]=\sum_{y[j]=Y} \sum_{k=1}^S f[i-l[j]][k]$

但这样转移的时间复杂度为$O(N^2\times K)$

观察后发现可以通过预处理降低时间复杂度

$g[i]=\sum_{k=1}^S f[i][k]$

那么$f[i][Y]=\sum_{y[i]=Y} g[i-l[j]]$

$Ans[i]=\sum_{j=1}^S (f[k][j])^{cnt[i]}$

通过$g$数组的建立，可以将转移的时间复杂度降低为$O(N\times K)$

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
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int mod=1e9+7,maxn=5e3+6;
int f[maxn],g[maxn],num[maxn],s[maxn],c[maxn];
int n,m,k;ll ans=1;
char ch;
inline ll qpow(ll x,int y){
    ll re=1;
    while(y){
        if(y&1)re=1ll*re*x%mod;
        x=1ll*x*x%mod;
        y>>=1;
    }
    return re;
}
int main(){
    n=read(),m=read(),k=read();
    rep(i,1,n)s[i]=read(),c[i]=read();
    f[0]=1;
    rep(i,0,k-1)
        rep(j,1,n)
            if(i+s[j]<=k)f[i+s[j]]=(f[i+s[j]]+f[i])%mod;
    rep(i,1,n)g[c[i]]=(f[k-s[i]]+g[c[i]])%mod;
    rep(i,1,m){cin>>ch;++num[ch-'A'];}
    rep(i,0,25){
        if(!num[i])continue;
        int sum=0;
        rep(j,1,n)sum=(sum+qpow(g[j],num[i]))%mod;
        ans=1ll*ans*sum%mod;
    }
    cout<<ans<<endl;
    return 0;
}
```

# 洛谷5200 [USACO19JAN]Sleepy Cow Sorting

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5200)

### 题目背景

USACO 19年一月月赛金组第二题

### 题目描述
Farmer John正在尝试将他的N头奶牛$（1≤N≤10^5）$，方便起见编号为$1…N$，在她们前往牧草地吃早餐之前排好顺序。 当前，这些奶牛以$p_1,p_2,p_3,…,p_N$的顺序排成一行，Farmer John站在奶牛$p_1$前面。他想要重新排列这些奶牛，使得她们的顺序变为$1,2,3,…,N$，奶牛$1$在Farmer John旁边。

今天奶牛们有些困倦，所以任何时刻都只有直接面向Farmer John的奶牛会注意听Farmer John的指令。每一次他可以命令这头奶牛沿着队伍向后移动$k$步，$k$可以是$1$到$N−1$之间的任意数。她经过的$k$头奶牛会向前移动，腾出空间使得她能够插入到队伍中这些奶牛之后的位置。

例如，假设$N=4$，奶牛们开始时是这样的顺序：

FJ: $4, 3, 2, 1$ 唯一注意FJ指令的奶牛是奶牛$4$。当他命令她向队伍后移动$2$步之后，队伍的顺序会变成：

FJ: $3, 2, 4, 1$ 现在唯一注意FJ指令的奶牛是奶牛3，所以第二次他可以给奶牛$3$下命令，如此进行直到奶牛们排好了顺序。

Farmer John急欲完成排序，这样他就可以回到他的农舍里享用他自己的早餐了。请帮助他求出一个操作序列，使得能够用最少的操作次数将奶牛们排好顺序。

### 输入输出格式
#### 输入格式
输入的第一行包含$N$。第二行包含$N$个空格分隔的整数：$p_1,p_2,p_3,…,p_N$，表示奶牛们的起始顺序。

#### 输出格式
输出的第一行包含一个整数$K$，为将奶牛们排好顺序所需的最小操作次数。 第二行包含$K$个空格分隔的整数，$c_1,c_2,…,c_K$，每个数均在$1…N−1$之间。如果第$i$次操作FJ命令面向他的奶牛向队伍后移动$c_i$步，那么$K$次操作过后奶牛们应该排好了顺序。

如果存在多种最优的操作序列，你的程序可以输出其中任何一种。

### 输入输出样例
#### 输入样例#1
```
4
1 2 4 3
```
#### 输出样例#1
```
3
2 2 3
```

## 分析

如果答案是$k$的话，那么意味着最少有$n-k$个元素是在自己正确的位置上

需要往后挪动的距离=当前插入的数的正确位置到上升后缀的最前端的距离+上升后缀里面小于当前数的数的个数

这样的时间复杂度是$O(N^2)$的

可以用树状数组优化到$O(N \log N)$

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
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=1e5+6;
int a[maxn],t[maxn],n;
void modify(int x){for(int i=x;i<=maxn-1;i+=i&(-i))t[i]++;}
inline int query(int x){int re=0;for(int i=x;i;i-=i&(-i))re+=t[i];return re;}
int main(){
    n=read();
    rep(i,1,n)a[i]=read();
    int mx=n+1;
    dep(i,n,1){
        if(a[i]>mx){
            cout<<i<<endl;
            rep(j,1,i){
                cout<<i-j+query(a[j])<<' ';
                modify(a[j]);
            }
            break;
        }
        mx=a[i];
        modify(a[i]);
    }
}
```

# 洛谷5201 [USACO19JAN]Shortcut

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5201)

### 题目背景

USACO 19年一月月赛金组第三题

### 题目描述

每天晚上，Farmer John都会敲响一个巨大的铃铛，召唤他的奶牛们前来牛棚享用晚餐。奶牛们都急切地想要前往牛棚，所以她们都会沿着最短的路径行走。 农场可以描述为N块草地（$1≤N≤10,000$），方便起见编号为$1…N$，牛棚位于草地$1$。草地之间由M条双向的小路连接（$N−1≤M≤50,000$）。每条小路有其通过时间，从每块草地出发都存在一条由一些小路组成的路径可以到达牛棚。

草地$i$中有$c_i$头奶牛。当她们听到晚餐铃时，这些奶牛都沿着一条消耗最少时间的路径前往牛棚。如果有多条路径并列消耗时间最少，奶牛们会选择其中“字典序”最小的路径（也就是说，她们通过在两条路径第一次出现分支的位置优先选择经过编号较小的草地的方式来打破并列关系，所以经过草地$7、3、6、1$的路径会优先于经过$7、5、1$的路径，如果它们所消耗的时间相同）。

Farmer John担心牛棚距离某些草地太远。他计算了每头奶牛路上的时间，将所有奶牛消耗的时间相加，称这个和为总移动时间。他想要通过额外添加一条从牛棚（草地$1$）连接到某块他选择的其他草地的、通过时间为T（$1≤T≤10,000$）的“近道”来尽可能地减少总移动时间。当一头奶牛在她平时前往牛棚的路上偶然看见这条近道时，如果这条近道能够使她更快地到达牛棚，她就会走这条路。否则，一头奶牛会仍然沿着原来的路径行走，即使使用这条近道可能会减少她的移动时间。

请帮助Farmer John求出通过添加一条近道能够达到的总移动时间减少量的最大值。

### 输入输出格式
#### 输入格式

输入的第一行包含$N,M,T$。以下$N$行包含$c_1…c_N$，均为$0…10,000$范围内的整数。以下$M$行，每行由三个整数$a,b$和$t$描述了一条小路，这条小路连接了草地$a$和$b$，通过时间为$t$。所有的通过时间在$1…25,000$范围内。

#### 输出格式

输出Farmer John可以达到的总移动时间的最大减少量。

### 输入输出样例
#### 输入样例#1
```
5 6 2
1 2 3 4 5
1 2 5
1 3 3
2 4 3
3 4 5
4 5 2
3 5 7
```
#### 输出样例#1
``` 
40
```

## 分析

首先预处理出最短路（SPFA/Dijkstra）

顺便可以构造出最短路树

显而易见的一个结论：对于最短路树中的一个节点，在树上朝根节点走，一定是最优方案

之后可以处理出每一个节点经过了多少头牛

考虑每个节点对于答案的更新

$ans=\sum_{i\ in\ path} C[i]\times Val[i]$

其中$Val[i]$表示当前点到根节点的距离减去加入新边的长度

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
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
#define inf 0x3f3f3f
const int maxn=1e4+6,maxm=1e5+6;
struct edge{int to,next,w;}e[maxm<<2];
int n,m,T,dis[maxn],c[maxn],cnt=0,To[maxn],Val[maxn],In[maxn],que[maxm<<2],last[maxn<<1],S=1;
ll ans=0;bool inq[maxn];
void Pre(){rep(i,1,n)dis[i]=1e9;dis[S]=0;}
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
    n=read(),m=read(),T=read();
    rep(i,1,n)c[i]=read();
    rep(i,1,m){
        int u=read(),v=read(),w=read();
        insert(u,v,w);insert(v,u,w);
    }
    Pre();SPFA();
    mem(To,inf);To[1]=1;
    rep(j,2,n){
        reg(j)if(dis[j]==dis[e[i].to]+e[i].w&&To[j]>e[i].to)To[j]=e[i].to,Val[j]=dis[j]-T;
        In[To[j]]++;
    }
    mem(que,0);int head=0,tail=0;
    rep(i,1,n)if(!In[i])que[tail++]=i;
    while(head<tail){
        int now=que[head++];
        ans=max(ans,1ll*Val[now]*c[now]);
        c[To[now]]+=c[now];In[To[now]]--;
        if(!In[To[now]])que[tail++]=To[now];
    }
    cout<<ans<<endl;
    return 0;
}
```