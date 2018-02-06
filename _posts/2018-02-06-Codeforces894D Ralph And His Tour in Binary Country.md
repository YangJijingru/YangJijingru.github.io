---
tags: 
 - 基础算法-二分
 - DP-树形
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P30.jpg"
preview-img: "/img/preview/P70.jpg"
---
标签：二分，树

# 题目

[题目传送门](http://codeforces.com/problemset/problem/894/D)

Ralph is in the Binary Country. The Binary Country consists of n cities and (n - 1) bidirectional roads connecting the cities. The roads are numbered from 1 to (n - 1), the i-th road connects the city labeled (here ⌊ x⌋ denotes the x rounded down to the nearest integer) and the city labeled (i + 1), and the length of the i-th road is Li.

Now Ralph gives you m queries. In each query he tells you some city Ai and an integer Hi. He wants to make some tours starting from this city. He can choose any city in the Binary Country (including Ai) as the terminal city for a tour. He gains happiness (Hi - L) during a tour, where L is the distance between the city Ai and the terminal city.

Ralph is interested in tours from Ai in which he can gain positive happiness. For each query, compute the sum of happiness gains for all such tours.

Ralph will never take the same tour twice or more (in one query), he will never pass the same city twice or more in one tour.
Input

The first line contains two integers n and m (1 ≤ n ≤ 106, 1 ≤ m ≤ 105).

(n - 1) lines follow, each line contains one integer Li (1 ≤ Li ≤ 105), which denotes the length of the i-th road.

m lines follow, each line contains two integers Ai and Hi (1 ≤ Ai ≤ n, 0 ≤ Hi ≤ 107).
Output

Print m lines, on the i-th line print one integer — the answer for the i-th query.
Examples
Input

2 2
5
1 8
2 4

Output

11
4

Input

6 4
2
1
1
3
2
2 4
1 3
3 2
1 7

Output

11
6
3
28

Note

Here is the explanation for the second sample.

Ralph's first query is to start tours from city 2 and Hi equals to 4. Here are the options:

    He can choose city 5 as his terminal city. Since the distance between city 5 and city 2 is 3, he can gain happiness 4 - 3 = 1.
    He can choose city 4 as his terminal city and gain happiness 3.
    He can choose city 1 as his terminal city and gain happiness 2.
    He can choose city 3 as his terminal city and gain happiness 1.
    Note that Ralph can choose city 2 as his terminal city and gain happiness 4.
    Ralph won't choose city 6 as his terminal city because the distance between city 6 and city 2 is 5, which leads to negative happiness for Ralph. 

So the answer for the first query is 1 + 3 + 2 + 1 + 4 = 11.

# 题意

给定一棵n个点的树，m次询问从x节点到其他节点（包括本身）距离L，给定点权$ H_i $，$ H_i $-L>0的权值之和

即 $ \sum H_i-L>0$

# 分析

先预处理出每个节点到其所有子节点的距离，从小到大排序

从下到上通过二分查找可以得到任意节点到子节点的满足条件的距离，对于每个询问，查询当前节点的子树权值和，同时向树根转移，每次计算兄弟节点对应的权值和（相当于看作以当前节点为根重构树），直到转移到树根

# code

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<vector>
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define maxn 1000006
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
int n,m,l[maxn],x,h,t,lson,rson,last;
ll ans=0;
vector<int>d[maxn];
vector<ll>pre[maxn];
inline ll query(int x,int h)
{
	if(h<=0)return 0;
	int pos=upper_bound(d[x].begin(),d[x].end(),h)-d[x].begin();
	return (ll)pos*h-pre[x][pos-1];
}

int main()
{
	n=read(),m=read();
	rep(i,2,n)l[i]=read();
	dep(i,n,1){
		d[i].push_back(0);
		lson=i<<1,rson=i<<1|1;
		if(lson<=n)
			rep(j,0,d[lson].size()-1)d[i].push_back(d[lson][j]+l[lson]);
		if(rson<=n)
		    rep(j,0,d[rson].size()-1)d[i].push_back(d[rson][j]+l[rson]);
		sort(d[i].begin(),d[i].end());
		pre[i].resize(d[i].size());
		rep(j,1,pre[i].size()-1)pre[i][j]=pre[i][j-1]+d[i][j];
	}
	while(m--){
		x=read(),h=read();
		ans=0;t=x;last=0;
		while(t){
			if(h<0)break;
			ans+=h;
			lson=t<<1,rson=t<<1|1;
			if(lson<=n&&lson!=last)ans+=query(lson,h-l[lson]);
			if(rson<=n&&rson!=last)ans+=query(rson,h-l[rson]);
			h-=l[t];last=t;t>>=1;
		}
		printf("%I64d\n",ans);
	}
	return 0;
}	
```