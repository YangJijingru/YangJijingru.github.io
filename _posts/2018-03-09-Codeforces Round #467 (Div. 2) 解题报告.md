---
subtitle: "水题round"
tags: 
 - 解题报告
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P21.jpg"
preview-img: "/img/preview/P21.jpg"
---
话说好久没打CF div 2了

自从上次lxl毒瘤round惨掉rating之后，三个月没打CF

这场质量一般吧，水题偏多

------

# A. Olympiad

题意：找出除了0有多少个不同的数？（应该是这样的题面吧，雾）

```
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
inline ll read()
{
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
int n,a[106],ans=0;
int main()
{
	n=read();
	rep(i,1,n)a[i]=read();
	sort(a+1,a+1+n);
	rep(i,1,n)if(a[i]!=a[i-1])ans++;
	cout<<ans<<endl;
	return 0;
}
```

# B. Vile Grasshoppers

题意：给出两个数x,y，找出y范围内最大的一个数满足不能被2->x整除

话说我莫名其妙理解错题意了，一直找最小，不停WA4 ＴＡＴ

```
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
#define lson (x<<1)
#define rson (x<<1|1)
using namespace std;
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
int x,y;
int main()
{
	x=read(),y=read();
	dep(i,y,x+1){
		bool flag=1;
		for(int j=2;j<=min((int)trunc(sqrt(i))+1,x);j++)
			if(i%j==0){flag=0;break;}
		if(flag){cout<<i;return 0;}
	}
	printf("-1");
	return 0;
}
```

# C. Save Energy!

好励志的题目名字？雾

公式题，不过多解释，反正题目说的神烦，什么开关啊鬼东西

```
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
#define lson (x<<1)
#define rson (x<<1|1)
using namespace std;
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

ll t,k,d,ans;  
int main()  
{  
    cin>>k>>d>>t;
    ll x=k/d*d+(k%d!=0)*d;t*=2ll;
    ll y=t/(2ll*k+(x-k));ans+=y*x;t-=y*(2ll*k+(x-k));
    if(t>=2ll*k){ans+=k+(t-2ll*k);printf("%lld.0\n",ans);}  
    else{  
        ans+=t/2;
    	if(t%2)printf("%lld.5\n",ans);else printf("%lld.0\n",ans);  
    }  
    return 0;  
}  
```

# D. Sleepy Game

题目标题更加有趣（逃

题意：给定一张N个点M条边的有向图和源点S，从S开始是否存在一条长度为奇数到达没有出边的点的路径

直接dfs判断就好了啊

vis[i][0/1]表示到第i个点经过边数为奇/偶数

然后要特判存在环的情况

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
#include<vector>
#include<set>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
#define lson (x<<1)
#define rson (x<<1|1)
#define mid ((l+r)>>1)
#define pi acos(-1)
#define mp make_pair
#define pb push_back
#define pa pair<int,int>
#define fi first
#define se second
#define lowbit(x) ((x)&(-x))
#define mod (ll)1e9+7
using namespace std;
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
inline ll getgcd(ll x,ll y){return !x?y:getgcd(y%x,x);}
inline ll getlcm(ll x,ll y){return x/getgcd(x,y)*y;}
inline ll qpow(int x,int y){
	ll re=1;
	while(y){
	   	if(y&1)re=(re*x)%mod;
		y>>=1,x=(x*x)%mod;
	}
	return re;
}
//**********head by yjjr**********
const int maxn=1e5+6;
vector<int>a[maxn],b;
int vis[maxn][2],pre[maxn][2],vs[maxn],n,m,s;
void dfs(int fr,int x,int d){
	if(vis[x][d]==1)return;
	vis[x][d]=1;pre[x][d]=fr;
	for(int i=0;i<a[x].size();i++)dfs(x,a[x][i],d^1);
}
inline bool loop(int x){
	vs[x]=1;
	bool flag=0;
	for(int i=0;i<a[x].size();i++){
		if(vs[a[x][i]]==1)flag=1;else if(vs[a[x][i]]==0)flag=loop(a[x][i]);
		if(flag)return 1;
	}
	vs[x]=2;
	return 0;
}
int main()
{
	n=read(),m=read();
	rep(i,1,n){
		int num=read(),x;
		rep(j,1,num)a[i].pb(x=read());
	}
	s=read();
	dfs(0,s,0);
	rep(i,1,n)
		if(vis[i][1]&&a[i].size()==0){
			int tmp=i,t=1;
			b.pb(tmp);
			while(pre[tmp][t]!=0){
				b.pb(pre[tmp][t]);
				tmp=pre[tmp][t];
				t^=1;
			}
			puts("Win");
			dep(j,b.size()-1,0)printf("%d ",b[j]);
			return 0;
		}
	if(loop(s))puts("Draw");else puts("Lose");
	return 0;
}
```

# E. Lock Puzzle

题意：给定字符串S和目标串T，每次变换过程为取S后缀进行翻转，之后将翻转后的放到原串前面，问操作的过程

构造题

假设字符串S为a--b--，a为T的后缀，ba也为T的后缀，设b的下标为x

那么先将n翻转，变为--b--a(reverse)

然后将x-1翻转，变为a--b--

最后将1翻转，变为ba

所以一定可以在3*n次操作内解决

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
#include<vector>
#include<set>
#define rep(i,a,b) for(int i=a;i<=b;i++)
#define dep(i,a,b) for(int i=a;i>=b;i--)
#define ll long long
#define mem(x,num) memset(x,num,sizeof x)
#define reg(x) for(int i=last[x];i;i=e[i].next)
#define lson (x<<1)
#define rson (x<<1|1)
#define mid ((l+r)>>1)
#define pi acos(-1)
#define mp make_pair
#define pb push_back
#define pa pair<int,int>
#define fi first
#define se second
#define lowbit(x) ((x)&(-x))
using namespace std;
inline ll read(){
	ll f=1,x=0;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
inline ll getgcd(ll x,ll y){return !x?y:getgcd(y%x,x);}
inline ll getlcm(ll x,ll y){return x/getgcd(x,y)*y;}
inline ll qpow(int x,int y,int p){
	ll re=1;
	while(y){
	   	if(y&1)re=(re*x)%p;
		y>>=1,x=(x*x)%p;
	}
	return re;
}
//**********head by yjjr**********
const int maxn=2e3+6;
char s[maxn],t[maxn];int n;
vector<int>a;
void shift(int x){
	if(!x)return;
	reverse(s,s+n);
	reverse(s+x,s+n);
	a.pb(x);
}
int main()
{
	n=read();scanf("%s%s",s,t);
	rep(i,0,n-1){
		int j=i;
		while(j<n&&s[j]!=t[n-i-1])j++;
		if(j==n){puts("-1");return 0;}
		shift(n);shift(j);shift(1);
	}
	printf("%d\n",a.size());
	for(int i=0;i<a.size();i++)printf("%d ",a[i]);
	return 0;
}
```
# 总结

水题round

但是公式题一定要注意式子正确性，而且注意实数的类型转化

仔细考虑英文的题意，注意每一个细节