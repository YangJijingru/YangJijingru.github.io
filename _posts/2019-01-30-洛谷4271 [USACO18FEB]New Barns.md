---
subtitle: "奶牛倍增LCA题"
tags: 
 - 树-LCA
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P51.jpg"
preview-img: "/img/preview/P51.jpg"
---
标签：LCA

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P4271)

## 题意翻译
Farmer John注意到他的奶牛们如果被关得太紧就容易吵架，所以他想开放一些新的牛棚来分散她们。
每当FJ建造一个新牛棚的时候，他会将这个牛棚用至多一条双向道路与一个现有的牛棚连接起来。为了确保他的奶牛们足够分散，他有时想要确定从某个特定的牛棚出发，到它能够到达的最远的牛棚的距离（两个牛棚之间的距离等于从一个牛棚出发到另一个之间必须经过的道路条数）。

FJ总共会给出$Q$（$1 \leq Q \leq 10^5$）次请求，每个请求都是“建造”和“距离”之一。对于一个建造请求，FJ建造一个牛棚，并将其与至多一个现有的牛棚连接起来。对于一个距离请求，FJ向你询问从某个特定的牛棚通过一些道路到离它最远的牛棚的距离。保证询问的牛棚已经被建造。请帮助FJ响应所有的请求。

输入格式（文件名：newbarn.in）：
第一行包含一个整数$Q$。以下$Q$行，每行包含一个请求。每个请求的格式都是“B p”或是“Q k”，分别告诉你建造一个牛棚并与牛棚$p$连接，或是根据定义求从牛棚$k$出发最远的距离。如果$p = -1$，则新的牛棚不会与其他牛棚连接。否则，$p$是一个已经建造的牛棚的编号。牛棚编号从$1$开始，所以第一个被建造的谷仓是$1$号谷仓，第二个是$2$号谷仓，以此类推。
输出格式（文件名：newbarn.out）：
对于每个距离请求输出一行。注意一个没有连接到其他牛棚的牛棚的最远距离为$0$。

## 题目描述
Farmer John notices that his cows tend to get into arguments if they are packed too closely together, so he wants to open a series of new barns to help spread them out.
Whenever FJ constructs a new barn, he connects it with at most one bidirectional pathway to an existing barn. In order to make sure his cows are spread sufficiently far apart, he sometimes wants to determine the distance from a certain barn to the farthest possible barn reachable from it (the distance between two barns is the number of paths one must traverse to go from one barn to the other).

FJ will give a total of $Q$ ($1 \leq Q \leq 10^5$) queries, each either of the form "build" or "distance". For a build query, FJ builds a barn and links it with at most one previously built barn. For a distance query, FJ asks you the distance from a certain barn to the farthest barn reachable from it via a series of pathways. It is guaranteed that the queried barn has already been built. Please help FJ answer all of these queries.

## 输入输出格式
### 输入格式
The first line contains the integer $Q$. Each of the next $Q$ lines contains a query. Each query is of the form "B p" or "Q k", respectively telling you to build a barn and connect it with barn $p$, or give the farthest distance, as defined, from barn $k$. If $p = -1$, then the new barn will be connected to no other barn. Otherwise, $p$ is the index of a barn that has already been built. The barn indices start from $1$, so the first barn built is barn $1$, the second is barn $2$, and so on.

### 输出格式
Please write one line of output for each distance query. Note that a barn which is connected to no other barns has farthest distance $0$.

## 输入输出样例
### 输入样例#1
```
7
B -1
Q 1
B 1
B 2
Q 3
B 2
Q 2
```
### 输出样例#1
```
0
2
1
```
## 说明
The example input corresponds to this network of barns:
```
  (1) 
    \   
     (2)---(4)
    /
  (3)
```
In query 1, we build barn number 1. In query 2, we ask for the distance of 1 to the farthest connected barn. Since barn 1 is connected to no other barns, the answer is 0. In query 3, we build barn number 2 and connect it to barn 1. In query 4, we build barn number 3 and connect it to barn 2. In query 5, we ask for the distance of 3 to the farthest connected barn. In this case, the farthest is barn 1, which is 2 units away. In query 6, we build barn number 4 and connect it to barn 2. In query 7, we ask for the distance of 2 to the farthest connected barn. All three barns 1, 3, 4 are the same distance away, which is 1, so this is our answer.

Problem credits: Anson Hu

## 题解

可以维护树的直径（树上离一个叶子节点最远的叶子节点一定是一条直径的端点）

在直径为$(x,y)$的树上加入一个叶子结点$z$，则新的直径必然为$(x,y),(x,z),(y,z)$中的一条

那么问题转化为询问树上两点距离，倍增LCA即可

时间复杂度$O(M \log N)$

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
const int maxn=1e5+6;
int Q,num,tot,x,dep[maxn],tx[maxn],ty[maxn],bel[maxn],fa[maxn][20];
inline int lca(int x,int y){
    if(dep[x]<dep[y])swap(x,y);
    int tmp=dep[x]-dep[y];
    rep(i,0,16)if((1<<i)&tmp)x=fa[x][i];
    dep(i,16,0)if(fa[x][i]!=fa[y][i])x=fa[x][i],y=fa[y][i];
    if(x==y)return x;else return fa[x][0];
}
inline int dist(int x,int y){return dep[x]+dep[y]-2*dep[lca(x,y)];}
void update(int x){
    int tmp=bel[x],nx,ny,nd;
    if(dist(x,tx[tmp])>dist(x,ty[tmp])){
        nd=dist(x,tx[tmp]);
        nx=x,ny=tx[tmp];
    }else{
        nd=dist(x,ty[tmp]);
        nx=x,ny=ty[tmp];
    }
    if(nd>dist(tx[tmp],ty[tmp]))tx[tmp]=nx,ty[tmp]=ny;
}
inline int query(int x){return max(dist(x,tx[bel[x]]),dist(x,ty[bel[x]]));}
int main(){
    Q=read();
    while(Q--){
        char ch=getchar();
        int x=read();
        if(ch=='B'){
            ++num;
            if(x==-1){
                bel[num]=++tot;
                fa[num][0]=0;
                dep[num]=1;
                tx[tot]=ty[tot]=num;
            }else{
                dep[num]=dep[x]+1;
                fa[num][0]=x;
                rep(i,1,17)fa[num][i]=fa[fa[num][i-1]][i-1];
                bel[num]=bel[x];
            }
            update(num);        
        }else printf("%d\n",query(x));
    }
    return 0;
}
```