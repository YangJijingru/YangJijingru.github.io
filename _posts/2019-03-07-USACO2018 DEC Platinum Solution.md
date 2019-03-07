---
subtitle: "没见过的套路和结论题"
tags: 
 - 解题报告
 - USACO
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P41.jpg"
preview-img: "/img/preview/P41.jpg"
---

# 洛谷5155 [USACO18DEC]Balance Beam

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5155)

### 题目描述

Bessie为了存钱给她的牛棚新建一间隔间，开始在当地的马戏团里表演，通过在平衡木上小心地来回走动来展示她卓越的平衡能力。

Bessie能够通过表演赚到的钱取决于她最终成功跳下平衡木的位置。平衡木上从左向右的位置记为 $0,1,\ldots,N+1$ 。如果Bessie到达了位置 $0$ 或是 $N+1$ ，她就会从平衡木的一端掉下去，遗憾地得不到报酬。

如果Bessie处在一个给定的位置 $k$ ，她可以进行下面两项中的任意一项：

1. 投掷一枚硬币。如果背面朝上，她前往位置 $k-1$ ，如果正面朝上，她前往位置 $k+1$ （也就是说，每种可能性 $1/2$ 的概率）。

2. 跳下平衡木，获得 $f(k)$ 的报酬（ $0\leq f(k) \leq 10^9$ ）。

Bessie意识到她并不能保证结果能够得到某一特定数量的报酬，这是由于她的移动是由随机的掷硬币结果控制。然而，基于她的起始位置，她想要求出当她进行一系列最优的决定之后，她能够得到的期望报酬（“最优”指的是这些决定能够带来最高可能的期望报酬）。

例如，如果她的策略能够使她以 $1/2$ 的概率获得 $10$ 的报酬，$1/4$ 的概率获得 $8$ 的报酬，$1/8$ 的概率获得 $0$ 的报酬，那么她的期望报酬为加权平均值 $10 \times (1/2)+8 \times (1/4)+0 \times (1/4)=7$ 。

### 输入输出格式
#### 输入格式

输入的第一行包含 $N$ （ $2 \leq N \leq 10^5$ ）。以下 $N$ 行包含 $f(1) \ldots f(N)$ 。

#### 输出格式

输出 $N$ 行。第 $i$ 行输出 $10^5$ 乘以Bessie从位置 $i$ 开始使用最优策略能够获得的报酬的期望值，向下取整。

### 输入输出样例
#### 输入样例#1
```
2
1
3
```
#### 输出样例#1
```
150000
300000
```

## 题意

给出长度为$n$的序列，每次有$\frac 1 2$的概率向左或向右移动一格，也可以选择在此位置退出并得到当前位置的收益，求从每个位置开始的使用最优策略的期望

## 分析

贪心的策略：如果当前移动后的收益期望比当前位置的收益大，那么会选择移动；否则选择直接停止

发现这个和凸包的模型类似

如果该点是凸包上的点，那么答案就是自己，否则用DP往两边寻找答案（找到两边的第一个在凸包上的点即停止）

$f[i]=(f[l[i]])\times (r[i]-i)+f[r[i]]\times (i-l[i])))/(r[i]-l[i])$ 

$l[i],r[i]$分别为第$i$号点左右两边第一个在凸包上的点

