---
subtitle: "根号分治"
tags: 
 - 特殊-根号分治
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P92.jpg"
preview-img: "/img/preview/P92.jpg"
---
标签：根号算法

# 题目

[题目传送门](https://cn.vjudge.net/problem/Gym-101741K)

Let us define a consistent set of occurrences of string t in string s as a set of occurrences of t in s such that no two occurrences intersect (in other words, no character position in s belongs to two different occurrences).

You are given a string s consisting of n lowercase English letters, and m queries. Each query contains a single string ti.

For each query, print the maximum size of a consistent set of occurrences of t in s.

## Input
The first line contains two space-separated integers n and m: the length of string s and the number of queries (1 ≤ n ≤ 105, 1 ≤ m ≤ 105).

The second line contains the string s consisting of n lowercase English letters.

Each of the next m lines contains a single string ti consisting of lowercase English letters: the i-th query (1 ≤ |ti| ≤ n, where |ti| is the length of the string ti).

It is guaranteed that the total length of all ti does not exceed 105 characters.

## Output
For each query i, print one integer on a separate line: the maximum size of a consistent set of occurrences of ti in s.

## Example
### Input
```
6 4
aaaaaa
a
aa
aaa
aaaa
```
## Output
```
6
3
2
1
```
# 分析

显然，对于一个字符串S，考虑其中本质不同的长度只有$\sqrt m$个，所以我们对每个起点预处理出$\sqrt m$个hash值，然后将答案存入Map

之后对每个询问，排序后直接计算hash查询就好了

时间复杂度$O(n \sqrt n)$

# code
```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<map>
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
#define pa pair <int,int>
#define mp make_pair
#define fi first
#define se second
const int maxn=1e5+6;
const int bas[2]={11257,12257};
const int Mod[2]={1e9+7,1e9+9};
int n,m,all,cur,l[maxn],r[maxn],ans[maxn],len[maxn],h[2][maxn],pwd[2][maxn];
map<pa,pa>cnt;
char s[maxn],t[maxn]; 
inline void solve(int len){
	cnt.clear();
	rep(i,len,n){
		pa val=mp((h[0][i]-1ll*h[0][i-len]*pwd[0][len]%Mod[0]+Mod[0])%Mod[0],(h[1][i]-1ll*h[1][i-len]*pwd[1][len]%Mod[1]+Mod[1])%Mod[1]);
    	if(cnt.find(val)==cnt.end())cnt[val]=mp(1,i);
    	else if(i-cnt[val].se<len)continue;else ++cnt[val].fi,cnt[val].se=i;
    }
    rep(i,1,m)
    	if(r[i]-l[i]+1==len){
    		pa cur=mp(0,0);
    		rep(j,l[i],r[i])cur=mp((1ll*cur.fi*bas[0]+t[j])%Mod[0],(1ll*cur.se*bas[1]+t[j])%Mod[1]);
			ans[i]=cnt[cur].fi;
		}
}
int main(){
	n=read(),m=read();
	scanf("%s",s+1);
	pwd[0][0]=pwd[1][0]=1;
	rep(j,0,1)
		rep(i,1,n){
			h[j][i]=(1ll*h[j][i-1]*bas[j]+s[i])%Mod[j];
			pwd[j][i]=1ll*pwd[j][i-1]*bas[j]%Mod[j];
		}
	rep(i,1,m){
		scanf("%s",t+cur+1);
		l[i]=cur+1,len[i]=strlen(t+cur+1);
		cur+=len[i],r[i]=cur;
	}
	sort(len+1,len+1+m),all=unique(len+1,len+1+m)-len-1;
	rep(i,1,all)solve(len[i]);
	rep(i,1,m)printf("%d\n",ans[i]);
	return 0;
}
```