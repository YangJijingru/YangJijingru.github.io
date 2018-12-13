---
subtitle: "线段相交问题"
tags: 
 - DP-杂题
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P59.jpg"
preview-img: "/img/preview/P59.jpg"
---
标签：线段求交，STL，二分

# 题目

[题目传送门](http://codeforces.com/contest/1061/problem/D）

# 题意

给出要看的$$n$$个电视节目，和每个节目开始结束时间分别为$$l_i$$和$$r_i$$，每次申请一个新的电视的花费为$$X+(r_i-l_i)\times Y$$，如果两个节目有交集，就必须用两个电视看，现在询问看完所有电视节目的最小花费

# 分析

先排序，后二分

二分判断是重新开一个电视划算，还是延续上一个划算

# code
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<set>
#include<map>
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
const int mod=1e9+7;
struct node{ll a,b;}c[maxn];
inline bool operator < (node p,node q){return p.a==q.a?p.b<q.b:p.a<q.a;}
set<ll>st;
map<ll,int>mp; 
int n;
ll X,Y,ans=0;
int main(){
    n=read(),X=read(),Y=read();
    rep(i,1,n)c[i]=(node){read(),read()};
    sort(c+1,c+1+n);
    st.insert(c[1].b);
    mp[c[1].b]++;
    ans=(ans+X+(c[1].b-c[1].a)*Y%mod)%mod;
    set<ll>::iterator it;
    rep(i,2,n){
        it=st.lower_bound(c[i].a);
        if(it==st.begin()){
            ans=(ans+X+(c[i].b-c[i].a)*Y%mod)%mod;
            mp[c[i].b]++;
        }else{
            it--;
            ll value=*it;
            if(c[i].a>value&&(c[i].a-value)*Y<=X){
                mp[value]--;
                if(mp[value]==0)st.erase(value);
                ans=(ans+(c[i].b-value)*Y%mod)%mod;
                mp[c[i].b]++;
            }else{
                ans=(ans+X+(c[i].b-c[i].a)*Y%mod)%mod;
                mp[c[i].b]++;
            }
        }
        st.insert(c[i].b);
    }
    cout<<ans<<endl;
    return 0;
}
```
