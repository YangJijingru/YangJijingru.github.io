---
tags: 
 - STL
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P4.jpg"
preview-img: "/img/preview/P44.jpg"
---
标签：STL，数据结构

# 题目
[题目传送门](http://codeforces.com/contest/899/problem/E)

Vasya has an array of integers of length n.

Vasya performs the following operations on the array: on each step he finds the longest segment of consecutive equal integers (the leftmost, if there are several such segments) and removes it. For example, if Vasya's array is [13, 13, 7, 7, 7, 2, 2, 2], then after one operation it becomes [13, 13, 2, 2, 2].

Compute the number of operations Vasya should make until the array becomes empty, i.e. Vasya removes all elements from it.
Input

The first line contains a single integer n (1 ≤ n ≤ 200 000) — the length of the array.

The second line contains a sequence a1, a2, ..., an (1 ≤ ai ≤ 109) — Vasya's array.
Output

Print the number of operations Vasya should make to remove all elements from the array.
Examples
Input

4
2 5 5 2

Output

2

Input

5
6 3 4 1 5

Output

5

Input

8
4 4 4 2 2 100 100 100

Output

3

Input

6
10 10 50 10 50 50

Output

4

Note

In the first example, at first Vasya removes two fives at the second and third positions. The array becomes [2, 2]. In the second operation Vasya removes two twos at the first and second positions. After that the array becomes empty.

In the second example Vasya has to perform five operations to make the array empty. In each of them he removes the first element from the array.

In the third example Vasya needs three operations. In the first operation he removes all integers 4, in the second — all integers 100, in the third — all integers 2.

In the fourth example in the first operation Vasya removes the first two integers 10. After that the array becomes [50, 10, 50, 50]. Then in the second operation Vasya removes the two rightmost integers 50, so that the array becomes [50, 10]. In the third operation he removes the remaining 50, and the array becomes [10] after that. In the last, fourth operation he removes the only remaining 10. The array is empty after that.

# 题意

给定长度为N的序列，每次将序列中长度最大部分相等的区间删去（若有多种情况删去最左端的区间），问多少次操作可以将删完

# 分析

数据结构题，用STL pair+priority queue写，维护区间最值

# code

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
#define mp(x,y) make_pair(x,y)
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
using namespace std;
inline int read()
{
	int f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=2e5+6;
priority_queue < pair < int,int > > que,del;
int n,cnt=0,ans=0,x,t1,t2;
int pre[maxn],next[maxn],num[maxn],col[maxn];
int main()
{
	n=read();
	rep(i,1,n){
		x=read();
		if(x==col[cnt])num[cnt]++;
		else col[++cnt]=x,num[cnt]=1;
	}
	rep(i,1,cnt)pre[i]=i-1,next[i]=i+1,que.push(mp(num[i],-i));//前驱后继建立链表 
	while(cnt){
		while(!del.empty()&&que.top()==del.top())que.pop(),del.pop();//找到一个可执行的区间 
		x=-que.top().second,que.pop();
		t1=pre[x],t2=next[x];
		next[t1]=t2,pre[t2]=t1;//修改前驱后继，删除区间 
		if(t1&&col[t2]==col[t1]){
			del.push(mp(num[t2],-t2)),del.push(mp(num[t1],-t1));//将两个即将合并的区间放入del队列 
			num[t1]+=num[t2],next[t1]=next[t2],pre[next[t2]]=t1;//合并操作将两个col颜色相同的合并 
			que.push(mp(num[t1],-t1));
			cnt--;
		}
		cnt--,ans++;
	}
	cout<<ans<<endl;
	return 0;
}
```