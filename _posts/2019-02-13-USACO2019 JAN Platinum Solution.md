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

# 洛谷5203 [USACO19JAN]Exercise Route

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5203)

### 题目背景

USACO 19年一月月赛铂金组第二题

### 题目描述

奶牛Bessie意识到为了保持好的体形她需要更多地进行锻炼。她需要你帮助她选择在农场里每天用来晨跑的路线。
农场由$N$块草地组成（$1\leq N\leq 2\times 10^5$），方便起见编号为$1…N$，由$M$条双向的小路连接（$1\leq M\leq 2\times 10^5$）。作为一种遵循规律的生物，奶牛们倾向于使用其中特定的$N−1$条小路作为她们日常在草地之间移动的路线——她们管这些叫“常规的”小路。从每块草地出发都可以仅通过常规的小路到达所有其他草地。

为了使她的晨跑更加有趣，Bessie觉得她应该选择一条包含一些非常规的小路的路线。然而，使用常规的小路能够使她感到舒适，所以她不是很想在她的路线中使用过多非常规的小路。经过一些思考，她认为一条好的路线应当形成一个简单环（能够不经过任何草地超过一次的情况下回到起点），其中包含恰好两条非常规的小路。

请帮助Bessie计算她可以使用的好的路线的数量。两条路线被认为是相同的，如果它们包含的小路的集合相等。

### 输入输出格式
#### 输入格式

输入的第一行包含N和M。以下M行每行包含两个整数$a_i$和$b_i$，描述了一条小路的两端。其中前$N−1$条是常规的小路。

#### 输出格式

输出Bessie可以选择的路线的总数。

### 输入输出样例

#### 输入样例#1
```
5 8
1 2
1 3
1 4
1 5
2 3
3 4
4 5
5 2
```
#### 输出样例#1
```
4
```
## 分析

因为满足题目要求的两条跑步路线必须要包含两条“非标准”边，发现当且仅当两条“非标准”边上的节点在树中的路径存在交集时才会出现环

转化为子问题：如何判断两段路径是否存在交集（重叠）

对于在树上的计数问题，可以把原来的路径拆分为$(x->lca(x,y)),(lca(x,y)->y)$，把每一条边和它向下对应的点绑定在一起，于是答案的统计变成了（开始在$x/y$节点的相交个数-开始在$lca(x,y)$节点的相交个数）

注意到一条路径可能存在两条深度最小的边，这种情况会被上述算法统计两次，因此我们需要将其多余计算的部分去除

即在两条路径的lca处统计答案，使用Map去重

时间复杂度$O(n log n)$


## Code

```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
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
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=2e5+6;
struct edge{int to,next;}e[maxn<<1];
int n,m,last[maxn],dep[maxn],fa[maxn][20],u[maxn],v[maxn],lca[maxn],s[maxn],sum[maxn],cnt=0;
ll ans=0;
map<pair<int,int>,int>mp;
void insert(int u,int v){e[++cnt]={v,last[u]};last[u]=cnt;}
void dfs(int x,int fr){
    fa[x][0]=fr;
    rep(i,1,18)fa[x][i]=fa[fa[x][i-1]][i-1];
    reg(x){
        if(e[i].to==fr)continue;
        dep[e[i].to]=dep[x]+1;
        dfs(e[i].to,x);
    }
}
inline int getlca(int x,int y){
    if(dep[x]<dep[y])swap(x,y);
    int tmp=dep[x]-dep[y];
    rep(i,0,18)if((1<<i)&tmp)x=fa[x][i];
    dep(i,18,0)
        if(fa[x][i]!=fa[y][i])x=fa[x][i],y=fa[y][i];
    return x==y?x:fa[x][0];
}
inline int son(int lca,int x){
    if(lca==x)return 0;
    dep(i,18,0)if(dep[fa[x][i]]>dep[lca])x=fa[x][i];
    return x;
}
void dfs2(int x,int tmp){
    sum[x]=tmp;
    reg(x){
        if(e[i].to==fa[x][0])continue;
        dfs2(e[i].to,tmp+s[e[i].to]);
    }
}
int main(){
    n=read(),m=read();
    rep(i,1,n-1){
        int u=read(),v=read();
        insert(u,v);insert(v,u);
    }
    dfs(1,1);
    rep(i,n,m){
        u[i]=read(),v[i]=read();
        lca[i]=getlca(u[i],v[i]);
        int r1=son(lca[i],u[i]),r2=son(lca[i],v[i]);
        if(r1)ans-=++s[r1];
        if(r2)ans-=++s[r2];
        if(r1&&r2){
            if(r1>r2)swap(r1,r2);
            ans-=mp[make_pair(r1,r2)];
            ++mp[make_pair(r1,r2)];
        }
    }
    dfs2(1,s[1]);
    rep(i,n,m)ans+=sum[u[i]]+sum[v[i]]-2*sum[lca[i]];
    cout<<ans<<endl;
    return 0;
}
```

# 洛谷5204 [USACO19JAN]Train Tracking 2

## 题目