## Code
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
const int maxn=1e5+6;
ll f[maxn],l[maxn],r[maxn],p[maxn];
int n,top=0;
int main(){
    n=read();p[++top]=0;
    rep(i,1,n)f[i]=read();
    rep(i,1,n+1){
        while(top>=2){
            int x=p[top],y=p[top-1];
            if((f[x]-f[y])*(i-x)<(f[i]-f[x])*(x-y))top--;
            else break;
        }
        p[++top]=i;
    }
    rep(i,1,top-1){
        rep(j,p[i]+1,p[i+1]-1)l[j]=p[i],r[j]=p[i+1];
        l[p[i]]=p[i],r[p[i]]=p[i];
    }
    rep(i,1,n){
        ll ans=0;
        if(l[i]==r[i])ans=f[i]*1e5;
        else ans=(1e5*(f[l[i]]*(r[i]-i)+f[r[i]]*(i-l[i]))/(r[i]-l[i]));
        printf("%lld\n",ans);
    }
    return 0;
}
```

# 洛谷5156 [USACO18DEC]Sort It Out

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5156)

### 题目描述
FJ有 $N$ （ $1 \leq N \leq 10^5$ ）头奶牛（分别用 $1 \ldots N$ 编号）排成一行。FJ喜欢他的奶牛以升序排列，不幸的是现在她们的顺序被打乱了。在过去FJ曾经使用一些诸如“冒泡排序”的开创性的算法来使他的奶牛排好序，但今天他想偷个懒。取而代之，他会每次对着一头奶牛叫道“按顺序排好”。当一头奶牛被叫到的时候，她会确保自己在队伍中的顺序是正确的（从她的角度看来）。当有一头紧接在她右边的奶牛的编号比她小，她们就交换位置。然后，当有一头紧接在她左边的奶牛的编号比她大，她们就交换位置。这样这头奶牛就完成了“按顺序排好”，在这头奶牛看来左边的奶牛编号比她小，右边的奶牛编号比她大。

FJ想要选出这些奶牛的一个子集，然后遍历这个子集，依次对着每一头奶牛发号施令（按编号递增的顺序），重复这样直到所有N头奶牛排好顺序。例如，如果她选出了编号为 $\{2,4,5\}$ 的奶牛的子集，那么他会喊叫奶牛$2$，然后是奶牛$4$，然后是奶牛$5$。如果 $N$ 头奶牛此时仍未排好顺序他会再次对着这几头奶牛喊叫，如果有必要的话继续重复。

由于FJ不确定哪些奶牛比较专心，他想要使得这个子集最小。此外，他认为 $K$ 是个幸运数字。请帮他求出满足重复喊叫可以使得所有奶牛排好顺序的最小子集之中字典序第 $K$ 小的子集。

我们称 $\{1, \ldots ,N\}$ 的一个子集 $S$ 在字典序下小于子集 $T$ ，当 $S$ 的所有元素组成的序列（按升序排列）在字典序下小于 $T$ 的所有元素组成的序列（按升序排列）。例如， $\{1,3,6\}$ 在字典序下小于 $\{1,4,5\}$ 。

### 输入输出格式
#### 输入格式

输入的第一行包含一个整数 $N$ 。第二行包含一个整数 $K$ （ $1 \leq K \leq 10^{18}$ ）。第三行包含 $N$ 个空格分隔的整数，表示从左到右奶牛的编号。

保证存在至少 $K$ 个符合要求的子集。

#### 输出格式

第一行输出最小子集的大小。接下来输出字典序第 $K$ 小的最小子集中奶牛的编号，每行一个数，升序排列。

### 输入输出样例
#### 输入样例#1
```
4 1
4 2 1 3
```
#### 输出样例#1
```
2
1
4
```
### 说明

开始的时候序列为 $\mathtt{\:4\:\; 2\:\; 1\:\; 3\:}$ 。在FJ喊叫编号为 $1$ 的奶牛之后，序列变为 $\mathtt{\:1\:\; 4\:\; 2\:\; 3\:}$ 。在FJ喊叫编号为 $4$ 的奶牛之后，序列变为 $\mathtt{\:1\:\; 2\:\; 3\:\; 4\:}$ 。在这个时候，序列已经完成了排序。

### 子任务

对于占总分 $3/16$ 的测试数据， $N \leq 6$ ，并且 $K=1$ 。对于另外的占总分 $5/16$ 的测试数据， $K=1$ 。对于另外的占总分 $8/16$ 的测试数据，没有其他限制。

## 题意

给出长度为$n$的序列$a_i \in [1,n]$（也可以说是一个长度为$n$的排列），每次可以将一个该位置权值为$a_i$的数字放到$a_i$位置（详见样例和解释），多次操作后使得序列从小到大排列，请最小化操作次数并输出字典序第$k$小的方案

## 分析

题目描述的操作类似于快速排序，不会改变其它数的相对位置

也就是说不在答案集合内的数字，其相对位置不会发生变化

那么不在答案中的数字必须构成一个上升序列，那么答案就是最长上升子序列（LIS）

现在问题转化为如何求字典序第$k$小的LIS

考虑预处理出来以数字$x$开头的LIS长度和个数

之后用树状数组维护以第i个数字为结尾的最长的LIS长度和方案数即可

或者也可以用Vector或者链表转移（~~什么骚操作~~）

## Code
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
#define inf 1e18
const int maxn=1e5+6;
struct edge{int to,next;}e[maxn<<1];
int a[maxn],last[maxn],n,cnt=0;ll K;bool vis[maxn];
struct node{
    int v;ll c;
    inline node(){}
    friend inline void operator + (node&A,const node B){
        if(A.v<B.v)A.v=B.v,A.c=B.c;
        else if(A.v==B.v)A.c=min((ll)inf,(ll)A.c+B.c);
    }
}d[maxn],g[maxn],t;
inline node query(int x){node re=t;for(int i=x;i<=n+1;i+=i&(-i))re+d[i];return re;}
void modify(int x,node y){for(int i=x;i;i-=i&(-i))d[i]+y;}
void insert(int u,int v){e[++cnt]=(edge){v,last[u]};last[u]=cnt;}
int main(){
    n=read(),K=read();
    rep(i,1,n)a[i]=read();
    t.c=1;modify(n+1,t);t.c=0;
    dep(i,n,1){g[i]=query(a[i]);++g[i].v;modify(a[i],g[i]);}
    dep(i,n,1)insert(g[i].v,i);
    int tmp=query(1).v,R=0;
    dep(j,tmp,1)
        reg(j){
            if(g[e[i].to].c<K)K-=g[e[i].to].c;
            else{
                vis[a[e[i].to]]=1;
                while(R<e[i].to)g[R++]=t;
                break;
            }
        }
    printf("%d\n",n-query(1).v);
    rep(i,1,n)if(!vis[i])printf("%d\n",i);
    return 0;
}
```

