---
subtitle: "搜索神仙题"
tags: 
 - 基础算法-bfs
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P93.jpg"
preview-img: "/img/preview/P93.jpg"
---
标签：bfs

# 题目

[题目传送门](https://cn.vjudge.net/problem/AtCoder-3916)

给定一个$w\times h$的网格，网格之间有1的边权。上面有$n\leq 30$个位置有障碍，网格大小 $10^6$，求所有非障碍点对间的最短路之和。

# 分析

把没有状态的两行或者两列压缩到一起，我们发现没有障碍的行和列出去的最短路一定是相同的（或者说对答案的影响是等价的）

计算出中间的边经过的次数然后合并

然后对周围存在障碍的点跑一边bfs即可

时间复杂度$O(n^4)$

细节：*因为每条边的贡献计算了两次，所以最后统计答案时候要除2*

# code
```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<vector>
#define rep(i,a,b) for(register int i=a;i<=b;i++)
#define dep(i,a,b) for(register int i=a;i>=b;i--)
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
#define pb push_back
#define inf 1e9
const int maxn=66,maxm=1e6+6,dx[4]={-1,1,0,0},dy[4]={0,0,-1,1},mod=1e9+7;
struct node{int x,y;}a[maxn<<1],que[maxn*maxn*4];
int n,m,K,ans,lx,ly,cnt[maxn][maxn],s1[maxm],s2[maxm],dis[maxn][maxn],head,tail;
bool fb[maxn][maxn],mark1[maxm],mark2[maxm];
vector<int>px,py;
void vnique(vector <int> &r){
	sort(r.begin(),r.end());
	auto t=unique(r.begin(),r.end());
	r.erase(t,r.end());
}
void bfs(int x,int y){
	rep(i,0,lx)rep(j,0,ly)dis[i][j]=inf;
	dis[x][y]=0;que[head=tail=1]=(node){x,y};
	while(head<=tail){
		int nx=que[head].x,ny=que[head++].y;
		rep(i,0,3){
			int nex=nx+dx[i],ney=ny+dy[i];
			if(nex>lx||nex<0||ney>ly||ney<0||dis[nex][ney]<=dis[nx][ny]+1||fb[nex][ney])continue;
			dis[nex][ney]=dis[nx][ny]+1;que[++tail]=(node){nex,ney};
		}
	}
}
int main()
{
	n=read(),m=read(),K=read();
	px.pb(1);px.pb(n+1);py.pb(1);py.pb(m+1);
	rep(i,1,n)s1[i]=m;
	rep(i,1,m)s2[i]=n;
	rep(i,1,K){
		a[i].x=read()+1,a[i].y=read()+1;
		px.pb(a[i].x),px.pb(a[i].x+1);
		py.pb(a[i].y),py.pb(a[i].y+1);
		s1[a[i].x]--,mark1[a[i].x]=1;
		s2[a[i].y]--,mark2[a[i].y]=1;
	}
	rep(i,1,n)s1[i]=(s1[i]+s1[i-1])%mod;
	rep(i,1,m)s2[i]=(s2[i]+s2[i-1])%mod;
	rep(i,1,n-1)if(!mark1[i]&&!mark1[i+1])ans=(ans+1ll*2*s1[i]*(s1[n]-s1[i]))%mod;
	rep(i,1,m-1)if(!mark2[i]&&!mark2[i+1])ans=(ans+1ll*2*s2[i]*(s2[m]-s2[i]))%mod;
	vnique(px);vnique(py);lx=px.size()-2;ly=py.size()-2;
	rep(i,1,K)
		fb[lower_bound(px.begin(),px.end(),a[i].x)-px.begin()][lower_bound(py.begin(),py.end(),a[i].y)-py.begin()]=1;
	rep(i,0,lx)rep(j,0,ly)cnt[i][j]=1ll*(px[i+1]-px[i])*(py[j+1]-py[j])%mod;
	rep(i,0,lx)rep(j,0,ly)
		if(!fb[i][j]){
			bfs(i,j);
			rep(k,0,lx)rep(p,0,ly)
				if(!fb[k][p])ans=(ans+1ll*cnt[i][j]*dis[k][p]%mod*cnt[k][p])%mod; 
   		}
	ans=(ans+mod)%mod;ans=1ll*ans*(mod/2+1)%mod;
	cout<<ans<<endl;
	return 0;
}
```