[题目传送门](https://www.luogu.org/problemnew/show/P5204)

### 题目背景
USACO 19年一月月赛铂金组第三题

### 题目描述
每天特快列车都会经过农场。列车有N节车厢（$1≤N≤10^5$），每节车厢上有一个$1$到$10^9$之间的正整数编号；不同的车厢可能会有相同的编号。
平时，Bessie会观察驶过的列车，记录车厢的编号。但是今天雾实在太浓了，Bessie一个编号也看不见！幸运的是，她从城市里某个可靠的信息源获知了列车编号序列的所有滑动窗口中的最小值。具体地说，她得到了一个正整数$K$，以及$N−K+1$个正整数$c_1,…,c_{N+1−K}$，其中$c_i$是车厢$i,i+1,…,i+K−1$之中编号的最小值。

帮助Bessie求出满足所有滑动窗口最小值的对每节车厢进行编号的方法数量。由于这个数字可能非常大，只要你求出这个数字对$10^9+7$取余的结果Bessie就满意了。

Bessie的消息是完全可靠的；也就是说，保证存在至少一种符合要求的编号方式。

### 输入输出格式

#### 输入格式
输入的第一行包含两个空格分隔的整数N和K。余下的行包含所有的滑动窗口最小值$c_1,…,c_{N+1−K}$，每行一个数。

#### 输出格式
输出一个整数：对每节车厢给予一个不超过$10^9$的正整数编号的方法数量对$10^9+7$取余的结果，满足车厢$i,i+1,…,i+K−1$之中编号的最小值等于$c_i$，对于$1≤i≤N−K+1$均成立。


### 输入输出样例

#### 输入样例#1
```
4 2
999999998
999999999
999999998
```
#### 输出样例#1
```
3
```
## 题意

给出一个长度为$n$的数列$a_i$的$n-k+1$个长度为$k$的子区间最小值$c_j$，即$c_j=\min_{i=j}^{j+k-1} a_i$，求满足$c_i$序列的$a_i$序列方案个数

## 分析

原问题转化为在数列上填充数字，每连续的$k$格中必须有一个是$c$的方案数

xzz的题解太神仙了tql

------

下面是一种被吊打的解法

当滑窗向左或者向右移动一个单位时，如果最大值或者最小值发生变化，那么我们可以确定一个值了

所以可以通过一些滑窗值的变化，来先确定部分值

对于不确定的位置，需要用DP解决

那么子问题就变成了如何计算一个长度为$n$的序列，使得其中长度为$k$的滑窗内任意一段中的最小值都小于$V$

设$f[i]$为以$V$结尾，长度为$i$的滑窗序列方案数

转移的时候枚举上一次$V$出现的位置

$f[i]=f[i-1]+f[i-2]\times x+f[i-3]\times x^2+...+f[i-k]\times x^{k-1}$

把式子变形后

$f[i]=f[i-1]+x(f[i-1]-f[i-k-1]\times x^{k-1})$

总时间复杂度为$O(n)$

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
const ll inf=1e9,mod=1e9+7;
const int maxn=2e5+6;
int n,k,c[maxn],tot=0,cur=0,cnt=0;ll ans=1;
struct node{int f;ll s;}comp[maxn];
inline ll Count(int mnval,int k,int len){
    ll f[len+6],pows[k+6],powslow[k+6],succ=inf-mnval;
    pows[0]=powslow[0]=1;
    rep(i,1,k){
        pows[i]=(1ll*pows[i-1]*(succ+1)%mod+mod)%mod;
        powslow[i]=(1ll*powslow[i-1]*succ%mod+mod)%mod;
    }
    f[0]=f[1]=1;
    rep(i,2,min(k,len))f[i]=pows[i-1];
    if(len<k)return pows[len];
    rep(i,k,len){
        f[i+1]=f[i];
        f[i+1]=(ll)((f[i+1]-(1ll*f[i-k]*powslow[k-1])+mod)%mod+mod)%mod;
        f[i+1]=(ll)(1ll*f[i+1]*succ)%mod;
        f[i+1]=(ll)(f[i+1]+f[i])%mod;
    }
    return f[len+1]%mod;
}
int main(){
    n=read(),k=read();
    rep(i,1,n-k+1)c[i]=read();
    rep(i,1,n-k+1){
        if(cur==c[i])++cnt;
        else{
            if(cnt)comp[++tot]=(node){cur,cnt};
            cur=c[i],cnt=1;
        }
    }
    comp[++tot]=(node){cur,cnt};
    if(tot==1){cout<<Count(comp[1].f,k,n)<<endl;return 0;}
    rep(i,1,tot){
        int a=comp[i].f,len;
        if(i==1){
            if(comp[2].f>comp[1].f)len=comp[1].s-1;else len=comp[1].s+k-1;
        }
        else if(i==tot){
            if(comp[i-1].f>comp[i].f)len=comp[i].s-1;else len=comp[i].s+k-1;
        }
        else{
            if(comp[i-1].f>comp[i].f){
                if(comp[i+1].f>comp[i].f)len=max(0,(int)comp[i].s-k-1);
                else len=comp[i].s-1;
            }else{
                if(comp[i+1].f>comp[i].f)len=comp[i].s-1;
                else len=comp[i].s+k-1;
            }
        }
        ans=(ll)((1ll*ans*Count(a,k,len))%mod+mod)%mod;
    }
    cout<<ans<<endl;
    return 0;
}
```