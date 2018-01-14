---
tags: 
 - 最短路
 - dijkstra
 - 并查集
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P12.jpg"
preview-img: "/img/preview/P32.jpg"
---
标签：最短路，并查集

# 题目

[题目传送门](http://www.lydsy.com/JudgeOnline/problem.php?id=1576)

Description

![这里写图片描述](http://www.lydsy.com/JudgeOnline/upload/201406/11%283%29.jpg)

Input

* 第一行: 两个空格分开的数, N和M

* 第2..M+1行: 三个空格分开的数a_i, b_i,和t_i
Output

* 第1..N-1行: 第i行包含一个数:从牛棚_1到牛棚_i+1并且避免从牛棚1到牛棚i+1最短路经上最后一条牛路的最少的时间.如果这样的路经不存在,输出-1.
Sample Input
4 5

1 2 2

1 3 2

3 4 4

3 2 1

2 4 3



输入解释:



跟题中例子相同



Sample Output
3

3

6



输出解释:



跟题中例子相同

# 分析

首先你需要了解什么是最短路径树：

对于此图而言，从1号点到所有点的最短路径，所有的最短路径拿出来后并起来就是最短路径树

对于一条非最短路径树内的有向边，长度为w，它能使uv以上，lca以下的点多一种路径，长度为dis[u]+dis[v]+e[i].w-dis[x]

我们令这条非树边的值为val[i]=dis[u]+dis[v]+e[i].w

所以我们只需要对每个x求出最小的val，先将val排序，然后用并查集压缩路径，记录在ans数组中

# code

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
#define inf 0x3f3f3f
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=2e5+6;
int n,m,last[maxn],cnt=0;
int dis[maxn],fa[maxn],f[maxn],tot=0,num=0,ans[maxn];
priority_queue < pair<int,int>,vector<pair<int,int > >,greater<pair<int,int > > > que;
struct edge{int to,next,w,from;}e[maxn<<2];
struct node{
	int u,v,w;
	bool operator < (const node &A ) const {return w<A.w;}
}road[maxn];
void insert(int u,int v,int w){
	e[++cnt]=(edge){v,last[u],w,u};last[u]=cnt;
	e[++cnt]=(edge){u,last[v],w,v};last[v]=cnt;
}

void dijkstra(int S){//用堆优化的dijkstra
	mem(dis,inf);
	que.push(make_pair(0,S));dis[S]=0;
	while(!que.empty()){
		int now=que.top().second,dist=que.top().first;
		que.pop();
		if(dist>dis[now])continue;
		reg(now)
			if(dist+e[i].w<dis[e[i].to]){
				dis[e[i].to]=dist+e[i].w;
				fa[e[i].to]=now;
				que.push(make_pair(dis[e[i].to],e[i].to));
			}
	}
}

int find(int x){return !f[x]?x:f[x]=find(f[x]);}

void add(node x){//寻找val的最小值（即为ans）
	int u=x.u,v=x.v,w=x.w;
	while(find(u)!=find(v)){
		num++;
		if(dis[find(u)]<dis[find(v)])swap(u,v);
		ans[find(u)]=min(ans[find(u)],w-dis[find(u)]);
		u=f[find(u)]=fa[find(u)];
	}
}
int main()
{
	n=read(),m=read();
	rep(i,1,m){
		int u=read(),v=read(),w=read();
		insert(u,v,w);
	}
	dijkstra(1);
	rep(i,1,cnt){
		if(i%2==1)continue;
		int u=e[i].from,v=e[i].to;
		if(fa[u]!=v&&fa[v]!=u)road[++tot]=(node){u,v,dis[u]+dis[v]+e[i].w};
	}
	sort(road+1,road+1+tot);
	rep(i,1,n+6)ans[i]=inf;
	rep(i,1,tot){if(num>=n-1)break;add(road[i]);}
	rep(i,2,n)printf("%d\n",ans[i]==inf?-1:ans[i]);
	return 0;
}

```

