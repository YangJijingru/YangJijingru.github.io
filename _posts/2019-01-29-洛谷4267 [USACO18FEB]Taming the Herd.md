---
subtitle: "考验预处理DP题"
tags: 
 - DP-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P53.jpg"
preview-img: "/img/preview/P53.jpg"
---
标签：DP

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P4267)

## 题意翻译
一大清早，Farmer John就被木材破裂的声音吵醒了。是这些奶牛们干的，她们又逃出牛棚了！
Farmer John已经厌烦了奶牛在清晨出逃，他觉得受够了：是时候采取强硬措施了。他在牛棚的墙上钉了一个计数器，追踪从上次出逃开始经过的天数。所以如果某一天早上发生了出逃事件，这一天的计数器就为$0$；如果最近的出逃是$3$天前，计数器读数就为$3$。Farmer John一丝不苟地记录了每一天计数器的读数。

年末到了，Farmer John准备做一些统计。他说，你们这些奶牛会付出代价的！然而他的某些记录看上去不太对劲……

Farmer John想要知道从他开始记录以来发生过多少次出逃。但是，他怀疑这些奶牛篡改了它的记录，现在他所确定的只有他是从发生出逃的某一天开始记录的。请帮助他求出，对于每个从他开始记录以来可能发生的出逃次数，他被篡改了的记录条数的最小值。

输入格式（文件名：taming.in）：
输入的第一行包含一个整数$N$（$1 \leq N \leq 100$），表示从Farmer John开始对奶牛出逃计数器进行计数以来已经经过的天数。
第二行包含$N$个空格分隔的整数。第$i$个整数是一个非负整数$a_i$（不超过$100$），表示在第$i$天计数器的数字是$a_i$，除非奶牛篡改了这一天的记录条目。

输出格式（文件名：taming.out）：
输出包含$N$个整数，每行一个。第$i$个整数为所有发生$i$次出逃的事件序列中，与该事件序列不一致的记录条目条数的最小值。

## 题目描述
Early in the morning, Farmer John woke up to the sound of splintering wood. It was the cows, and they were breaking out of the barn again!
Farmer John was sick and tired of the cows' morning breakouts, and he decided enough was enough: it was time to get tough. He nailed to the barn wall a counter tracking the number of days since the last breakout. So if a breakout occurred in the morning, the counter would be $0$ that day; if the most recent breakout was $3$ days ago, the counter would read $3$. Farmer John meticulously logged the counter every day.

The end of the year has come, and Farmer John is ready to do some accounting. The cows will pay, he says! But something about his log doesn't look quite right...

Farmer John wants to find out how many breakouts have occurred since he started his log. However, he suspects that the cows have tampered with his log, and all he knows for sure is that he started his log on the day of a breakout. Please help him determine, for each number of breakouts that might have occurred since he started the log, the minimum number of log entries that must have been tampered with.

## 输入输出格式
### 输入格式
The first line contains a single integer $N$ ($1 \leq N \leq 100$), denoting the number of days since Farmer John started logging the cow breakout counter.
The second line contains $N$ space-separated integers. The $i$th integer is a non-negative integer $a_i$ (at most $100$), indicating that on day $i$ the counter was at $a_i$, unless the cows tampered with that day's log entry.

### 输出格式

The output should consist of $N$ integers, one per line. The $i$th integer should be the minimum over all possible breakout sequences with $i$ breakouts, of the number of log entries that are inconsistent with that sequence.

## 输入输出样例
### 输入样例#1
```
6
1 1 2 0 0 1
```
### 输出样例#1
```
4
2
1
2
3
4
```
## 说明

If there was only 1 breakout, then the correct log would look like 0 1 2 3 4 5, which is 4 entries different from the given log.

If there were 2 breakouts, then the correct log might look like 0 1 2 3 0 1, which is 2 entries different from the given log. In this case, the breakouts occurred on the first and fifth days.

If there were 3 breakouts, then the correct log might look like 0 1 2 0 0 1, which is just 1 entry different from the given log. In this case, the breakouts occurred on the first, fourth, and fifth days.

And so on.

Problem credits: Brian Dean and Dhruv Rohatgi

# 题解

## 中文

可以用**DP**来解决

首先设计状态 $f[i][j]$ 表示前$i$天内经历$j$次出逃可以取到的最小修改次数

在预处理过程中，可以求出 $b[i][j]$ 表示如果第$i$天出逃，那么到第$j$天需要修改的数量，时间复杂度为$O(N^2)$

之后的状态转移方程为 $f[i][j]=min(f[i][j],f[k][j-1]+b[k+1][i]) (k<i)$

相当于在$i$之前的位置中，找出一个最小的出逃位置

因为状态转移方程中三个变量 $i,j,k$ ，并且都是从$1$持续到$n$的，所以时间复杂度为$O(N^3)$

## English

We can use **Dynamic Programming** to solve this task. Let $DP[i][j]$ means the minimum number of changes that during the first $i$ days there was $j$ breakouts. 

Before the nomal Dynamic Programing, we should get $b[i][j]$ that means the minimum number of changes that if there was a breakout on day $i$, lasted until day $j$.

After that, the most important is **State transition equation**.

$$DP[i][j]=min(DP[i][j],DP[k][j-1]+b[k+1][i]) (k<i)$$

It means before day $i$, look for making a breakout on day $k$ that can let $DP[i][j]$ minimal.

The time Usage must be $O(N^3)$, because there are three Loop Variable $i,j,k$, and every Loop Variable will last from $1$ to $N$.

# Code
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
#define inf 0x3f3f3f
const int maxn=1e3+6;
int f[maxn][maxn],n,a[maxn],b[maxn][maxn];
int main(){
    n=read();
    rep(i,1,n)a[i]=read();
    rep(i,1,n){
        int tmp=0;
        rep(j,i,n){
            if(a[j]!=j-i)tmp++;
            b[i][j]=tmp;
        }
    }
    mem(f,inf);f[0][0]=0;
    rep(i,1,n)
        rep(j,1,i)
            rep(k,0,i-1)f[i][j]=min(f[i][j],f[k][j-1]+b[k+1][i]);
    rep(i,1,n)printf("%d\n",f[n][i]);
    return 0;    
}
```