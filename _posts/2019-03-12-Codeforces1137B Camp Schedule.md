---
subtitle: "利用KMP的Next数组贪心"
tags: 
 - 字符串-KMP
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P35.jpg"
preview-img: "/img/preview/P35.jpg"
---

标签：KMP
# 题目

[题目传送门](http://codeforces.com/contest/1137/problem/B)

# 题意

给出**01**字符串$s$和$t$，可以将字符串$s$内字符位置改变（无限制），最大化$t$在新的$s$中出现次数

# 分析

先用KMP预处理求出Next数组

按照$t$的结构组合，每次到$t$的末尾时，贪心的思想跳到$next[t]$的位置（保证最大匹配）

直到$0$或者$1$使用完毕

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
const int maxn=5e5+6;
int nx[maxn],len1,len2,o=0,z=0;
char s[maxn],t[maxn];
void GetNext(char st[]){
    int i=0,j=-1;nx[0]=-1;
    while(i<len1){
        if(j==-1||st[i]==st[j])nx[++i]=++j;else j=nx[j];
    }
}
int main(){
    cin>>s>>t;
    len1=strlen(s),len2=strlen(t);GetNext(t);
    rep(i,0,len1-1)if(s[i]=='0')z++;else o++;
    string ans="";
    int tot=0;
    while(z&&o){
        if(t[tot]=='0'){
            if(z)z--,ans+='0';else break;
        }else{
            if(o)o--,ans+='1';else break;
        }
        if((++tot)==len2)tot=nx[tot];
    }
    while(z--)ans+='0';
    while(o--)ans+='1';
    cout<<ans<<endl;
}
```