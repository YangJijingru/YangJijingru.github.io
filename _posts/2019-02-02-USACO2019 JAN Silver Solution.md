---
subtitle: "银组水题拼盘"
tags: 
 - 解题报告
 - USACO
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P50.jpg"
preview-img: "/img/preview/P50.jpg"
categories: OI
---
# 洛谷5197 [USACO19JAN]Grass Planting

## 题目

### 题目背景
USACO一月月赛银组第一题

### 题目描述
到了一年中Farmer John在他的草地里种草的时间了。整个农场由N块草地组成（$1≤N≤10^5$），方便起见编号为$1…N$，由$N−1$条双向的小路连接，每块草地都可以经过一些小路到达其他所有的草地。
Farmer John当然可以在每块草地里种不同种类的草，但是他想要使得使用的草的种类数最小，因为他用的草的种类数越多，他就需要负担更高的花费。

不幸的是，他的奶牛们对选择农场上的草表现得十分苛刻。如果两块相邻（由一条小路直接相连）的草地种了同一种草，或者即使是两块接近相邻（均可由一条小路直接连向同一块草地）的草地，那么奶牛们就会抱怨她们进餐的选择不够多样。Farmer John能做的只能是抱怨这些奶牛，因为他知道她们不能被满足的时候会制造多大的麻烦。

请帮助Farmer John求出他的整个农场所需要的最少的草的种类数。

### 输入输出格式
#### 输入格式
输入的第一行包含$N$。以下$N−1$行每行描述了一条小路连接的两块草地。

#### 输出格式
输出Farmer John需要使用的最少的草的种类数

### 输入输出样例
#### 输入样例#1
```
4
1 2
4 3
2 3
```
#### 输出样例#1
```
3
```
### 说明
在这个简单的例子中，$4$块草地以一条直线的形式相连。最少需要三种草。例如，Farmer John可以用草A，B和C将草地按A - B - C - A的方式播种。

## 题解

$ans=\max_{i=1}^n depth[i] +1$

对于任意一个节点，其度数$dep[i]$表示$i$周围的节点需要用这么多不同种类的草来播种

而其本身也需要另外一种

所以即可在所有节点中寻找度数最大的节点

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
int n,ans=0,dep[maxn];
int main(){
    n=read();
    rep(i,1,n-1){
        int x=read(),y=read();
        ans=max(ans,max(++dep[x],++dep[y]));
    }
    cout<<ans+1<<endl;
    return 0;
}
```

# 洛谷5198 [USACO19JAN]Icy Perimeter

## 题目

### 题目背景
USACO一月月赛银组第二题

### 题目描述
Farmer John要开始他的冰激凌生意了！他制造了一台可以生产冰激凌球的机器，然而不幸的是形状不太规则，所以他现在希望优化一下这台机器，使其产出的冰激凌球的形状更加合理。
机器生产出的冰激凌的形状可以用一个$N×N（1≤N≤1000）$的矩形图案表示，例如：

```
##....
....#.
.#..#.
.#####
...###
....##
```

每个'.'字符表示空的区域，每个'#'字符表示一块$1×1$的正方形格子大小的冰激凌。

不幸的是，机器当前工作得并不是很正常，可能会生产出多个互不相连的冰激凌球（上图中有两个）。一个冰激凌球是连通的，如果其中每个冰激凌的正方形格子都可以从这个冰激凌球中其他所有的冰激凌格子出发重复地前往东、南、西、北四个方向上相邻的冰激凌格子所到达。

Farmer John想要求出他的面积最大的冰激凌球的面积和周长。冰激凌球的面积就是这个冰激凌球中'#'的数量。如果有多个冰激凌球并列面积最大，他想要知道其中周长最小的冰激凌球的周长。在上图中，小的冰激凌球的面积为$2$，周长为$6$，大的冰激凌球的面积为$13$，周长为$22$。

注意一个冰激凌球可能在中间有“洞”（由冰激凌包围着的空的区域）。如果这样，洞的边界同样计入冰激凌球的周长。冰激凌球也可能出现在被其他冰激凌球包围的区域内，在这种情况下它们计为不同的冰激凌球。例如，以下这种情况包括一个面积为1的冰激凌球，被包围在一个面积为$16$的冰激凌球内：
```
#####
#...#
#.#.#
#...#
#####
```
同时求得冰激凌球的面积和周长十分重要，因为Farmer John最终想要最小化周长与面积的比值，他称这是他的冰激凌的“冰周率”。当这个比率较小的时候，冰激凌化得比较慢，因为此时冰激凌单位质量的表面积较小。

### 输入输出格式
#### 输入格式
输入的第一行包含$N$，以下$N$行描述了机器的生产结果。其中至少出现一个'#'字符。

#### 输出格式
输出一行，包含两个空格分隔的整数，第一个数为最大的冰激凌球的面积，第二个数为它的周长。如果多个冰激凌球并列面积最大，输出其中周长最小的那一个的信息。

### 输入输出样例
#### 输入样例#1
```
6
##....
....#.
.#..#.
.#####
...###
....##
```
#### 输出样例#1
```
13 22
```

## 题解

题意大概是找出一个矩阵，使得周长和面积的比值最小

可以使用DFS/BFS搜索

也可以使用并查集加速（貌似略微快一点）

对于并查集的方法

首先将每块的面积都初始化为1，周长初始化为4，之后在搜索的过程中，判断其四周的四个方块是否和它处于同一个集合，如果处于相同集合，那么该方块的周长贡献减一

其余细节可以详见代码

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
const int maxn=1e3+6,Maxn=1e6+6;
const int dx[4]={0,-1,0,1},dy[4]={-1,0,1,0};
int n,num,Map[maxn][maxn],Edge[Maxn],fa[Maxn],Area[Maxn],mxArea,mnEdge=2e9;
char ch;
inline int find(int x){return x==fa[x]?x:fa[x]=find(fa[x]);}
void merge(int x,int y){fa[find(x)]=find(y);}
int main(){
    n=read();
    rep(i,1,n){
        rep(j,1,n){
            cin>>ch;
            if(ch=='#')Map[i][j]=++num;
        }
    }
    rep(i,1,n*n)fa[i]=i,Area[i]=1;
    rep(i,1,n)rep(j,1,n){
        if(Map[i][j]==0)continue;
        rep(k,0,1){
            if(Map[i+dx[k]][j+dy[k]]==0)continue;
            int r1=find(Map[i+dx[k]][j+dy[k]]),r2=find(Map[i][j]);
            if(r1!=r2){
                Area[r2]+=Area[r1];
                Edge[r2]+=Edge[r1];
                merge(Map[i+dx[k]][j+dy[k]],Map[i][j]);
            }
        }
        int edgex=4,root=find(Map[i][j]);
        rep(k,0,3)if(Map[i+dx[k]][j+dy[k]])edgex--;
        Edge[root]+=edgex;
        if(mxArea<Area[root])mxArea=Area[root],mnEdge=Edge[root];
        else if(mxArea==Area[root])mnEdge=min(Edge[root],mnEdge);
    }
    cout<<mxArea<<' '<<mnEdge<<endl;
    return 0;
}
```

