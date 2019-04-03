---
subtitle: "感觉十分逆区分度的ARC"
tags: 
 - Atcoder
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P31.jpg"
preview-img: "/img/preview/P31.jpg"
---
# A	Regular Triangle

## 题意

确定边长为$A,B,C$的三角形是否为等边三角形

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
ll a,b,c;
int main(){
    a=read(),b=read(),c=read();
    if(a==b&&a==c&&b==c)puts("Yes");else puts("No");
    return 0;
}
```

# B	Red or Blue

## 题意

判断字符串中'R'和'B'出现的数量大小

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
int n,s1=0,s2=0;char ch;
int main(){
    n=read();
    rep(i,1,n){
        cin>>ch;
        if(ch=='B')s1++;else s2++;
    }
    if(s2>s1)puts("Yes");else puts("No");
    return 0;
}

```

# C	Snuke the Wizard

## 题意

给出一排带有属性的格子（全部使用小写字母表示），初始时每个格子上都有一枚硬币

给出$Q$次操作，每次将某种属性的格子上的硬币全部左移或右移一位

移出边界就消失，询问最后剩余的硬币数量

## 分析

观察后发现移出去的一定是一段前缀和一段后缀，答案具有二分的可行性

因为硬币的相对顺序不变，所以可以直接二分边界

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
#define mid ((l+r)>>1)
const int maxn=2e5+6;
char st[maxn],t[maxn],d[maxn];
int n,Q;
inline int check(int x){
    rep(i,1,Q){
        if(st[x]==t[i])x+=(d[i]=='R')?1:-1;
        if(x>n)return -1;
        if(x<1)return 1;
    }
    return 0;
}
int main(){
    n=read(),Q=read();
    scanf("%s",st+1);
    rep(i,1,Q)cin>>t[i]>>d[i];
    int l=1,r=n,la=0,ra=n+1;
    while(l<=r)if(check(mid)==1)la=mid,l=mid+1;else r=mid-1;
    l=1,r=n;
    while(l<=r)if(check(mid)==-1)ra=mid,r=mid-1;else l=mid+1;
    cout<<ra-la-1<<endl;
    return 0;
}
```

# D	Modulo Operations

## 题意

给出一个长度为$n$的数列和数字$X$，对于数列的每一种排列，其权值$X$依次对排列中的数取模，求出$n!$种情况最后剩下的数的权值和

## 分析

先要知道一个结论：如果大的数排在小的数字后面，那么大的数字对答案无影响

可以将数列从大到小排序

设$f[i][j]$表示做了$i$次操作，剩余数为$j$的方案数

转移要分为两种情况

- 选择：$f[i][j\mod a[i]]=f[i][j\mod a[i]]+f[i-1][j]$
- 不选：$f[i][j]=f[i-1][j]\times (n-i)$

不选的话即可放在后面$n-i$中的任意一个位置

## Code
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#define rep(i,a,b) for(register int i=a;i<=b;i++)
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
const int maxn=2e2+6,maxX=1e5+6;
const ll mod=1e9+7;
int n,X;
ll f[maxn][maxX],a[maxn],ans=0;
inline bool cmp(ll x,ll y){return x>y;}
int main(){
    n=read(),X=read();
    rep(i,1,n)a[i]=read();
    sort(a+1,a+1+n,cmp);
    f[0][X]=1;
    rep(i,1,n)
        rep(j,0,X){
            f[i][j%a[i]]=((f[i][j%a[i]]+f[i-1][j])%mod+mod)%mod;
            f[i][j]=((f[i][j]+1ll*f[i-1][j]*(n-i))%mod+mod)%mod;
        }
    rep(i,0,X)ans=((ans+1ll*f[n][i]*i%mod)+mod)%mod;
    cout<<ans<<endl;
    return 0;
}
```

# E	Black or White

## 题意

存在一些黑球和白球，两种球都存在的话则各有一半的几率取出，否则一定取出剩下的那种颜色的球

求第$i$次拿到黑球的概率

## 分析

先不妨假设白球先被拿完，则概率为$g_i=2^{i}{i-1 \choose W-1}$

如果前$i$次把白球拿完的概率为$g_i$，把黑球拿完的概率为$f_i$，那么第$i+1$次取出黑球的概率为$g_i+(1-g_i-f_i)\times \frac 1 2$

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
const int mod=1e9+7,maxn=2e5+6;
int fac[maxn],inv[maxn],bit[maxn],B,W,sw[maxn],sb[maxn];
inline int C(int n,int k){return 1ll*fac[n]*inv[k]%mod*inv[n-k]%mod;}
inline int qpow(int x,int y){
    int re=1;
    while(y){
        if(y&1)re=(1ll*re*x)%mod;
        x=(1ll*x*x)%mod,y>>=1;
    }
    return re;
}
int main(){
    B=read(),W=read();
    fac[0]=bit[0]=inv[0]=fac[1]=inv[1]=1;
    rep(i,2,B+W)fac[i]=1ll*fac[i-1]*i%mod;
    rep(i,2,B+W)inv[i]=qpow(fac[i],mod-2);
    rep(i,1,B+W)bit[i]=1ll*bit[i-1]*inv[2]%mod;

    rep(i,W,B+W-1)sw[i]=1ll*bit[i]*C(i-1,i-W)%mod;
    rep(i,B,B+W-1)sb[i]=1ll*bit[i]*C(i-1,i-B)%mod;
    
    rep(i,1,B+W)sw[i]=(sw[i]+sw[i-1])%mod,sb[i]=(sb[i]+sb[i-1])%mod;
    rep(i,1,B+W){
        int w=(sw[i-1]+1ll*((mod+1-sw[i-1]-sb[i-1])%mod+mod)%mod*(i==B+W?1:inv[2])%mod)%mod;
        printf("%d\n",w);
    }
    return 0;
}
```

# F题暂时鸽了（咕咕咕）

感觉这场ARC区分度很醚

A&&B简直是CF Div 3????

C一看感觉不可做就自闭放弃了

然后剩下时间都在肝D（感觉难度突然到Div 1???）

而且还在不停地写假算法（很天真地认为自己可以过D）

还有最近比赛的教训：一些表面上很复杂的题目可能比较容易拿分