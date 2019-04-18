---
subtitle: "出题越来越神仙的USACO"
tags: 
 - USACO
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P27.jpg"
preview-img: "/img/preview/P27.jpg"
---

# [USACO19FEB]Cow Dating

## 题意

给出$N$个接受邀请的概率$p_i$，询问子区间内恰好只有一个奶牛接受邀请的最大概率

## 分析

区间$[l,r]$的答案为$(1-p_l)\times(1-p_{l+1})\times ... \times (1-p_{r}) \times (\frac {p_l} {1-p_l} + \frac {p_{l+1}} {1-p_{l+1}} + ... + \frac {p_n} {1-p_n})$

可以用尺取法优化到$O(N)$的时间复杂度

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define mem(x,num) memset(x,num,sizeof x)
#define ll long long
#define reg(i,x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=1e6+6;
int n;
double p[maxn],ans=0.0,tmp1=1,tmp2;
int main(){
    n=read();
    rep(i,1,n)p[i]=1.0*read()/1e6,ans=max(ans,p[i]);
    //rep(i,1,n)printf("%.4lf\n",p[i]);
    int j=1;
    rep(i,1,n){
        while(j<=n&&tmp1*tmp2<tmp1*(1-p[j])*(tmp2+p[j]/(1-p[j]))){
            tmp1*=(1-p[j]);
            tmp2+=p[j]/(1-p[j]);
            j++;
        }
        ans=max(ans,tmp1*tmp2);
        tmp1/=(1-p[i]);
        tmp2-=p[i]/(1-p[i]);
    }
   // cout<<ans<<endl;
    cout<<(int)(ans*1e6)<<endl;
    return 0;
}
```

# [USACO19FEB]Moorio Kart

## 题意

给出若干棵树，选择部分节点连接成环，使得环长度不小于$Y$得方案数

## 分析

考虑利用容斥的思想，所有的路径减去不合法的路径的长度

所有路径的长度较为容易计算，只需要计算每一条边被统计的次数

可以先暴力dfs统计出每个树内部的路径

$f[i][0/1]$表示当前路径和为$i$的方案数/方案权值之和

$g[i][0/1]$表示当前这棵树内部的长度为$i$的方案数/方案权值之和

$f[i+j][0]=f[i][0]\times g[j][0]$

$f[i+j][1]=f[i][0]\times g[j][1] + f[i][1]\times g[j][0]$

时间复杂度$O(N\times Y^{1.5})$

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define mem(x,num) memset(x,num,sizeof x)
#define ll long long
#define reg(i,x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=3e3+6;
const ll mod=1e9+7;
struct edge{int to,next,w;}e[maxn<<1];
int last[maxn],fa[maxn],bel[maxn],cnt=0,tot=0,n,m,now,X,Y;
ll sum[maxn][maxn],num[maxn][maxn],g[maxn][2],f[maxn][2],ans;
inline int find(int x){return x==fa[x]?fa[x]:fa[x]=find(fa[x]);}
void insert(int u,int v,int w){e[++cnt]=(edge){v,last[u],w};last[u]=cnt;}
void build(int x,int fr,int id){
    bel[x]=id;
    reg(i,x)if(e[i].to!=fr)build(e[i].to,x,id);
}
void dfs(int x,int fr,int l){
    if(fr){sum[bel[x]][min(l,Y)]=(sum[bel[x]][min(l,Y)]+l)%mod;++num[bel[x]][min(l,Y)];}
    reg(i,x)if(e[i].to!=fr)dfs(e[i].to,x,l+e[i].w);
}
int main(){
    n=read(),m=read(),X=read(),Y=read();
    rep(i,1,n)fa[i]=i;
    rep(i,1,m){
        int u=read(),v=read(),w=read();
        insert(u,v,w);insert(v,u,w);
        int r1=find(u),r2=find(v);
        if(r1!=r2)fa[r1]=r2;
    }
    rep(i,1,n)if(find(i)==i)build(i,0,++tot);
    rep(i,1,n)dfs(i,0,0);
    now=min(tot*X,Y);
    f[now][0]=1;f[now][1]=tot*X;
    rep(i,1,tot){
        rep(j,now,Y)g[j][0]=f[j][0],g[j][1]=f[j][1],f[j][0]=f[j][1]=0;
        rep(j,0,Y)
            if(num[i][j])
                rep(k,now,Y)
                    if(g[k][0]){
                        f[min(j+k,Y)][0]=(f[min(j+k,Y)][0]+1ll*g[k][0]*num[i][j])%mod;
                        f[min(j+k,Y)][1]=(f[min(j+k,Y)][1]+1ll*g[k][0]*sum[i][j]+1ll*g[k][1]*num[i][j])%mod;
                    }
    }
    ans=f[Y][1];
    cout<<ans<<endl;
    rep(i,1,tot-1)ans=ans*i%mod;
    cout<<1ll*ans*(mod+1)/2%mod<<endl;
    return 0;
}
```

