---
tags: 
 - 基础算法-模拟
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P35.jpg"
preview-img: "/img/preview/P75.jpg"
---
标签：模拟

# 题目

[题面传送门](http://codeforces.com/contest/897/problem/A)

Are you going to Scarborough Fair?

Parsley, sage, rosemary and thyme.

Remember me to one who lives there.

He once was the true love of mine.

Willem is taking the girl to the highest building in island No.28, however, neither of them knows how to get there.

Willem asks his friend, Grick for directions, Grick helped them, and gave them a task.

Although the girl wants to help, Willem insists on doing it by himself.

Grick gave Willem a string of length n.

Willem needs to do m operations, each operation has four parameters l, r, c1, c2, which means that all symbols c1 in range [l, r] (from l-th to r-th, including l and r) are changed into c2. String is 1-indexed.

Grick wants to know the final string after all the m operations.
Input

The first line contains two integers n and m (1 ≤ n, m ≤ 100).

The second line contains a string s of length n, consisting of lowercase English letters.

Each of the next m lines contains four parameters l, r, c1, c2 (1 ≤ l ≤ r ≤ n, c1, c2 are lowercase English letters), separated by space.
Output

Output string s after performing m operations described above.
Examples
Input

3 1
ioi
1 1 i n

Output

noi

Input

5 3
wxhak
3 3 h x
1 5 x a
1 3 w g

Output

gaaak

Note

For the second example:

After the first operation, the string is wxxak.

After the second operation, the string is waaak.

After the third operation, the string is gaaak.


# 题意

给定长度为N的字符串，进行m次操作，每次将$l->r$范围内为$C_1$的字符改成$C_2$，输出操作完的字符串

# 数据范围

$1\le n,m\le 100$

# 分析

注意使用string，模拟水题吧

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

int n,m,l,r;
char c1,c2;
string s;
int main()
{
	cin>>n>>m;
	cin>>s;
	rep(i,1,m){
		cin>>l>>r>>c1>>c2;
		rep(j,l,r)
		    if(s[j-1]==c1)s[j-1]=c2;
	}
	cout<<s;
	return 0;
}
```