# 洛谷5199 [USACO19JAN]Mountain View

## 题目

### 题目背景
USACO19年一月月赛银组第三题

### 题目描述
从农场里奶牛Bessie的牧草地向远端眺望，可以看到巍峨壮丽的山脉绵延在地平线上。山脉里由N座山峰$（1≤N≤10^5）$。如果我们把Bessie的视野想象成xy平面，那么每座山峰都是一个底边在x轴上的三角形。山峰的两腰均与底边成45度角，所以山峰的峰顶是一个直角。于是山峰i可以由它的峰顶坐标$(xi,yi)$精确描述。没有两座山峰有完全相同的峰顶坐标。
Bessie尝试数清所有的山峰，然而由于它们几乎是相同的颜色，所以如果一座山峰的峰顶在另一座山峰的三角形区域的边界上或是内部，她就无法看清。

请求出Bessie能够看见的不同的山峰的峰顶的数量，也就是山峰的数量。

### 输入输出格式
#### 输入格式

输入的第一行包含N。以下N行每行包含$x_i（0≤x_i≤10^9）$和$y_i（1≤y_i≤10^9）$，描述一座山峰的峰顶的坐标。

#### 输出格式

输出Bessie能够分辨出的山峰的数量。

### 输入输出样例
#### 输入样例#1
```
3
4 6
7 2
2 5
```
#### 输出样例#1
```
2
```
### 说明

在这个例子中，Bessie能够看见第一座和最后一座山峰。第二座山峰被第一座山峰掩盖了。

## 题解

对于子问题：一个山峰是否会被另一个山峰覆盖

只需要考虑两个山峰的底角位置（因为是等腰三角形）

如果一个山峰的底角右边界远于另一个山峰的右边界，那么这个山峰就不会被其遮挡

对于山峰底角左边界相同的，可以考虑较高的一个

首先以左端点为关键字进行排序，之后维护一个最小左端点和最大右端点

时间复杂度$O(N)$

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
int n,ans,ml=1e9,mr=0;
struct node{int x,y,l,r;}a[maxn<<1];
inline bool cmp(node a,node b){return a.l==b.l?a.y>b.y:a.l<b.l;}
int main(){
    ans=n=read();
    rep(i,1,n)a[i].x=read(),a[i].y=read(),a[i].l=a[i].x-a[i].y,a[i].r=a[i].x+a[i].y;
    sort(a+1,a+1+n,cmp);
    rep(i,1,n){
        if(a[i].l>=ml&&a[i].r<=mr)ans--;
        ml=min(ml,a[i].l);mr=max(mr,a[i].r);
    }
    cout<<ans<<endl;
    return 0;
}
```