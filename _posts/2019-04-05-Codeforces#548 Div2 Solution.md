---
subtitle: "难度略微毒瘤的Div 2"
tags: 
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P29.jpg"
preview-img: "/img/preview/P29.jpg"
---
# A. Even Substrings

## 题意

给出数字字符串，求有多少数字子串代表的数字为偶数

## 分析

判断如果当前数字为偶数，那么答案加上当前的位数

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
int n,ans=0;
int main(){
    n=read();
    rep(i,1,n){
        char ch=getchar();
        if((ch-'0')%2==0)ans+=i;
    }
    cout<<ans<<endl;
    return 0;
}
```

# B. Chocolates

## 题意

求每个元素不超过给定上限的严格上升序列最大和

## 分析

贪心：从后往前选择每一位可能的最大值并累和

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
int a[maxn],tmp=2e9,n;
ll ans=0;
int main(){
    n=read();
    rep(i,1,n)a[i]=read();
    dep(i,n,1)ans+=min(a[i],max(0,tmp-1)),tmp=min(tmp-1,a[i]);//,cout<<ans<<endl;
    cout<<ans<<endl;
    return 0;
}
```

# C. Edgy Trees

## 题意

给出一棵边带有属性的树（要么红色要么黑色），统计至少为$K$个节点并包含至少一条黑边的路径数量

## 分析

容斥原理，可以求出全部为红边的集合

删除黑边求出只包含红边的联通块，对于每个联通块会有$x^K$不合法的路径

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
const ll mod=1e9+7;
struct edge{int to,next,w;}e[maxn<<3];
int last[maxn],n,K,tmp=0,cnt=0,b[maxn];
bool vis[maxn];
ll ans;
void insert(int u,int v,int w){e[++cnt]=(edge){v,last[u],w};last[u]=cnt;}
inline ll qpow(ll x,ll y){
    ll re=1;
    x%=mod;
    while(y){
        if(y&1)re=(re*x)%mod;
        y>>=1,x=(x*x)%mod;
    }
    return re;
}
void dfs(int x){
    tmp++;
    vis[x]=1;
    reg(x)
        if(!vis[e[i].to]&&!e[i].w)dfs(e[i].to);
}
int main(){
    n=read(),K=read();
    ans=qpow(n,K);
    rep(i,1,n-1){
        int u=read(),v=read(),w=read();
        insert(u,v,w);insert(v,u,w);
    }
    rep(i,1,n)if(!vis[i]){dfs(i);b[++cnt]=tmp;tmp=0;}
    rep(i,1,cnt)ans=((ans-qpow(b[i],K)+mod)%mod+mod)%mod;
    cout<<ans<<endl;
    return 0;
}
```

# D. Steps to One

## 题意

每次从$1-m$中随机取出一个数，向集合$s$中添加数字，直至集合中所有的数的$gcd$为$1$，求出问集合大小的期望

## 分析

集合大小的期望等价于在加最后一个数字之前所有的数字的公因数为不同的值的集合大小的的和

可以使用莫比乌斯函数反演之后求和

$f_i$表示写的序列最大公约数已经等于$i$的情况下写到结束为止的期望长度

$f_i=1+\frac 1 m \sum_{j=1}^n f_{gcd(i,j)}$

移项整理之后发现直接DP的复杂度会是$O(m^2)$

可以统计一下$i$的每个约数对$i$的贡献

先去枚举作为约数的$x$，再去枚举$x$的倍数$n$

即$\sum_{i=1}^m [(n,i)==x]$

反演之后变成$\sum_{d|n} \mu(d) \lfloor \frac m d\rfloor$

于是只需要预处理$n$的约数

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
const ll mod=1e9+7;
int m,prime[maxn],cnt=0,mu[maxn],is_prime[maxn]={0};
void Pre(){
    mu[1]=1;
    rep(i,2,m){
        if(!is_prime[i])prime[++cnt]=i,mu[i]=-1;
        rep(j,1,cnt){
            if(prime[j]*i>m)break;
            is_prime[i*prime[j]]=1;
            if(i%prime[j]==0){mu[i*prime[j]]=0;break;}else mu[i*prime[j]]=-mu[i];
        }
    }
}
inline ll qpow(ll x,ll y){
    ll re=1;
    while(y){
        if(y&1)re=(re*x)%mod;
        y>>=1,x=(x*x)%mod;
    }
    return re;
}
int main(){
    m=read();
    Pre();
    ll ans=1;
    rep(i,2,m+1)if(mu[i])ans=(ans-mu[i]*(m/i)*qpow(m-m/i,mod-2)+mod)%mod;
    cout<<ans<<endl;
    return 0;
}
```

# E. Maximize Mex

## 题意

每个人存在一个权值$p$，每个人又归属于一个集合$c$，$K$次操作每次删除一个人，之后求出每个集合选一个人组成集合的最小非负数的最大值

## 分析

如果只询问一次且不删点的话那么只需要跑一边二分图

我们可以将删点变成不停加点的过程（倒着跑），每次在上一次的基础上跑一次二分图即可

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
const int maxn=1e4+6;
int n,m,D,tot,cnt=0,base=5000,link[maxn],last[maxn],val[maxn],group[maxn],le[maxn],ans[maxn],vis[maxn];
bool tmp[maxn];
struct edge{int to,next;}e[maxn<<1];
void insert(int u,int v){e[++cnt]=(edge){v,last[u]};last[u]=cnt;}
inline bool dfs(int x){
    reg(x){
        if(vis[e[i].to]==tot)continue;
        vis[e[i].to]=tot;
        if(link[e[i].to]==-1||dfs(link[e[i].to])){
            link[e[i].to]=x;
            return 1;
        }
    }
    return 0;
}
inline int calc(int x){
    //cout<<x<<"E\n";
    rep(i,x,maxn-1){
        if(link[i]==-1){
            tot++;
            if(!dfs(i))return i;
        }
    }
}
int main(){
    rep(i,0,maxn-1)link[i]=-1;
    n=read(),m=read();
    rep(i,1,n)val[i]=read();
    rep(i,1,n)group[i]=read();
    D=read();
    rep(i,1,D)tmp[le[i]=read()]=1;
    rep(i,1,n)if(!tmp[i])insert(val[i],group[i]+base);
    //puts("R");
    dep(i,D,1){
        ans[i]=calc(ans[i+1]);
        int idx=le[i];
        insert(val[idx],group[idx]+base);
    }
    rep(i,1,D)printf("%d\n",ans[i]);
    return 0;
}
```
