---
tags: 
 - 基础算法-dfs
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P36.jpg"
preview-img: "/img/preview/P76.jpg"
---
标签：dfs

# 题目

[题面传送门](http://codeforces.com/contest/897/problem/C)

What are you doing at the end of the world? Are you busy? Will you save us?

Nephren is playing a game with little leprechauns.

She gives them an infinite array of strings, f0... ∞.

f0 is "What are you doing at the end of the world? Are you busy? Will you save us?".

She wants to let more people know about it, so she defines fi =  "What are you doing while sending "fi - 1"? Are you busy? Will you send "fi - 1"?" for all i ≥ 1.

For example, f1 is

"What are you doing while sending "What are you doing at the end of the world? Are you busy? Will you save us?"? Are you busy? Will you send "What are you doing at the end of the world? Are you busy? Will you save us?"?". Note that the quotes in the very beginning and in the very end are for clarity and are not a part of f1.

It can be seen that the characters in fi are letters, question marks, (possibly) quotation marks and spaces.

Nephren will ask the little leprechauns q times. Each time she will let them find the k-th character of fn. The characters are indexed starting from 1. If fn consists of less than k characters, output '.' (without quotes).

Can you answer her queries?
Input

The first line contains one integer q (1 ≤ q ≤ 10) — the number of Nephren's questions.

Each of the next q lines describes Nephren's question and contains two integers n and k (0 ≤ n ≤ 105, 1 ≤ k ≤ 1018).
Output

One line containing q characters. The i-th character in it should be the answer for the i-th query.

Examples
Input

3
1 1
1 2
1 111111111111

Output

Wh.

Input

5
0 69
1 194
1 139
0 47
1 66

Output

abdef

Input

10
4 1825
3 75
3 530
4 1829
4 1651
3 187
4 584
4 255
4 774
2 474

Output

Areyoubusy

Note

For the first two examples, refer to f0 and f1 given in the legend.

# 题意

给出了字符串$f_0$，递推关系式为
$f(n) = str1 + f(n - 1) + str2 + f(n - 1) + str3$

Q次询问给出n,k，n表示f的下标，k表示 $f(n)$ 中第k个字符

# 数据范围

$0 ≤ n ≤ 10^5, 1 ≤ k ≤ 10^{18}$

# 分析

见到的最恶心题目，差评++

dfs深搜，注意细节啊，容易写挂题

# code

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=100006;
string s0="What are you doing at the end of the world? Are you busy? Will you save us?",
s1="What are you doing while sending \"",s2="\"? Are you busy? Will you send \"",s3="\"?";
ll q,n,l0,l1,l2,l3=2;
ll k,f[maxn];

char dfs(ll n,ll k)
{
	if(!n)return k<=l0?s0[k-1]:'.';
	if(k<=l1)return s1[k-1];
	k-=l1;
	if(k<=f[n-1]||!f[n-1])return dfs(n-1,k);
	k-=f[n-1];
	if(k<=l2)return s2[k-1];
	k-=l2;
	if(k<=f[n-1]||!f[n-1])return dfs(n-1,k);
	k-=f[n-1];
	return k<=l3?s3[k-1]:'.';
}

inline int slen(string s)
{
	for(int i=0;;i++)
	if(s[i]=='\0')return i;
}
int main()
{
	q=read();
	l0=s0.length();l1=s1.length();l2=s2.length();
	f[0]=l0;
	rep(i,1,maxn)
	{
		f[i]=2*f[i-1]+l1+l2+l3;
		if(f[i]>1e18)break;
	}
	while(q--)
	{
		n=read(),k=read();
		putchar(dfs(n,k));
	}
}
```

