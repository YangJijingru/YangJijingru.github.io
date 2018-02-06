---
tags: 
 - 解题报告
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P38.jpg"
preview-img: "/img/preview/P78.jpg"
---
# A

[题面传送门](http://codeforces.com/contest/897/problem/A)

## 题意

给定长度为N的字符串，进行m次操作，每次将$l->r$范围内为$C_1$的字符改成$C_2$，输出操作完的字符串

## 数据范围

$1\le n,m\le 100$

## 分析

注意使用string，模拟水题吧

## code

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
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

int n,m,l,r;
char c1,c2;
string s;
int main()
{
	cin>>n>>m;
	cin>>s;
	rep(i,1,m){
		cin>>l>>r>>c1>>c2;
		rep(j,l,r)
		    if(s[j-1]==c1)s[j-1]=c2;
	}
	cout<<s;
	return 0;
}
```

# B

[题面传送门](http://codeforces.com/contest/897/problem/B)

## 题意

规定一种zcy数的定义为：回文数且位数为偶数。询问前k小的zcy数的和并取模。

## 数据范围

$1 ≤ k ≤ 10^5, 1 ≤ p ≤ 10^9$

## 分析

显然枚举前k小个数，然后翻转求回文并求和取模就可以了，比如这样：

12->1221

329->329923

## code

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
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
int num[100];
ll k,p,ans;
int main()
{
	k=read(),p=read();
	rep(i,1,k){
		ll t=i,s=0;
		int cnt=0;
		mem(num,0);
		while(t){
			num[++cnt]=t%10;
			t/=10;
		}
		dep(j,cnt,1)s=(s+num[j])*10;
		rep(j,1,cnt)s=(s+num[j])*10;
		s/=10;
		ans=(ans+s)%p;
		//cout<<s<<' '<<cnt<<endl;
	}
	cout<<ans<<endl;
	return 0;
}
```

# C

[题面传送门](http://codeforces.com/contest/897/problem/C)

## 题意

给出了字符串$f_0$，递推关系式为$f(n) = str1 + f(n - 1) + str2 + f(n - 1) + str3$

Q次询问给出n,k，n表示f的下标，k表示 $f(n)$ 中第k个字符

## 数据范围

$0 ≤ n ≤ 10^5, 1 ≤ k ≤ 10^{18}$

## 分析

见到的最恶心题目，差评++

dfs深搜，注意细节啊，容易写挂题

## code

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
using namespace std;
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const int maxn=100006;
string s0="What are you doing at the end of the world? Are you busy? Will you save us?",
s1="What are you doing while sending \"",s2="\"? Are you busy? Will you send \"",s3="\"?";
ll q,n,l0,l1,l2,l3=2;
ll k,f[maxn];

char dfs(ll n,ll k)
{
	if(!n)return k<=l0?s0[k-1]:'.';
	if(k<=l1)return s1[k-1];
	k-=l1;
	if(k<=f[n-1]||!f[n-1])return dfs(n-1,k);
	k-=f[n-1];
	if(k<=l2)return s2[k-1];
	k-=l2;
	if(k<=f[n-1]||!f[n-1])return dfs(n-1,k);
	k-=f[n-1];
	return k<=l3?s3[k-1]:'.';
}

inline int slen(string s)
{
	for(int i=0;;i++)
	if(s[i]=='\0')return i;
}
int main()
{
	q=read();
	l0=s0.length();l1=s1.length();l2=s2.length();
	f[0]=l0;
	rep(i,1,maxn)
	{
		f[i]=2*f[i-1]+l1+l2+l3;
		if(f[i]>1e18)break;
	}
	while(q--)
	{
		n=read(),k=read();
		putchar(dfs(n,k));
	}
}
```



# 总结

第一次在cf上碰到毒瘤题，做完AB两题就没什么事情干了

C题debug不出来（难受的一匹，好恶心），D题交互题很好（du）啊，可是我不会啊QwQ

E题让我知道了什么叫做OI的暴力，成七大爷毒瘤啊，珂学家毒瘤啊

还需要多见不同种类的题目