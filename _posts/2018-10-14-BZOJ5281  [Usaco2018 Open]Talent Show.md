---
subtitle: "分数规划"
tags: 
 - 特殊-分数规划
 - 基础算法-二分
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P87.jpg"
preview-img: "/img/preview/P87.jpg"
---
标签：二分，背包DP，分数规划

# 题目
[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=5281)

## Description
FarmerJohn要带着他的N头奶牛，方便起见编号为1…N，到农业展览会上去，参加每年的达牛秀！他的第i头奶牛重量为wi，才艺水平为ti，两者都是整数。在到达时，FarmerJohn就被今年达牛秀的新规则吓到了：
（一）参加比赛的一组奶牛必须总重量至少为W（这是为了确保是强大的队伍在比赛，而不仅是强大的某头奶牛），并且
（二）总才艺值与总重量的比值最大的一组获得胜利。FJ注意到他的所有奶牛的总重量不小于W，所以他能够派出符合规则（一）的队伍。帮助他确定这样的队伍中能够达到的最佳的才艺与重量的比值。
## Input
输入的第一行包含N和W。下面N行，每行用两个整数wi和ti描述了一头奶牛。
1≤N≤250
1≤W≤1000
1≤wi≤10^6
1≤ti≤10^3
## Output
请求出Farmer用一组总重量最少为W的奶牛最大可能达到的总才艺值与总重量的比值。
如果你的答案是A，输出1000A向下取整的值，以使得输出是整数
（当问题中的数不是一个整数的时候，向下取整操作在向下舍入到整数的时候去除所有小数部分）。
## Sample Input
```
3 15
20 21
10 11
30 31
```
## Sample Output
```
1066
```
在这个例子中，总体来看最佳的才艺与重量的比值应该是仅用一头才艺值为11、重量为10的奶牛，但是由于我们需要至少15单位的重量，最优解最终为使用这头奶牛加上才艺值为21、重量为20的奶牛。这样的话才艺与重量的比值为(11+21)/(10+20)=32/30=1.0666666...，乘以1000向下取整之后得到1066。

# 分析

分数规划典型题

先二分答案，然后用背包DP检验答案合理性

$$\frac{\sum{t_i}}{\sum w_i}\geq ans$$

$$t_i-w_i*ans\geq 0$$

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
//******head by yjjr******
#define inf 1e9
#define mid ((l+r)>>1)
const int maxn=1e6;
int n,m,t[maxn],w[maxn];ll f[maxn];
inline bool check(int x){
	rep(i,1,m)f[i]=-inf;
	rep(i,1,n){
		ll tmp=t[i]-1ll*w[i]*x;
		dep(j,m,0){
			if(f[j]==-inf)continue;
			int tt=j+w[i];if(tt>m)tt=m;
			f[tt]=max(f[tt],f[j]+tmp);
		}
	}
	return f[m]>=0;
}
int main(){
	n=read(),m=read();
	rep(i,1,n)w[i]=read(),t[i]=read()*1000;
	int l=0,r=inf;
	while(l<=r){if(check(mid))l=mid+1;else r=mid-1;}
	cout<<l-1<<endl;
	return 0;
}
```
