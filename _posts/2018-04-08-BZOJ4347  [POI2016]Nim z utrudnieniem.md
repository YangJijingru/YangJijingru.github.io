---
subtitle: "吐槽dl卡内存"
tags: 
 - DP-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P79.jpg"
preview-img: "/img/preview/P79.jpg"
---
标签：DP，博弈论

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=4347)

## Description

A和B两个人玩游戏，一共有m颗石子，A把它们分成了n堆，每堆石子数分别为a[1],a[2],...,a[n]，每轮可以选择一堆石子，取掉任意颗石子，但不能不取。谁先不能操作，谁就输了。

在游戏开始前，B可以扔掉若干堆石子，但是必须保证扔掉的堆数是d的倍数，且不能扔掉所有石子。A先手，请问B有多少种扔的方式，使得B能够获胜。

## Input

第一行包含两个正整数$$n,d(1\leq n\leq 500000,1\leq d\leq 10)$$。

第二行包含n个正整数$$a[1],a[2],...,a[n](1\leq a[i]\leq 1000000)$$。

本题中m不直接给出，但是保证$$m\leq 10000000$$。

## Output

输出一行一个整数，即方案数对$$10^9+7$$取模的结果。

## Sample Input
```
5 2
1 3 4 1 2
```
## Sample Output
```
2
```
# 分析

一开始往奇奇怪怪的博弈论和sg函数上想，还是水平不行啊qwq

---

结论：如果所有石子的异或和为0，那么B获胜

然后就是DP的问题了

我们设计状态f[i][j][k]表示前i堆石子中，取了%d余数为j的堆，剩下的石子xor和为k的方案数

但这样的状态数为$$val\times d\times n$$，其中val表示石子的最大值

这样显然会MLE（吐槽下这题卡内存）

可以用滚动数组优化为$$val\times d\times 2$$，但依然不行

然后clairs给出了[一种神仙做法](https://www.cnblogs.com/clrs97/p/5006924.html)

根据递推式，k和k^a[i]两个状态是需要互相计算的，这样就可以存一个中间变量，原地DP即可，注意要单独计算余0的情况

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
const int maxn=6e5+6,mod=1e9+7;
int a[maxn],tmp[maxn<<1],f[10][maxn<<1],n,d,p=1;
int main()
{
	n=read(),d=read();
	rep(i,1,n)a[i]=read();
	sort(a+1,a+1+n);
	f[0][0]=1;
	rep(i,1,n){
		int t=a[i];
		while(p<=t)p<<=1;
		rep(j,0,p-1)tmp[j]=(((f[d-1][j]+f[0][j^t])%mod)+mod)%mod;
		dep(j,d-1,1)rep(k,0,p-1)if(k<=(k^t)){
			int x=f[j][k];
			f[j][k]=(((f[j-1][k]+f[j][k^t])%mod)+mod)%mod;
			f[j][k^t]=(((f[j-1][k^t]+x)%mod)+mod)%mod;
		}
		rep(j,0,p-1)f[0][j]=tmp[j];
	}
	if(n%d==0)f[0][0]=(((f[0][0]+mod-1)%mod)+mod)%mod;
	cout<<f[0][0]<<endl;
	return 0;
}
```