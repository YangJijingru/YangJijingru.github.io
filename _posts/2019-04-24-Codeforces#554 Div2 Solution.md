---
subtitle: "Div2使人自闭TAT"
tags: 
 - Codeforces
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P25.jpg"
preview-img: "/img/preview/P25.jpg"
---
# A. Neko Finds Grapes

## 题意

给出长度为$n$的序列$a_i$和长度为$m$的序列$b_i$，询问最多有多少组$a_i+b_j$和为奇数

## 分析

分别统计下每组中奇偶个数即可

## Code
```cpp
#include<bits/stdc++.h>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(i,x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
int n,m,ans,a1=0,a2=0,b1=0,b2=0;
int main(){
    n=read(),m=read();
    rep(i,1,n){
        int x=read();
        if(x%2==1)a1++;else a2++;
    }
    rep(i,1,m){
        int x=read();
        if(x%2==1)b1++;else b2++;
    }
    ans=min(a1,b2)+min(a2,b1);
    cout<<ans<<endl;
    return 0;
}
```
# B. Neko Performs Cat Furrier Transform

## 题意

将数字$x$通过一系列操作变成$2^k-1$

操作如下：

- 奇数次的操作$x \oplus 2^n-1$
- 偶数次的操作$x+1$

构造长度小于$40$的操作序列

## 分析

按照位数处理即可，每次二进制中相邻位发生$01$变化时要进行一次奇数操作

## Code
```cpp
#include<bits/stdc++.h>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(i,x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
int ans[46],n,x,pos,tot=0,A[46],num=0;
int main(){
    x=n=read();
    rep(i,1,30)if((1<<i)>n){pos=(1<<i)-1;break;}
    int p=0,cnt=0;
    while(x){
        p++;
        int opt=x%2;
        x>>=1;
        if(x%2!=opt)ans[++cnt]=p;
    }
    cnt--;
    int j=cnt;
    while(n<pos){
        n=n^((1<<ans[j])-1);A[++num]=ans[j];
        tot++;
        if(n<pos)tot++,n++;
        j--;
    }
    cout<<tot<<endl;
    rep(i,1,num)cout<<A[i]<<' ';cout<<endl;
}
```

# C. Neko does Maths

## 题意

给出数字$a,b$，请你最小化$LCM(a+k,b+k)$，求出$k$

## 分析

$LCM(a+k,b+k)=\frac {(a+k)\times (b+k)} {gcd(a+k,b+k)}$

即要最大化$gcd(a+k,b+k)=gcd(b-a,a+k)$

那么我们可以暴力枚举所有$1->(b-a)$的因数进行判断，求出最小值

时间复杂度$O(\sqrt {b-a})$

## Code
```cpp
#include<bits/stdc++.h>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(i,x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
ll a,b,k;
inline ll gcd(ll a,ll b){return !b?a:gcd(b,a%b);}
inline ll lcm(ll a,ll b){return 1ll*a*b/gcd(a,b);}
int main(){
    a=read(),b=read();
    if(a<b)swap(a,b);
    ll x=abs(a-b),ans=lcm(a,b),ansk=0,blo=trunc(sqrt(x))+1;
    if(a==b){puts("0");return 0;}
    rep(i,1,blo)
        if(x%i==0){
            k=i-a%i;
            if(lcm(a+k,b+k)<ans){
                ans=lcm(a+k,b+k);
                ansk=k;
            }
            k=x/i-a%(x/i);
            if(lcm(a+k,b+k)<ans){
                ans=lcm(a+k,b+k);
                ansk=k;
            }
            //cout<<i-a%i<<' '<<x/i-a%(x/i)<<endl;
        }
    cout<<ansk<<endl;
    return 0;
}
```

# D. Neko and Aki's Prank

## 题意

给出括号序列的长度$N$，将所有合法的括号序列放进Trie树，求该Tried树的最大匹配（一个点最多只能属于一条边）

## 分析

发现Trie树中存在不少重复的子树

可以直接进行DP

设计状态$f[i][j]$表示深度为$i$，括号序列左右平衡为$j$的计数值

可以将其转移到状态$f[i+1][j+1]$和$f[i+1][j-1]$]

时间复杂度$O(n^2)$

## Code

```cpp
#include<bits/stdc++.h>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(i,x) for(int i=last[x];i;i=e[i].next)
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr******
const int mod=1e9+7;
const int maxn=2e3+6;
int f[maxn][maxn],n,ans=0;
int main(){
    n=read();
    f[0][0]=1;
    rep(i,0,2*n-1)
        rep(j,0,2*n-1){
            f[i+1][j+1]=((f[i+1][j+1]+f[i][j])%mod+mod)%mod;
            if(j)f[i+1][j-1]=((f[i+1][j-1]+f[i][j])%mod+mod)%mod;
        }
    rep(i,1,2*n-1){
        if(i%2==0)continue;
        rep(j,0,2*n-1)if(i+j<=2*n)ans=(ans+f[i][j])%mod;
    }
    cout<<ans<<endl;
    return 0;
}
```


# E. Neko and Flashback

## 题意

给出序列$b'$和$c'$，分别含义如下

- $b_i=\min(a_i,a_{i+1})$
- $c_i=\max(a_i,a_{i+1})$
- $b'_i=b_{p_i}$
- $c'_i=c_{p_i}$

请你求出任意一个满足条件的原序列$a$

## 分析

实际上发现数列$p$并没有实际作用

从每一个$b_i$向$c_i$连边，那么问题转化为是否存在一条长度为$n$的欧拉路径，可以直接$dfs$处理

注意要判断许多$-1$的情况，细节较多

## Code
```cpp
#include<bits/stdc++.h>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(i,x) for(int i=last[x];i;i=e[i].next) 
using namespace std;
inline ll read(){
    ll f=1,x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
//******head by yjjr****** 
const int maxn=1e5+6;
int n,m,tot;
map<int,int> a;
multiset<int> to[maxn<<1];
int ans[maxn<<4],top,d[maxn<<1],cnt,p[maxn<<4],b[maxn],c[maxn],val[maxn<<1];
void dfs(int x){
    for(auto i=to[x].begin();i!=to[x].end();i=to[x].begin()) {
        auto v=*i;
        to[x].erase(i),to[v].erase(to[v].lower_bound(x));
        dfs(v);
    }
    ans[++top]=x;
}
int main(){
    n=read();
    rep(i,1,n-1){
        b[i]=read();
        if(!a.count(b[i]))a[b[i]]=++tot,val[tot]=b[i];
    }
    rep(i,1,n-1){
        c[i]=read();
        if(b[i]>c[i]){puts("-1");return 0;}
        if(!a.count(c[i]))a[c[i]]=++tot,val[tot]=c[i];
        to[a[c[i]]].insert(a[b[i]]),to[a[b[i]]].insert(a[c[i]]);
        d[a[c[i]]]++,d[a[b[i]]]++;
    }
    rep(i,1,tot)if(d[i]&1)p[++cnt]=i;
    if(cnt!=0&&cnt!=2){puts("-1");return 0;}
    else{
        if(cnt==0)dfs(1);else dfs(p[1]);
        if(top!=n){puts("-1");return 0;}
        else{
            while(top)printf("%d ",val[ans[top--]]);
            return 0;
        }
    }
}
```