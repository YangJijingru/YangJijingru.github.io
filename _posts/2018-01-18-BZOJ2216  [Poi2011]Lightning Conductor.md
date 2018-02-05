---
subtitle: "正经的决策单调性"
tags: 
 - 数据结构-单调队列
 - 基础算法-二分
 - DP-决策单调性
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P12.jpg"
preview-img: "/img/preview/P49.jpg"
---
标签：决策单调性DP，单调队列，二分

# 题目

[题目传送门](http://www.lydsy.com/JudgeOnline/problem.php?id=2216)

Description


已知一个长度为n的序列a1,a2,...,an。
对于每个1<=i<=n，找到最小的非负整数p满足 对于任意的j, aj < = ai + p - sqrt(abs(i-j))
Input

第一行n，(1<=n<=500000)
下面每行一个整数，其中第i行是ai。(0<=ai<=1000000000)

Output

n行，第i行表示对于i，得到的p

Sample Input
6

5

3

2

4

2

4

Sample Output
2

3

5

3

5

4

# 分析

先普通DP,f[i]就是对应的P

$$f[i]=max\{a[j]+sqrt(abs(i-j))\}-a[i]$$

显然，这样做法时间复杂度是O(n^2)，因为这题是1D1D（就是存在n种状态，每种状态存在n种转移）

把绝对值去掉

$$f[i]=max\{a[j]+sqrt(i-j)\}-a[i]\ (i>=j)$$
$$f[i]=max\{a[j]+sqrt(j-i)\}-a[i]\ (i<j)$$

这两个式子很显然决策单调，然后可以用单调队列优化+二分

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
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=5e5+6;
int n,a[maxn];
double f[maxn],g[maxn];
struct node{int l,r,p;}que[maxn];
double cal(int j,int i){return a[j]+sqrt(abs(i-j))-a[i];}
int find(node t,int x){
	int l=t.l,r=t.r,mid;
	while(l<=r){
		mid=(l+r)>>1;
		if(cal(t.p,mid)>cal(x,mid))l=mid+1;else r=mid-1;
	}
	return l;
}
void dp(double *F){
	int head=1,tail=0;
	rep(i,1,n){
		que[head].l++;
		if(head<=tail&&que[head].r<que[head].l)head++;
		if(head>tail||cal(i,n)>cal(que[tail].p,n)){
			while(head<=tail&&cal(que[tail].p,que[tail].l)<cal(i,que[tail].l))tail--;
			if(head>tail)que[++tail]=(node){i,n,i};
			else{int t=find(que[tail],i);que[tail].r=t-1;que[++tail]=(node){t,n,i};}
		}
		F[i]=cal(que[head].p,i);
	}
}
int main()
{
	n=read();
	rep(i,1,n)a[i]=read();
	dp(f);
	rep(i,1,n/2)swap(a[i],a[n-i+1]);
	dp(g);
	rep(i,1,n)printf("%d\n",max(0,(int)ceil(max(f[i],g[n-i+1]))));
	return 0;
}
```


