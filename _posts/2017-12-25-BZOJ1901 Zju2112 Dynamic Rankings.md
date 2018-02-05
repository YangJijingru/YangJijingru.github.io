---
tags: 
 - 数据结构-主席树
 - 数据结构-树状数组
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P8.jpg"
preview-img: "/img/preview/P28.jpg"
---
标签：主席树，树状数组

# 题目

[题目传送门](http://www.lydsy.com/JudgeOnline/problem.php?id=1901)

Description
给定一个含有n个数的序列a[1],a[2],a[3]……a[n]，程序必须回答这样的询问：对于给定的i,j,k，在a[i],a[i+1
],a[i+2]……a[j]中第k小的数是多少(1≤k≤j-i+1)，并且，你可以改变一些a[i]的值，改变后，程序还能针对改
变后的a继续回答上面的问题。
Input
第一行有两个正整数n(1≤n≤10000)，m(1≤m≤10000)。
分别表示序列的长度和指令的个数。
第二行有n个数，表示a[1],a[2]……a[n]，这些数都小于10^9。
接下来的m行描述每条指令
每行的格式是下面两种格式中的一种。 
Q i j k 或者 C i t 
Q i j k （i,j,k是数字，1≤i≤j≤n, 1≤k≤j-i+1）
表示询问指令，询问a[i]，a[i+1]……a[j]中第k小的数。
C i t (1≤i≤n，0≤t≤10^9)表示把a[i]改变成为t
m,n≤10000
Output

 对于每一次询问，你都需要输出他的答案，每一个输出占单独的一行。
Sample Input
5 3

3 2 1 4 7

Q 1 4 3

C 2 6

Q 2 5 3

Sample Output
3

6

# 分析

带权值修改的区间第K小数，用树状数组套主席树解决

一开始O(n log n)建出n棵主席树，然后树状数组只维护变化就好了

query函数中累和操作写错，调了一个小时，真的zz

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
const int max1=3e6+6,max2=1e4+6;
int v[max2],num[max2<<1],hash[max2<<1],flag[max2],A[max2],B[max2],K[max2],root[max2];
int sum[max1],ls[max1],rs[max1];
int L[36],R[36],a,b,n,m,tot,top,sz;
char s[3];
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

inline int lowbit(int x){return x&(-x);}

int find(int x){
	int l=1,r=tot,mid;
	while(l<=r){
		mid=(l+r)>>1;
		if(hash[mid]<x)l=mid+1;else r=mid-1;
	}
	return l;
}

void update(int last,int l,int r,int &root,int w,int x){
	root=++sz;
	sum[root]=sum[last]+x;ls[root]=ls[last];rs[root]=rs[last];
	if(l==r)return;int mid=(l+r)>>1;
	if(w<=mid)update(ls[last],l,mid,ls[root],w,x);else update(rs[last],mid+1,r,rs[root],w,x);
}

int query(int l,int r,int k){
	if(l==r)return l;
	int i,suml=0,sumr=0,mid=(l+r)>>1;
	rep(i,1,a)suml+=sum[ls[L[i]]];
	rep(i,1,b)sumr+=sum[ls[R[i]]];
	if(sumr-suml>=k){
		rep(i,1,a)L[i]=ls[L[i]];
		rep(i,1,b)R[i]=ls[R[i]];
		return query(l,mid,k);
	}else{
		rep(i,1,a)L[i]=rs[L[i]];
		rep(i,1,b)R[i]=rs[R[i]];
		return query(mid+1,r,k-(sumr-suml));	
	}
}

int main()
{
	n=read(),m=read();
	rep(i,1,n)v[i]=read(),num[++top]=v[i];
	rep(i,1,m){
		scanf("%s",s);
		A[i]=read(),B[i]=read();
		if(s[0]=='Q')K[i]=read(),flag[i]=1;
		else num[++top]=B[i];
	}
	sort(num+1,num+top+1);hash[++tot]=num[1];
	rep(i,2,top)if(num[i]!=num[i-1])hash[++tot]=num[i];
	rep(i,1,n){
		int t=find(v[i]);
		for(int j=i;j<=n;j+=lowbit(j))update(root[j],1,tot,root[j],t,1);
	}
	rep(i,1,m)
		if(flag[i]){
			a=0;b=0;A[i]--;
			for(int j=A[i];j>0;j-=lowbit(j))L[++a]=root[j];
			for(int j=B[i];j>0;j-=lowbit(j))R[++b]=root[j];
			printf("%d\n",hash[query(1,tot,K[i])]);
		}else{
			int t=find(v[A[i]]);
			for(int j=A[i];j<=n;j+=lowbit(j))update(root[j],1,tot,root[j],t,-1);
			v[A[i]]=B[i];
			t=find(B[i]);
			for(int j=A[i];j<=n;j+=lowbit(j))update(root[j],1,tot,root[j],t,1);
		}
	return 0;
}

```

