---
subtitle: "神仙的小鸟！"
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P90.jpg"
preview-img: "/img/preview/P90.jpg"
---
标签：模拟

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=4723)

## Description

《飞扬的小鸟》是一款风靡的小游戏。在游戏中，小鸟一开始位于(0,0)处，它的目标是飞到横坐标为X的某个位置上。每一秒，你可以选择点击屏幕，那么小鸟会从(x,y)飞到(x+1,y+1)，或者不点击，那么小鸟会飞到(x+1,y-1)。在游戏中还有n个障碍物，用三元组(x[i],a[i],b[i])描述，表示在直线x=x[i]上，y<=a[i]或者y>=b[i]的部分
都是障碍物，碰到或者擦边都算游戏失败。请求出小鸟从(0,0)飞到目的地最少需要点击多少次屏幕。

## Input
第一行包含两个整数n(0<=n<=500000),X(1<=n<=10^9)。
接下来n行，每行三个整数x[i],a[i],b[i](0<x[i]<X，-10^9<=a[i]<b[i]<=10^9)。
数据保证x[i]<x[i+1]。
## Output
如果无论如何都飞不到目的地，输出NIE，否则输出点击屏幕的最少次数。
## Sample Input
```
4 11
4 1 4
7 -1 2
8 -1 3
9 0 2
```
## Sample Output
```
5
```
## HINT
![](https://www.lydsy.com/JudgeOnline/upload/201611/flahint.png)

# 分析

只需要判断对于每个障碍物能到达的区间即可，如果区间左右端点相反，那么就无法到达

注意要保证区间左右端点为闭区间且奇偶性与当前横坐标相同

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
const int maxn=5e5+6;
int X,n,x[maxn],a[maxn],b[maxn],A,B;
int main()
{
	n=read(),X=read();
	rep(i,1,n)x[i]=read(),a[i]=read(),b[i]=read();
	rep(i,1,n){
		int tmp=x[i]-x[i-1];
		A=max(A-tmp,a[i]+1);
		B=min(B+tmp,b[i]-1);
		if((A&1)!=(x[i]&1))A++;
		if((B&1)!=(x[i]&1))B--;
		if(A>B){puts("NIE");return 0;}
	}
	cout<<(A+x[n])/2<<endl;
	return 0;
}
```