# 洛谷5157 [USACO18DEC]The Cow Gathering

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5157)

### 题目描述

奶牛们从世界各地聚集起来参加一场大型聚会。总共有 $N$ 头奶牛， $N-1$ 对奶牛互为朋友。每头奶牛都可以通过一些朋友关系认识其他每头奶牛。

她们玩得很开心，但是现在到了她们应当离开的时间了，她们会一个接一个地离开。她们想要以某种顺序离开，使得只要至少还有两头奶牛尚未离开，所有尚未离开的奶牛都还有没有离开的朋友。此外，由于行李寄存的因素，有 $M$ 对奶牛 $(a_i,b_i)$ 必须满足奶牛 $a_i$ 要比奶牛 $b_i$ 先离开。注意奶牛 $a_i$ 和奶牛 $b_i$ 可能是朋友，也可能不是朋友。

帮助奶牛们求出，对于每一头奶牛，她是否可以成为最后一头离开的奶牛。可能会发生不存在满足上述要求的奶牛离开顺序的情况。

### 输入输出格式
#### 输入格式

输入的第 $1$ 行包含两个空格分隔的整数 $N$ 和 $M$ 。

输入的第 $2 \leq i \leq N$ 行，每行包含两个整数 $x_i$ 和 $y_i$ ，满足 $1 \leq x_i$ ， $y_i \leq N,xi \neq yi$ ，表示奶牛 $x_i$ 和奶牛 $y_i$ 是朋友关系。

输入的第 $N+1 \leq i \leq N+M$ 行，每行包含两个整数 $a_i$ 和 $b_i$ ，满足 $1 \leq a_i,b_i \leq N$ ， $a_i \neq b_i$ ，表示奶牛 $a_i$ 必须比奶牛 $b_i$ 先离开聚会。

输入保证 $1 \leq N,M \leq 10^5$ 。对于占总分 $20%$ 的测试数据，保证 $N,M \leq 3000$ 。

#### 输出格式
输出 $N$ 行，每行包含一个整数 $d_i$ ，如果奶牛 $i$ 可以成为最后一头离开的奶牛，则 $d_i=1$ ，否则 $d_i=0$ 。

### 输入输出样例
#### 输入样例#1
```
5 1
1 2
2 3
3 4
4 5
2 4
```
#### 输出样例#1
```
0
0
1
1
1
```
## 题意

给定大小为$N$的树和树上的$N-1$条有向边，

同时给出$M$个限制$(u,v)$，$u$必须在$v$之前被删除

每次删去叶子，询问每个结点能否最后一个删去

## 分析

当我们新加入一条边$(u,v)$，显然以$v$为根时$u$的子树都无法最后删除

根据这个结论，每次在加入新边后，一些点已经不可能成为最后删除的点了

------

每次判断一个点是否可行，只要将它提为$root$，并将所有边记为子节点指向父节点

以$root$为根，可以发现任意一个限制$(u,v)$中, $u$的子树一定是不可行的

同样一个结论：每个点要么可行要么不可行，标记完不可行点之后剩下的点都是可行的（可以利用数学归纳法证明）

那么可以发现所有的可行解必定都是在一起的, 跑一遍dfs即可

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<vector>
#include<queue>
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
#define pb push_back
const int maxn=1e5+6;
int n,m,root,du[maxn],vis[maxn],ans[maxn],tot;
vector <int> g[maxn],l[maxn];
queue <int> que;
int solve(){
    tot=0;
    while(!que.empty()){
        int now=que.front();que.pop();
        tot++;
        if(du[now]!=1){
            if(du[now]<1)return now;
            rep(i,1,n)puts("0");exit(0);
        } 
        for(auto v:g[now]){
            du[v]--;tot++;
            if(du[v]==1)que.push(v);
        }
        for(auto v:l[now]) {
            du[v]--;tot++;
            if(du[v]==1)que.push(v);
        }
    }
    if(tot!=n){rep(i,1,n)puts("0");exit(0);}
}
void dfs(int x,int fa){
    ans[x]=1;
    rep(i,0,g[x].size()-1){
        if(g[x][i]==fa)continue;
        if(!vis[g[x][i]])dfs(g[x][i],x);
    }
}
int main(){
    n=read(),m=read();
    rep(i,1,n-1){
        int u=read(),v=read();
        g[u].pb(v),g[v].pb(u);
        du[u]++,du[v]++;
    }
    rep(i,1,m){
        int a=read(),b=read();
        l[a].pb(b);du[b]++;vis[a]=1;
    }
    rep(i,1,n)if(du[i]==1)que.push(i);
    root=solve();
    dfs(root,-1);
    rep(i,1,n)printf("%d\n",ans[i]);
    return 0;
}
```