# [USACO19FEB]Mowing Mischief

## 题意

给出规模为$T\times T$的平面直接坐标系和$N$个点坐标，其中两个人从$(0,0)$行走至$(T,T)$，行走过程中必须要经过这$N$个点，最大化两个路径形成的相交矩形面积和

## 分析

首先可以将点排序分层，这样保证后面的点不会对前面的点最优值状态造成影响

$dp[i]=\min \{ dp[j]+(x_i-x_j)\times (y_i-y_j) \} (x_j\leq x_i,y_j\leq y_i)$

将式子展开后

$dp[i]=\min \{ dp[j]+x_jy_j-x_iy_j-x_jy_i \}+x_iy_i (x_j\leq x_i,y_j\leq y_i)$

考虑决策单调性，令在$a$处优于$b$处的转移且$a<b$

$x_i \times (y_b - y_a) + y_i \times (x_b - x_a) < dp_b + x_b \times y_b - dp[a] - x_a \times y_a$

对于等式左边，$x_b-x_a>0,y_b-y_a<0$，而$i$越大$y_i$越小，$x_i$越大，所以左边会越来越小，不等式更容易满足

所以对于两个决策$a$和$b$，一定存在一个分界点，在分界点之前$b$更优，分界点之后$a$更优，即该dp具有决策单调性

因为有对于X和Y的大小限制，所以每个点可以考虑的决策是一个区间

可以直接用线段树维护决策队列来优化

具体而言，线段树的每个节点维护该区间中所有决策的决策队列

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<vector>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define mem(x,num) memset(x,num,sizeof x)
#define ll long long
#define reg(i,x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int maxn=2e5+6;
const ll inf=1e18;
int n,m,tot,f[maxn];
ll dp[maxn];
vector<int> g[maxn];
struct node{
    int x,y;
    inline bool operator < (const node &u)const{return u.x==x?y<u.y:x<u.x;}
    inline bool operator > (const node &u)const{return ((x>=u.x)&&(y>=u.y));}
}a[maxn];
namespace BIT{
    #define lowbit(x) (x&(-x))
    int tree[maxn<<3];
    inline void modifybit(int x,int Val){
        x=max(x,1);
        for(int i=x;i<=m;i+=lowbit(i))tree[i]=max(tree[i],Val);
    }
    inline int querybit(int x){
        int re=0;
        for(int i=x;i;i-=lowbit(i))re=max(re,tree[i]);
        return re;
    }
}
using namespace BIT;
namespace SEG{
    #define ls (x<<1)
    #define rs (x<<1|1)
    #define mid ((l+r)>>1)
    #define MID ((L+R)>>1)
    vector<int>p[maxn<<2];
    vector<int>now,s;
    void build(int x,int l,int r){
        p[x]=vector<int>();
        if(l==r)return;
        build(ls,l,mid);build(rs,mid+1,r);
    }
    void init(int x){
        now=g[x];
        build(1,0,now.size()-1);
    }
    void insert(int x,int l,int r,int k){
        if(a[k].x>=a[now[r]].x&&a[k].y>=a[now[l]].y){
            p[x].push_back(k);
            return;
        }
        if(a[k].x<a[now[l]].x||a[k].y<a[now[r]].y||l==r)return;
        insert(ls,l,mid,k);insert(rs,mid+1,r,k);
    }
    void queryseg(int l,int r,int L,int R){
        if(l>r)return;
        ll ans=inf;
        int x=s[mid],pos=0;
        rep(i,L,R){
            ll re=dp[now[i]]+1ll*(a[x].x-a[now[i]].x)*(a[x].y-a[now[i]].y);
            if(re<ans)ans=re,pos=i;
        }
        dp[x]=min(dp[x],ans);
        queryseg(l,mid-1,pos,R);
        queryseg(mid+1,r,L,pos);
    }
    void work(int x,int l,int r){
        if(!p[x].empty()){
            s=p[x];
            queryseg(0,s.size()-1,l,r);
        }
        if(l==r)return;
        work(ls,l,mid);work(rs,mid+1,r);
    }
}
using namespace SEG;
void solve(int x){
    init(x);
    for(int i=0,sz=g[x+1].size(),ssz=g[x].size();i<sz;i++)insert(1,0,ssz-1,g[x+1][i]);
    work(1,0,g[x].size()-1);
}
int main(){
    n=read(),m=read();
    rep(i,1,n)a[i].x=read(),a[i].y=read();
    a[++n]=(node){0,0};
    a[++n]=(node){m,m};
    sort(a+1,a+1+n);
    rep(i,1,n){
        f[i]=querybit(a[i].y)+1;
        modifybit(a[i].y,f[i]);
        tot=max(tot,f[i]);
        g[f[i]].push_back(i);
    }
    rep(i,1,n)dp[i]=1ll*a[i].x*a[i].y;
    rep(i,1,tot-1)solve(i);
    cout<<dp[n]<<endl;
    return 0;
}
```