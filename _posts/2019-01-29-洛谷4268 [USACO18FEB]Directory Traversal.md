---
subtitle: "奶牛树形DP题"
tags: 
 - DP-树形
 - 基础算法-dfs
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P52.jpg"
preview-img: "/img/preview/P52.jpg"
---
标签：树形DP，dfs

# 题目

[题目传送门](https://www.luogu.org/problemnew/show/P4268)

## 题意翻译
奶牛Bessie令人惊讶地精通计算机。她在牛棚的电脑里用一组文件夹储存了她所有珍贵的文件，比如：
```
bessie/
  folder1/
    file1
    folder2/
      file2
  folder3/
    file3
  file4
```
只有一个“顶层”的文件夹，叫做bessie。

Bessie可以浏览任何一个她想要访问的文件夹。从一个给定的文件夹，每一个文件都可以通过一个“相对路径”被引用。在一个相对路径中，符号“..”指的是上级目录。如果Bessie在folder2中，她可以按下列路径引用这四个文件：
```
../file1
file2
../../folder3/file3
../../file4
```
Bessie想要选择一个文件夹，使得从该文件夹出发，对所有文件的相对路径的长度之和最小。

输入格式（文件名：dirtraverse.in）：
第一行包含一个整数N（$2 \leq N \leq 100,000$），为所有文件和文件夹的总数量。为了便于输入，每个对象（文件或文件夹）被赋予一个唯一的1至$N$之间的ID，其中ID 1指的是顶层文件夹。
接下来有$N$行。每行的第一项是一个文件或是文件夹的名称。名称仅包含小写字母a-z和数字0-9，长度至多为16个字符。名称之后是一个整数$m$。如果$m$为0，则该对象是一个文件。如果$m > 0$，则该对象是一个文件夹，并且该文件夹下共有$m$个文件或文件夹。在$m$之后有$m$个整数，为该文件夹下的对象的ID。

输出格式（文件名：dirtraverse.out）：
输出所有文件的相对路径的长度之和的最小值。注意这个值可能超过32位整数的表示范围。

## 题目描述
Bessie the cow is surprisingly computer savvy. On her computer in the barn, she stores all of her precious files in a collection of directories; for example:

```
bessie/
  folder1/
    file1
    folder2/
      file2
  folder3/
    file3
  file4
```

There is a single "top level" directory, called bessie.

Bessie can navigate to be inside any directory she wants. From a given directory, any file can be referenced by a "relative path". In a relative path, the symbol ".." refers to the parent directory. If Bessie were in folder2, she could refer to the four files as follows:

```
../file1
file2
../../folder3/file3
../../file4
```

Bessie would like to choose a directory from which the sum of the lengths of the relative paths to all the files is minimized.

## 输入输出格式
### 输入格式
The first line contains an integer N ($2 \leq N \leq 100,000$), giving the total number of files and directories. For the purposes of input, each object (file or directory) is assigned a unique integer ID between 1 and $N$, where ID 1 refers to the top level directory.
Next, there will be $N$ lines. Each line starts with the name of a file or directory. The name will have only lower case characters a-z and digits 0-9, and will be at most 16 characters long. Following the name is an integer, $m$. If $m$ is 0, then this entity is a file. If $m > 0$, then this entity is a directory, and it has a total of $m$ files or directories inside it. Following $m$ there will be $m$ integers giving the IDs of the entities in this directory.
### 输出格式

Output the minimal possible total length of all relative paths to files. Note that this value may be too large to fit into a 32-bit integer.

## 输入输出样例
### 输入样例#1
```
8
bessie 3 2 6 8
folder1 2 3 4
file1 0
folder2 1 5
file2 0
folder3 1 7
file3 0
file4 0
```
### 输出样例#1
```
42
```
## 说明
This input describes the example directory structure given above.

The best solution is to be in folder1. From this directory, the relative paths are:

```
file1
folder2/file2
../folder3/file3
../file4
```

Problem credits: Mark Gordon

# 题解

设$size[x]$为$x$的子树所包含的叶子节点的个数

$$f[son]=f[x]−(len[son]+1)∗size[son]+3∗(n−size[son])$$

两遍dfs搞定即可

时间复杂度为$O(N)$

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
const int maxn=1e5+6;
int n,cnt=0,tot=0,m,last[maxn],size[maxn],len[maxn];
ll f[maxn],ans,dis[maxn];bool flag[maxn];
char st[256];
struct node{int to,next;}e[maxn<<1];
void insert(int u,int v){e[++cnt]=(node){v,last[u]};last[u]=cnt;}
void dfs1(int x,int fa){
    if(flag[x])return;
    reg(x){
        if(e[i].to==fa)continue;
        dfs1(e[i].to,x);
        size[x]+=size[e[i].to];
        dis[x]+=dis[e[i].to];
    }
    dis[x]+=size[x]*len[x];
}
void dfs2(int x,int fa){
    reg(x){
        if(e[i].to==fa)continue;
        f[e[i].to]=f[x]-size[e[i].to]*len[e[i].to]+3*(tot-size[e[i].to]);
        dfs2(e[i].to,x);
        ans=min(ans,f[e[i].to]);
    }
}
int main(){
    n=read();
    rep(i,1,n){
        cin>>st;m=read();
        len[i]=strlen(st)+1;
        if(!m)++tot,dis[i]=strlen(st),flag[i]=size[i]=1;
        rep(j,1,m){int x=read();insert(i,x);insert(x,i);}
    }
    dfs1(1,1);
    reg(1)dis[1]-=len[1]*size[e[i].to];
    ans=f[1]=dis[1];
    dfs2(1,1);
    cout<<ans<<endl;
    return 0;
}
```