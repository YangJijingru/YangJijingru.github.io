---
subtitle: "花式树上贪心乱搞"
tags: 
 - 树-杂题
 - HDU
 - 基础算法-贪心
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P38.jpg"
preview-img: "/img/preview/P38.jpg"
---

标签：贪心

# 题目

[题目传送门](http://acm.hdu.edu.cn/showproblem.php?pid=4582)

# 题意

给出一张无向图（包括一棵dfs树和若干条反向边），请你选取最少的边使得所有只包含一条反向边的环被完全覆盖

# 分析

花式贪心

尽量选择深度较小（靠上/较高）的树边

从底向上每次遇到反向边则记录，判断当前节点到其父节点的边必选性，如果非必须则向上传递区间

用了bitset鬼畜加速

# Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<bitset>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg1(x) for(int i=last[x];i;i=e[i].next)
#define reg2(x) for(int i=pre[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=2e3+6;
int pre[maxn<<2],last[maxn<<2],cnt=0,ans,n,m;
bool vis[maxn];
struct edge{int to,next;}e[maxn<<5];
bitset<maxn> To[maxn];
void insert1(int u,int v){e[++cnt]=(edge){v,last[u]};last[u]=cnt;}
void insert2(int u,int v){e[++cnt]=(edge){v,pre[u]};pre[u]=cnt;}
void dfs(int x,int fa){
    vis[x]=1;
    To[x].reset();
    reg2(x)if(vis[e[i].to])To[x].set(e[i].to);
    reg1(x){if(e[i].to==fa)continue;dfs(e[i].to,x);}
    //cout<<x<<' '<<fa<<"R"<<endl;
    if(fa==0)return;
    if(To[x].test(fa))ans++;else To[fa]|=To[x];
}
int main(){
    n=read(),m=read();
    while(n&&m){
        mem(last,0);mem(pre,0);cnt=0;mem(e,0);mem(vis,0);
        rep(i,1,n-1){
            int u=read(),v=read();
            insert1(u,v);insert1(v,u);
        }
        rep(i,n,m){
            int u=read(),v=read();
            insert2(u,v);insert2(v,u);
        }
        //cout<<cnt<<endl;
        ans=0;
        dfs(1,0);
        printf("%d\n",ans);
        n=read(),m=read();
    }
    return 0;
}
```