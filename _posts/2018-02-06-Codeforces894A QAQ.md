---
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P33.jpg"
preview-img: "/img/preview/P73.jpg"
---
标签：模拟

#题目

[题目传送门](http://codeforces.com/problemset/problem/894/A)

"QAQ" is a word to denote an expression of crying. Imagine "Q" as eyes with tears and "A" as a mouth.

Now Diamond has given Bort a string consisting of only uppercase English letters of length n. There is a great number of "QAQ" in the string (Diamond is so cute!).
illustration by 猫屋 https://twitter.com/nekoyaliu

Bort wants to know how many subsequences "QAQ" are in the string Diamond has given. Note that the letters "QAQ" don't have to be consecutive, but the order of letters should be exact.
Input

The only line contains a string of length n (1 ≤ n ≤ 100). It's guaranteed that the string only contains uppercase English letters.
Output

Print a single integer — the number of subsequences "QAQ" in the string.
Examples
Input

QAQAQYSYIOIWIN

Output

4

Input

QAQQQZZYNOIWIN

Output

3

Note

In the first example there are 4 subsequences "QAQ": "QAQAQYSYIOIWIN", "QAQAQYSYIOIWIN", "QAQAQYSYIOIWIN", "QAQAQYSYIOIWIN".

# 题意

查找字符串S中有多少个“QAQ”

# 分析

三重循环的模拟水题啊

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
#ifdef WIN32
#define LL "%I64d"
#else
#define LL "%lld"
#endif
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
string s;
int ans=0;
int main()
{
	cin>>s;
	int len=s.length();
    rep(i,0,len-1)
    	if(s[i]=='Q')
    		rep(j,i+1,len-1)
    		    if(s[j]=='A')
    		        rep(k,j+1,len-1)
    		            if(s[k]=='Q')ans++;
    cout<<ans<<endl;
    return 0;
}
```