标签：最大生成树

# 题目

[题目传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=3390)

## Description

奶牛贝茜被雇去建设$N(2≤N≤1000)$个牛棚间的互联网．她已经勘探出$M(1≤M≤20000)$条可建的线路，每条线路连接两个牛棚，而且会苞费$C(1≤C≤100000)$．农夫约翰吝啬得很，他希望建设费用最少甚至他都不想给贝茜工钱． 贝茜得知工钱要告吹，决定报复．她打算选择建一些线路，把所有牛棚连接在一起，让约翰花费最大．但是她不能造出环来，这样约翰就会发现．
## Input
  第1行：$N,M$
  第2到$M+1$行：三个整数，表示一条可能线路的两个端点和费用．
 
## Output
 最大的花费．如果不能建成合理的线路，就输出$-1$
## Sample Input
```
5 8
1 2 3
1 3 7
2 3 10
2 4 4
2 5 8
3 4 6
3 5 2
4 5 17
```
## Sample Output
```
42
```
连接4和5,2和5，2和3，1和3，花费$17+8+10+7=42$

# 分析

最大生成树模板题

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
const int maxn=1e6+6;
int n,m,num=0,ans,fa[maxn];
struct edge{int x,y,v;}e[maxn<<1];
inline bool cmp(edge a,edge b){return a.v>b.v;}
inline int find(int x){return fa[x]==x?x:fa[x]=find(fa[x]);}
int main()
{
	n=read(),m=read();
	rep(i,1,n)fa[i]=i;
	rep(i,1,m)e[i]=(edge){read(),read(),read()};
	sort(e+1,e+1+m,cmp);
	rep(i,1,m){
		int root1=find(e[i].x),root2=find(e[i].y);
		if(root1!=root2){
			fa[root1]=root2,ans+=e[i].v,num++;
			if(num==n-1){cout<<ans<<endl;return 0;}
		}
	}
	puts("-1");
	return 0;
}
```
