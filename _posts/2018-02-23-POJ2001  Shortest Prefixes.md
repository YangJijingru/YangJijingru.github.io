---
subtitle: "字典树划水签到题"
tags: 
 - 字符串-字典树
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P39.jpg"
preview-img: "/img/preview/P79.jpg"
---
标签：字典树

# 题目

[题目传送门](https://cn.vjudge.net/problem/POJ-2001)

A prefix of a string is a substring starting at the beginning of the given string. The prefixes of "carbon" are: "c", "ca", "car", "carb", "carbo", and "carbon". Note that the empty string is not considered a prefix in this problem, but every non-empty string is considered to be a prefix of itself. In everyday language, we tend to abbreviate words by prefixes. For example, "carbohydrate" is commonly abbreviated by "carb". In this problem, given a set of words, you will find for each word the shortest prefix that uniquely identifies the word it represents.

In the sample input below, "carbohydrate" can be abbreviated to "carboh", but it cannot be abbreviated to "carbo" (or anything shorter) because there are other words in the list that begin with "carbo".

An exact match will override a prefix match. For example, the prefix "car" matches the given word "car" exactly. Therefore, it is understood without ambiguity that "car" is an abbreviation for "car" , not for "carriage" or any of the other words in the list that begins with "car".

## Input

The input contains at least two, but no more than 1000 lines. Each line contains one word consisting of 1 to 20 lower case letters. 

## Output

The output contains the same number of lines as the input. Each line of the output contains the word from the corresponding line of the input, followed by one blank space, and the shortest prefix that uniquely (without ambiguity) identifies this word. 

## Sample Input

    carbohydrate
    cart
    carburetor
    caramel
    caribou
    carbonic
    cartilage
    carbon
    carriage
    carton
    car
    carbonate


## Sample Output


    carbohydrate carboh
    cart cart
    carburetor carbu
    caramel cara
    caribou cari
    carbonic carboni
    cartilage carti
    carbon carbon
    carriage carr
    carton carto
    car car
    carbonate carbona
    

# 分析

字典树前缀（也不能这么说）

不知道该怎么解释（雾）

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
const int maxn=3e5+6;
char a[10006][36];
int son[maxn],trie[maxn][27],n,id;
void insert(char st[]){
	int k,now=0,len=strlen(st+1);
	rep(i,1,len){
		k=st[i]-'a';
		if(!trie[now][k])trie[now][k]=++id;
		now=trie[now][k];
		son[now]++;
	}
}
void ask(char st[]){
	int k,now=0,len=strlen(st+1);
	rep(i,1,len){
		if(son[now]==1)break;
		k=st[i]-'a';
		printf("%c",st[i]);
		now=trie[now][k];
	}
}
int main()
{
	while(scanf("%s",a[++n]+1)!=EOF)insert(a[n]);
	rep(i,1,n){
		printf("%s ",a[i]+1);
		ask(a[i]);
		printf("\n");
	}
	return 0;
}
```
