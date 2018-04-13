---
subtitle: "根号分治的思想优化树剖"
tags: 
 - 特殊-根号分治
 - 数据结构-树链剖分
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P69.jpg"
preview-img: "/img/preview/P69.jpg"
---
标签：树链剖分，根号分治

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=4381)

## Description

给定一棵n个点的树，树上每条边的长度都为1，第i个点的权值为a[i]。

Byteasar想要走遍这整棵树，他会按照某个1到n的全排列b走n-1次，第i次他会从b[i]点走到b[i+1]点，并且这一次的步伐大小为c[i]。

对于一次行走，假设起点为x，终点为y，步伐为k，那么Byteasar会从x开始，每步往前走k步，如果最后不足k步就能到达y，那么他会一步走到y。

请帮助Byteasar统计出每一次行走时经过的所有点的权值和。

## Input

第一行包含一个正整数n(2<=n<=50000)。表示节点的个数。

第二行包含n个正整数，其中第i个数为a[i](1<=a[i]<=10000)，分别表示每个点的权值。

接下来n-1行，每行包含两个正整数u,v(1<=u,v<=n)，表示u与v之间有一条边。

接下来一行包含n个互不相同的正整数，其中第i个数为b[i](1<=b[i]<=n)，表示行走路线。

接下来一行包含n-1个正整数，其中第i个数为c[i](1<=c[i]<n)，表示每次行走的步伐大小。

## Output

包含n-1行，每行一个正整数，依次输出每次行走时经过的所有点的权值和

## Sample Input
```
5
1 2 3 4 5
1 2
2 3
3 4
3 5
4 1 5 2 3
1 3 1 1
```
## Sample Output
```
10
6
10
5
```

# 分析

一眼就看到树链剖分

然而没想到用分治的思想

---

将步伐$$\leq \sqrt n$$的预处理出s[i][x]表示x不断向上走i步经过的点的和，然后直接$$O(1)$$查询

否则树链剖分在重链上面走，暴力查询

时间复杂度$$O(\sqrt n+m \log_2 n+m \sqrt n)$$

# code
```
#include<iostream>
#include<cstdlib>
#include<cstdio>
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
const int maxn=5e4+6;
int n,blo,cnt,dfsclk,a[maxn],b[maxn],fa[maxn],dep[maxn],sz[maxn],s[maxn][206],son[maxn],anc[maxn];
int pos[maxn],id[maxn],last[maxn];
struct edge{int to,next;}e[maxn<<1];
void insert(int u,int v){
	e[++cnt]=(edge){v,last[u]};last[u]=cnt;
	e[++cnt]=(edge){u,last[v]};last[v]=cnt;
}
inline int get_lca(int x,int y){
	for(;anc[x]!=anc[y];x=fa[anc[x]])
		if(dep[anc[x]]<dep[anc[y]])swap(x,y);
	return (dep[x]<dep[y])?x:y;
}
inline int find(int x,int deep){
	for(;dep[x]-dep[anc[x]]<deep;x=fa[anc[x]])deep-=dep[x]-dep[anc[x]]+1;
	return id[pos[x]-deep];
}
void dfs(int x){
	sz[x]=1;
	for(int i=1,p=fa[x];i<=blo&&p;i++,p=fa[p])s[x][i]=s[p][i]+a[x];
	reg(x){
		if(e[i].to==fa[x])continue;
		fa[e[i].to]=x;dep[e[i].to]=dep[x]+1;dfs(e[i].to);sz[x]+=sz[e[i].to];
		if(sz[e[i].to]>sz[son[x]])son[x]=e[i].to;
	}
}
void divide(int x,int tp){
	anc[x]=tp;id[pos[x]=++dfsclk]=x;
	if(son[x])divide(son[x],tp);
	reg(x){
		if(e[i].to==fa[x])continue;
		if(e[i].to!=son[x])divide(e[i].to,e[i].to);
	}
}
inline int query(int x,int y,int z){
	if(z<=blo)return s[x][z]-s[y][z]+a[y];int i,sum=0;
	for(;anc[x]!=anc[y];x=find(x,pos[x]-i))
		for(i=pos[x];i>=pos[anc[x]];i-=z)sum+=a[id[i]];
	for(int i=pos[x];i>=pos[y];i-=z)sum+=a[id[i]];
	return sum;
}
inline int solve(int x,int y,int z){
	int ans=0,lca=get_lca(x,y),len=dep[x]+dep[y]-(dep[lca]<<1);
	if(len%z)ans+=a[y];y=find(y,len%z);
	int t1=find(x,(dep[x]-dep[lca])/z*z);
	if(dep[y]<=dep[lca])return ans+query(x,t1,z);
	int t2=find(y,(dep[y]-dep[lca])/z*z);
	if(t1==t2)ans-=a[t1];return ans+query(x,t1,z)+query(y,t2,z);
}
int main(){
	n=read();blo=((int)sqrt(n))/9*4+1;
	rep(i,1,n)a[i]=read();
	rep(i,1,n-1){
		int u=read(),v=read();
		insert(u,v);
	}
	dfs(1);divide(1,1);
	rep(i,1,n)b[i]=read();
	rep(i,1,n-1)printf("%d\n",solve(b[i],b[i+1],read()));
	return 0;
}
```