---
subtitle: "毫无区分度的手速场"
tags: 
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P28.jpg"
preview-img: "/img/preview/P28.jpg"
---
# A. Ilya and a Colorful Walk

## 题意

给出长度为$n$的序列$C_i$，寻找两个节点$(x,y)$使得$C_x!=C_y$且$y-x+1$（即两点间距离）最大

## 分析

贪心两头各查找一遍

答案一定包含左右两个端点中的一个

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<cstdlib>
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
const int maxn=3e5+6;
int n,a[maxn],ans=0;
int main(){
    n=read();
    rep(i,1,n)a[i]=read();
    int p=1,q=n;
    while(p<=q){
        if(a[p]!=a[q]){ans=q-p;break;}
        if(a[p+1]!=a[q]){p++;continue;}
        if(a[q-1]!=a[p]){q--;continue;}
        p++,q--;
    }
    rep(i,1,n-1)if(a[i]!=a[n])ans=max(ans,n-i);
    dep(i,n,2)if(a[i]!=a[1])ans=max(ans,i-1);
    cout<<ans<<endl;
    return 0;
}
```

# B. Alyona and a Narrow Fridge

## 题意

给出$2\times H$的面积，每一个高度可以放置甲板，同样给出$n$个物体的高度$a_i$，问最多可以在这么大的规模内放下**前**多少个物体（注意是按给定读入顺序的前$K$个）

## 分析

二分答案

之后每次重新排序贪心检验

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<cstdlib>
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
#define mid ((L+R)>>1)
const int maxn=1e3+6;
int a[maxn],b[maxn],n;
ll K;
inline bool check(int x){
    ll sum=0;
    rep(i,1,x)b[i]=a[i];
    sort(b+1,b+1+x);
    for(int i=x;i>=1;i-=2)sum+=b[i];
    if(sum>K)return 0;else return 1;
}
int main(){
    n=read(),K=read();
    rep(i,1,n)a[i]=read();
    int L=1,R=n,ans=0;
    while(L<=R){
        if(check(mid))ans=mid,L=mid+1;else R=mid-1;
    }
    cout<<ans<<endl;
    return 0;
}
```

# C. Ramesses and Corner Inversion

## 题意

给出两个大小为$N\times M$的两个$01$矩阵，每次可以选择一个子矩阵，将子矩阵的四个顶点异或（取反），询问两个矩阵是否可以通过若干次变换相互转化

## 分析

并不会严谨的数学分析qwq

只是凭感觉YY（猜测）

只需要判断下每行每列的异或和是否相等

如果存在不等的情况，那么无论如何选择子矩阵，因为变换的坐标始终都是同一行（或同一列），所以始终无法改变其每行每列单独的异或和

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<cstdlib>
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
const int maxn=1e3+6;
int n,m,s1[maxn],s2[maxn],s3[maxn],s4[maxn],a[maxn][maxn],b[maxn][maxn];
int main(){
    n=read(),m=read();
    rep(i,1,n)rep(j,1,m)cin>>a[i][j];
    rep(i,1,n)rep(j,1,m)cin>>b[i][j];
    rep(i,1,n){
        rep(j,1,m)s1[i]=s1[i]^a[i][j],s3[i]=s3[i]^b[i][j];
    }
    rep(j,1,m){
        rep(i,1,n)s2[j]=s2[j]^a[i][j],s4[j]=s4[j]^b[i][j];
    }
    rep(i,1,n)if(s1[i]!=s3[i]){puts("No");return 0;}
    rep(i,1,m)if(s2[i]!=s4[i]){puts("No");return 0;}
    puts("Yes");
    return 0;
}
```

# D. Frets On Fire

## 题意

给出长度为$n$的序列$S_i$，以及$Q$次询问，每次询问给出$l_k,r_k$两个参数，求出对于所有的$S_i$，在其$S_i+l_k\leq X \leq S_i+r_k$中有多少个不同取值的$X$

## 分析

先将序列$S$去重，之后求出差分序列

再将差分序列排序后做一遍前缀和

在查询的时候可以二分查询当前的位置，所以可以$O(\log n)$回答

详见代码

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll unsigned long long
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
const int maxn=2e5+6;
int n,Q,cnt=0;
ll a[maxn],b[maxn],c[maxn],d[maxn];
int main(){
    n=read();
    rep(i,1,n)a[i]=read();
    sort(a+1,a+1+n);
    a[0]=-1;
    rep(i,1,n)if(a[i]!=a[i-1])b[++cnt]=a[i];
    sort(b+1,b+1+cnt);
    rep(i,1,cnt-1)c[i]=b[i+1]-b[i];
    sort(c+1,c+cnt);
    //rep(i,1,cnt-1)cout<<c[i]<<' ';cout<<endl;
    rep(i,1,cnt-1)d[i]=d[i-1]+c[i];
    Q=read();
    while(Q--){
        ll l=read(),r=read(),len=r-l+1;
        int pos=upper_bound(c+1,c+cnt,len)-c;
        pos--;
        ll ans=(ll)(1ll*len*(ll)(cnt-pos)+1ll*d[pos]);
        cout<<ans<<' ';
    }
    return 0;
}
```

# 剩下的待填坑
