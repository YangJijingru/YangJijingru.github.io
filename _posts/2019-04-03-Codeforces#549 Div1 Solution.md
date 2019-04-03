---
subtitle: "正经的Div 1场"
tags: 
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P30.jpg"
preview-img: "/img/preview/P30.jpg"
---
# A. The Beatles

## 题意

给一个有$n\times k$个点的环，环上从$1$开始，每$k$个点有一个餐厅，每次可以单向移动$L$个点，现在给出$n$和$k$以及$a,b$（分别表示初始位置离最近的餐厅的距离和第一次移动后离最近的餐厅的距离），询问最少和最多要移动多少次才能够回到初始位置

## 分析

考虑$L$存在的两种情况

- $L=m\times k +b-a$
- $L=m\times k -b-a$

其中$m\in [1,n]$

那么移动的次数$ans=\frac {lmc(n\times k , l)} {l} = \frac {n\times k} {gcd(n\times k , l)}$

可以直接暴力枚举

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
ll n,k,l1,l2,a,b,mn=6e18,mx=0;
inline ll gcd(ll x,ll y){return y?gcd(y,x%y):x;}
int main(){
    n=read(),k=read(),a=read(),b=read();
    l1=b-a,l2=k-b-a;
    if(l1<=0)l1+=k;
    if(l2<=0)l2+=k;
    rep(i,0,n-1){
        mn=min(mn,min(n*k/gcd(n*k,l1),(n*k/gcd(n*k,l2))));
        mx=max(mx,max(n*k/gcd(n*k,l1),(n*k/gcd(n*k,l2))));        
        l1+=k,l2+=k;
    }
    cout<<mn<<' '<<mx<<endl;
    return 0;
}
```

# B. Lynyrd Skynyrd

## 题意

给出长度为$m$的序列$p$，和长度为$n$的序列$a$

给出$Q$次询问，每次询问序列$a$的区间$[l_i,r_i]$中是否存在子序列为序列$p$的循环移位

e.g. 循环移位：比如$(2,1,3)$，那么它存在三个等价的循环$(2,1,3)(1,3,2)(3,2,1)$

## 分析

维护每个元素按照循环排列的下一个元素

之后对每个左端点求出离其最近的右端点位置

但如果暴力查询的话时间复杂度为$O(n\times m)$

可以使用倍增进行优化（因为是连续寻找前驱的操作）

需要倒着循环一遍取后缀最小值

每次询问可以直接$O(1)$判断是否超过最近的右端点即可

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
const int maxn=2e5+6;
int n,m,Q,p[maxn],a[maxn],fi[maxn],nxt[maxn][26],lst[maxn],minr[maxn];
int main(){
    n=read(),m=read(),Q=read();
    rep(i,1,n)p[i]=read();
    rep(i,1,m)a[i]=read();
    rep(i,1,n-1)fi[p[i]]=p[i+1];fi[p[n]]=p[1];
    rep(i,1,n)lst[i]=m+1;
    nxt[m+1][0]=m+1;
    dep(i,m,1)nxt[i][0]=lst[fi[a[i]]],lst[a[i]]=i;
    rep(j,1,19)rep(i,1,m+1)
        nxt[i][j]=nxt[nxt[i][j-1]][j-1];
    rep(i,1,m){
        int now=n-1,r=i;
        dep(j,19,0)if(now-(1<<j)>=0)now-=(1<<j),r=nxt[r][j];
        minr[i]=r;
    }
    dep(i,m-1,1)minr[i]=min(minr[i],minr[i+1]);
    while(Q--){
        int l=read(),r=read();
        if(r>=minr[l])printf("%d",1);else printf("%d",0);
    }
    return 0;
}
```

# C. U2

## 题意

给出形如$y=x^2 + bx + c$的二次函数图象（注意没有$a$）

给出$n$个点的坐标，任意两个点可以确定一个二次函数图像

询问在构成的图像中有多少个图像其内部没有顶点

## 分析

$(x,y)->(x,y-x^2)$

转化之后可以求凸包了

或者可以直接魔改凸包，改成判断是否在抛物线内部即可

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
#define eps 1e-7
const int maxn=2e5+6;
int n,tot=0;
struct P{double x,y;}a[maxn],p[maxn],s[maxn];
inline bool cmp(P a,P b){return a.x==b.x?a.y<b.y:a.x<b.x;}
inline P operator - (P a,P b){P t;t.x=a.x-b.x;t.y=a.y-b.y;return t;}
inline ll operator * (P a,P b){return a.x*b.y-a.y*b.x;}
inline ll dis(P a,P b){return (a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y);}
/*inline bool operator < (P a,P b){
    ll t=(a-p[1])*(b-p[1]);
    if(t==0)return dis(p[1],a)<dis(p[1],b);else return t>0;
}
*/
int main(){
    n=read();
    rep(i,1,n){
        scanf("%lf%lf",&a[i].x,&a[i].y);
        a[i].y=a[i].y-1ll*a[i].x*a[i].x;
    }
    sort(a+1,a+1+n,cmp);
    rep(i,1,n-1)if(fabs(a[i].x-a[i+1].x)>eps)p[++tot]=a[i];
    p[++tot]=a[n];
    int top=0;
    dep(i,tot,1){
        while(top>1&&((s[top]-s[top-1])*(p[i]-s[top-1])<=0))top--;
        s[++top]=p[i];
    }
    cout<<top-1<<endl;
    return 0;
}
```

# 皮一下

Wo sind Aufgabe D und E????

Kein Ahnung!

剩下的D和E咕了