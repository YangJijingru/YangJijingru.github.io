---
subtitle: "AC自动机模板题"
tags: 
 - 字符串-AC自动机
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P2.jpg"
preview-img: "/img/preview/P2.jpg"
---
标签：AC自动机

# 题目

[题目传送门](https://cn.vjudge.net/problem/HDU-2222)
## Description

   In the modern time, Search engine came into the life of everybody like Google, Baidu, etc.
    Wiskey also wants to bring this feature to his image retrieval system.
    Every image have a long description, when users type some keywords to find the image, the system will match the keywords with description of image and show the image which the most keywords be matched.
    To simplify the problem, giving you a description of image, and some keywords, you should tell me how many keywords will be match.
## Input
   First line will contain one integer means how many cases will follow by.
    Each case will contain two integers N means the number of keywords and N keywords follow. (N <= 10000)
    Each keyword will only contains characters 'a'-'z', and the length will be not longer than 50.
    The last line is the description, and the length will be not longer than 1000000.
## Output
   Print how many keywords are contained in the description.
## Sample Input
```
    1
    5
    she
    he
    say
    shr
    her
    yasherhs
```
## Sample Output
```
    3
```

# 分析

AC自动机模板题

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
char st[56],m[1000006];
int T,n,id,ans,trie[maxn][27],que[maxn],point[maxn],danger[maxn];
bool mark[maxn];
void insert(char st[]){
	int now=1,len=strlen(st+1),k;
	rep(i,1,len){
		k=st[i]-'a'+1;
		if(!trie[now][k])trie[now][k]=++id;
		now=trie[now][k];
	}
	danger[now]++;
}
void acmach(){
	int head=0,tail=1,now;
	que[0]=1;point[1]=0;
	while(head<tail){
		now=que[head++];
		rep(i,1,26){
			if(!trie[now][i])continue;
			int k=point[now];while(!trie[k][i])k=point[k];
			point[trie[now][i]]=trie[k][i];que[tail++]=trie[now][i];
		}
	}
}
void solve(){
	int now=1,len=strlen(m+1);
	rep(i,1,len){
		mark[now]=1;
		int k=m[i]-'a'+1;
		while(!trie[now][k])now=point[now];
		now=trie[now][k];
		if(!mark[now])
			for(int j=now;j;j=point[j])ans+=danger[j],danger[j]=0;
	}
	printf("%d\n",ans);
}
void clear(){
	rep(i,1,id){
	    point[i]=danger[i]=mark[i]=0;
		rep(j,1,26)trie[i][j]=0;
	}
}
int main()
{
	T=read();
	while(T--){
		n=read();
		id=1,ans=0;
		rep(i,1,26)trie[0][i]=1;
		rep(i,1,n)scanf("%s",st+1),insert(st);
		acmach();
		scanf("%s",m+1);
		solve();clear();
	}
	return 0;
}
```
