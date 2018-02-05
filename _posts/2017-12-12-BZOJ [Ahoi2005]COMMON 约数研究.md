---
tags: 
 - 数论-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P17.jpg"
preview-img: "/img/preview/P57.jpg"
---
标签：数学

# 题目

[题目传送门](http://www.lydsy.com/JudgeOnline/problem.php?id=1968)

Description
![](http://www.lydsy.com/JudgeOnline/images/1968.jpg)

Input

只有一行一个整数 N（0 < N < 1000000）。
Output

只有一行输出，为整数M，即f(1)到f(N)的累加和。
Sample Input

    3

Sample Output

    5

HINT

Source

Day2

# code

```
#include<bits/stdc++.h>
#define rep(i,a,b) for(int i=a;i<=b;i++)
using namespace std;

long long  ans,n;

int main()
{
	cin>>n;
	rep(i,1,n)ans+=n/i;
	cout<<ans<<endl;
	return 0;
}
```