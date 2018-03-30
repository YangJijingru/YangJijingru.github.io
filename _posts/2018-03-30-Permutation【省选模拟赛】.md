---
subtitle: "网络流思维好题"
tags: 
 - 网络流-最小割
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P97.jpg"
preview-img: "/img/preview/P97.jpg"
---
标签：网络流

# 题目

有 n 个数 x1~xn。你需要找出它们的一个排列，满足 m 个条件，每个条件形 如 xa必须在 xb之前。在此基础上，你要最大化这个排列的最大子段和。

n<=500，m<=1000。

# 分析

一眼网络流，可是不会建图gg

然后推性质：

发现最大字段和一定是一个满足条件的排列中的一段

形式为“不选->选->不选”

将每个点拆成两个，转化为最小割

对于每个限制，将其连上正无穷的边即可

然后讨论每个点权的正负与源汇的连边

答案就是正的权值和-最小割

# code
```
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
//**********head by yjjr**********
#define inf 1e9
const int maxn=1e4+6;
struct edge{int to,next,v;}e[maxn<<2];
int S,T,n,m,cnt=1,a[maxn],h[maxn],u[maxn],v[maxn],last[maxn],que[maxn<<2];
ll ans=0;
void insert(int u,int v,int w){
	e[++cnt]=(edge){v,last[u],w};last[u]=cnt;
	e[++cnt]=(edge){u,last[v],0};last[v]=cnt;
}
inline bool bfs(){
	int head=0,tail=1,now;
	mem(h,-1);que[0]=S,h[S]=0;
	while(head<tail){
		now=que[head++];
		reg(now)
			if(e[i].v&&h[e[i].to]==-1){h[e[i].to]=h[now]+1;que[tail++]=e[i].to;}
	}
	return h[T]!=-1;
}
inline int dfs(int x,int f){
	if(x==T)return f;
	int w,used=0;
	reg(x)
		if(e[i].v&&h[e[i].to]==h[x]+1){
			w=dfs(e[i].to,min(f-used,e[i].v));
			e[i].v-=w;e[i^1].v+=w;
			used+=w;if(used==f)return f;
		}
	if(!used)h[x]=-1;
	return used;
}	
void dinic(){while(bfs())ans-=dfs(S,inf);}
int main()
{
	freopen("permutation.in","r",stdin);
	freopen("permutation.out","w",stdout);
	n=read(),m=read();S=0;T=n<<2|1;
	rep(i,1,n){
		int x=read();
		if(x>0){
			ans+=x;
			insert(S,2*i+1,x);
			insert(2*i+2,T,x);
		}else insert(2*i+1,2*i+2,-x);
	}
	rep(i,1,m){
		int u=read(),v=read();
		insert(2*u+1,2*v+1,inf);
		insert(2*u+2,2*v+2,inf);
	}
	dinic();
	cout<<ans<<endl;
	return 0;
}
```