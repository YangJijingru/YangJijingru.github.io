---
subtitle: "启发式合并总是很神仙"
tags: 
 - 数据结构-线段树
 - 特殊-启发式合并
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P67.jpg"
preview-img: "/img/preview/P67.jpg"
---
标签：线段树，启发式合并

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P3521)

给一棵n(1≤n≤200000个叶子的二叉树，可以交换每个点的左右子树，要求前序遍历叶子的逆序对最少。

# 分析

树的逆序对个数=左子树的逆序对个数+右子树的逆序对个数+跨越两棵子树形成的逆序对

在合并的过程中只需要交换比较下跨越两棵子树形成的逆序对数

# code
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
//**********head by yjjr**********
const int maxn=4e5+6,maxm=4e6+6;
int n,Size,seg,v[maxn],l[maxn],r[maxn],root[maxn];
int sum[maxm],ls[maxm],rs[maxm];
ll ans,cnt1,cnt2;
void readtree(int x){
	v[x]=read();
	if(!v[x]){
		l[x]=++Size;
		readtree(l[x]);
		r[x]=++Size;
		readtree(r[x]);
	}
}
void pushup(int k){sum[k]=sum[ls[k]]+sum[rs[k]];}
void build(int &k,int l,int r,int val){
#define mid ((l+r)>>1)
	if(!k)k=++seg;
	if(l==r){sum[k]=1;return;}
	if(val<=mid)build(ls[k],l,mid,val);else build(rs[k],mid+1,r,val);
	pushup(k);
}
inline int merge(int x,int y){
	if(!x)return y;
	if(!y)return x;
	cnt1+=(ll)sum[rs[x]]*sum[ls[y]];
	cnt2+=(ll)sum[ls[x]]*sum[rs[y]];
	ls[x]=merge(ls[x],ls[y]);
	rs[x]=merge(rs[x],rs[y]);
	pushup(x);
	return x;
}
void solve(int x){
	if(!x)return;
	solve(l[x]);solve(r[x]);
	if(!v[x]){
		cnt1=cnt2=0;
		root[x]=merge(root[l[x]],root[r[x]]);
		ans+=min(cnt1,cnt2);
	}
}
int main()
{
	n=read();++Size;
	readtree(1);
	rep(i,1,Size)if(v[i])build(root[i],1,n,v[i]);
	solve(1);
	cout<<ans<<endl;
	return 0;
}
```
