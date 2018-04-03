---
subtitle: "智商题"
tags: 
 - 数据结构-栈
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P94.jpg"
preview-img: "/img/preview/P94.jpg"
---
标签：模拟，栈

# 题目

[题目传送门](https://cn.vjudge.net/problem/AtCoder-2644)

在$$r\times c$$的矩形中有n对点，问是否存在一种方案连接每一对点，同时连线不能相交。

n<=100000

# 分析

如果一对点至少有一个点不在边界上，那么就不改变图的连通性，不影响答案，所以只需要判断边界上的点是否出现交叉的情况即可

然后模拟栈的操作进行括号匹配即可

# code
```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<vector>
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
#define pb push_back
const int maxn=1e5+6;
struct point{int id,val;};
vector<point> v[4];
int n,r,c,s[maxn<<1],top=0;
inline bool check(int x,int y){
	if(x==0||x==r||y==0||y==c)return 1;else return 0;
}
void push(int id,int x,int y){
	if(x==0)v[0].pb({id,y});
	else if(y==c)v[1].pb({id,x});
	else if(x==r)v[2].pb({id,y});
	else if(y==0)v[3].pb({id,x});
}
inline bool cmp1(point a,point b){return a.val<b.val;}
inline bool cmp2(point a,point b){return a.val>b.val;}
int main()
{
	r=read(),c=read(),n=read();
	rep(i,1,n){
		int X1=read(),Y1=read(),X2=read(),Y2=read();
		if(check(X1,Y1)&&check(X2,Y2))push(i,X1,Y1),push(i,X2,Y2);
	}
	sort(v[0].begin(),v[0].end(),cmp1);
	sort(v[1].begin(),v[1].end(),cmp1);
	sort(v[2].begin(),v[2].end(),cmp2);
	sort(v[3].begin(),v[3].end(),cmp2);
	rep(j,0,3)
		for(int i=0;i<v[j].size();i++){
			if(top==0){s[++top]=v[j][i].id;continue;}
			if(s[top]==v[j][i].id)top--;else s[++top]=v[j][i].id;
		}
	if(top==0)puts("YES");else puts("NO");
	return 0;
}
```