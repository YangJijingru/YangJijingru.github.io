---
tags: 
 - splay
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P2.jpg"
preview-img: "/img/preview/P42.jpg"
---
标签：splay

# 题目

本题同BZOJ3506

[题目传送门](http://www.lydsy.com/JudgeOnline/problem.php?id=1552)

Description

Input
输入共两行，第一行为一个整数N，N表示物品的个数，1<=N<=100000。
第二行为N个用空格隔开的正整数，表示N个物品最初排列的编号。
Output
输出共一行，N个用空格隔开的正整数P1,P2,P3…Pn，Pi表示第i次操作前第i小的物品所在的位置。 
注意：如果第i次操作前，第i小的物品己经在正确的位置Pi上，我们将区间[Pi,Pi]反转(单个物品)。
Sample Input
6

3 4 5 1 6 2
Sample Output
4 6 4 5 6 6
HINT

Source

HNOI2009集训Day6

# 分析

splay模板（区间翻转）+维护子树内最小值

找到最小值把它翻转到根root的位置然后它左子树节点个数就是当前所处位置

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
#define inf 0x7fffffff
using namespace std;
inline ll read()
{
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
const int maxn=1e5+6;
struct node{int val,pos;}a[maxn];
inline bool operator < (node a,node b){return a.pos<b.pos;}
inline bool cmp (node a,node b){return (a.val<b.val)||(a.val==b.val&&a.pos<b.pos);}
 
int n,root,top,ans[maxn],s[maxn],fa[maxn],c[maxn][2],v[maxn],mn[maxn],size[maxn],pos[maxn];
bool rev[maxn];
 
 
void update(int x)
{
    int l=c[x][0],r=c[x][1];
    size[x]=size[l]+size[r]+1;
    mn[x]=v[x];pos[x]=x;
    if(mn[l]<mn[x]){mn[x]=mn[l];pos[x]=pos[l];}
    if(mn[r]<mn[x]){mn[x]=mn[r];pos[x]=pos[r];}
}
 
void pushdown(int x)
{
    int l=c[x][0],r=c[x][1];
    rev[x]^=1;rev[l]^=1;rev[r]^=1;
    swap(c[x][0],c[x][1]);
}
 
void rotate(int x,int &k)
{
    int y=fa[x],z=fa[y],l,r;
    if(c[y][0]==x)l=0;else l=1;r=l^1;
    if(y==k)k=x;
    else{if(c[z][0]==y)c[z][0]=x;else c[z][1]=x;}
    fa[x]=z;fa[y]=x;fa[c[x][r]]=y;
    c[y][l]=c[x][r];c[x][r]=y;
    update(y);update(x);
}
 
void splay(int x,int &k)
{
    top=0;s[++top]=x;
    for(int i=x;fa[i];i=fa[i])s[++top]=fa[i];
    dep(i,top,1)
        if(rev[s[i]])pushdown(s[i]);
    while(x!=k){
        int y=fa[x],z=fa[y];
        if(y!=k){
            if(c[y][0]==x^c[z][0]==y)rotate(x,k);
            else rotate(y,k);
        }
        rotate(x,k);
    }
}
 
void build(int l,int r,int f)
{
    if(l>r)return;
    if(l==r){
        fa[l]=f;size[l]=1;
        if(l<f)c[f][0]=l;else c[f][1]=l;
        mn[l]=v[l]=a[l].val;pos[l]=l;
        return;
    }
    int mid=(l+r)>>1;
    build(l,mid-1,mid);build(mid+1,r,mid);
    fa[mid]=f;v[mid]=a[mid].val;update(mid); 
    if(mid<f)c[f][0]=mid;else c[f][1]=mid;
}
 
int find(int x,int rank)
{
    if(rev[x])pushdown(x);
    int l=c[x][0],r=c[x][1];
    if(size[l]+1==rank)return x;
    else if(size[l]>=rank)return find(l,rank);
    else return find(r,rank-size[l]-1);
}
 
void rever(int l,int r)
{
    int x=find(root,l),y=find(root,r+2);
    splay(x,root);splay(y,c[x][1]);
    int z=c[y][0];
    rev[z]^=1;
}
 
int querymn(int l,int r)
{
    int x=find(root,l),y=find(root,r+2);
    splay(x,root);splay(y,c[x][1]);
    int z=c[y][0];
    return pos[z];
}
 
int main()
{
    n=read();root=(n+3)>>1;
    a[1].val=a[n+2].val=inf;mn[0]=inf; 
    rep(i,2,n+1)a[i].val=read(),a[i].pos=i;
    sort(a+2,a+2+n,cmp);
    rep(i,2,n+1)a[i].val=i;
    sort(a+2,a+2+n);
    build(1,n+2,0);
    rep(i,1,n){
        int x=querymn(i,n);
        splay(x,root);
        ans[i]=size[c[x][0]];
        rever(i,ans[i]);
    }
    rep(i,1,n-1)printf("%d ",ans[i]);printf("%d",ans[n]);
    return 0;
}
```

