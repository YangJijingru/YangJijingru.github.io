---
subtitle: "吊打std的乱搞法"
tags: 
 - 计算几何-凸包
 - DP-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P7.jpg"
preview-img: "/img/preview/P7.jpg"
---
标签：DP，凸包

# 题目

给出N个点，选出其中若干个点，使得这些点的凸包上点的个数尽量多

N<=250

# 分析

30pts

暴力二进制+graham凸包

$$O(2^n n \log n)$$

正解

枚举每条边然后DP

每次合并上下两个凸线

狂虐std

# code

暴力
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
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=106;
int n,ans=0,book[maxn],cnt=0;
struct P{int x,y;}p[maxn],q[maxn],s[maxn];
inline P operator - (P a,P b){P t;t.x=a.x-b.x;t.y=a.y-b.y;return t;}
inline ll operator * (P a,P b){return a.x*b.y-a.y*b.x;}
inline ll dis(P a,P b){return (a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y);}
inline bool operator < (P a,P b){
	ll t=(a-q[1])*(b-q[1]);
	if(t==0)return dis(q[1],a)<dis(q[1],b);else return t>0;
}
void graham(){
	int t=1,top=0;
	rep(i,2,cnt)
		if(q[i].y<q[t].y||(q[i].y==q[t].y&&q[i].x<q[t].x))t=i;
	swap(q[1],q[t]);
	sort(q+2,q+1+cnt);
	s[++top]=q[1],s[++top]=q[2];
	rep(i,3,cnt){
		while((s[top]-s[top-1])*(q[i]-q[top-1])<=0)top--;
		s[++top]=q[i];
	}
	s[top+1]=p[1];
	ans=max(ans,top);
}
int main()
{
	n=read();
	rep(i,1,n)p[i].x=read(),p[i].y=read();
	rep(i,1,n)q[i]=p[i];cnt=n;graham();
	while(book[n+1]==0){
		mem(q,0);cnt=0;int pos;
		rep(i,1,n+1)if(book[i]==0){pos=i;break;}
		rep(i,1,pos-1)book[i]=0;book[pos]=1;
		//rep(i,1,n)cout<<book[i];cout<<endl;
		rep(i,1,n)if(book[i])q[++cnt]=p[i];//cout<<"R";
		if(cnt)graham();
	}
	cout<<ans<<endl;
	return 0;
}
```
正解
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
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=256;
int f1[maxn],f2[maxn],n,cnt,ans=1;
struct node{int x,y;}p[maxn];
inline bool operator < (node a,node b){return a.x==b.x?a.y<b.y:a.x<b.x;}
inline node operator - (node a,node b){return (node){a.x-b.x,a.y-b.y};}
inline ll operator * (node a,node b){return a.x*b.y-a.y*b.x;}
struct edge{int a,b;}s1[maxn*maxn],s2[maxn*maxn]; 
inline bool cmp1(edge a,edge b){
	ll tmp=(p[a.b]-p[a.a])*(p[b.b]-p[b.a]);
	if(tmp<0||((tmp==0)&&(a.a<b.a)))return true;else return false;
}
inline bool cmp2(edge a,edge b){
	ll tmp=(p[a.b]-p[a.a])*(p[b.b]-p[b.a]);
	if(tmp>0||((tmp==0)&&(a.a<b.a)))return true;else return false;
}
struct edgex{
	int pre[maxn*maxn],next[maxn*maxn];
	void init(int cnt){
		pre[0]=0;rep(i,1,cnt)pre[i]=i-1;pre[cnt+1]=cnt;
		next[0]=1;rep(i,1,cnt)next[i]=i+1;next[cnt+1]=cnt+1;
	}
	void del(int x){pre[next[x]]=pre[x];next[pre[x]]=next[x];}
}e1,e2;
int main()
{
	n=read();
	rep(i,1,n)p[i].x=read(),p[i].y=read();
	sort(p+1,p+1+n);
	rep(i,1,n)
		rep(j,i+1,n)s1[++cnt]=s2[cnt]=(edge){i,j};
	sort(s1+1,s1+cnt+1,cmp1);sort(s2+1,s2+cnt+1,cmp2);
	e1.init(cnt),e2.init(cnt);
	rep(i,1,n){
		mem(f1,0);mem(f2,0);
		f1[i]=f2[i]=1;
		for(int j=e1.next[0];j<=cnt;j=e1.next[j])
			if(s1[j].a<i)e1.del(j);else if(f1[s1[j].a])f1[s1[j].b]=max(f1[s1[j].b],f1[s1[j].a]+1);
		for(int j=e2.next[0];j<=cnt;j=e2.next[j])
			if(s2[j].a<i)e2.del(j);else if(f2[s2[j].a])f2[s2[j].b]=max(f2[s2[j].b],f2[s2[j].a]+1);
		rep(j,1,n)ans=max(ans,f1[j]+f2[j]-2);
	}
	cout<<ans<<endl;
	return 0;
}
```