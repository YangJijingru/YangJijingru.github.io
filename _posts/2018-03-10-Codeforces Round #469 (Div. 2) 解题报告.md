---
subtitle: "智商round"
tags: 
 - 解题报告
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P22.jpg"
preview-img: "/img/preview/P22.jpg"
---
# A. Left-handers, Right-handers and Ambidexters

题意：l个人会用左手打球，r个人会用右手打球，a个人两只手都能打球，需要组建一个球队，保证球队中一半人会用左手打球，另一半会右手打球，最大化球队总人数

模拟就行吧

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
int l,r,a,ans;
int main()
{
	l=read(),r=read(),a=read();
	ans=min(l,r);
	int tmp=max(l,r)-min(l,r);
	if(a<tmp)ans+=a;else ans=max(l,r),a=a-tmp,ans+=(a/2);
	cout<<ans*2<<endl;
	return 0;
}
```

# B. Intercepted Message

题意：给定两个序列a,b，每块的定义为ai+...+aj=bk+...+br，最大化块数

两个指针扫一遍就好了

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
const int maxn=1e6+6;
int ans,a[maxn],b[maxn],n,m;
int main()
{
	n=read(),m=read();
	rep(i,1,n)a[i]=read();
	rep(i,1,m)b[i]=read();
	int i=1,j=1;ll s1=a[i],s2=b[j];
	while(i<=n&&j<=m){
	    if(s1<s2){s1+=a[++i];continue;}
	    if(s1>s2){s2+=b[++j];continue;}
	    if(s1==s2){ans++;s1=a[++i],s2=b[++j];continue;}
	}
	cout<<ans<<endl;
	return 0;
}
```

# C. Zebras

题意：给你一个0,1组成的串，要求你按顺序任意组合成若干个串，要求每个串首尾都是0，且0和1不相邻

谜一般的题面和样例

常老师写挂题系列

如果当前是0，那么找一个末尾为1的加进去

如果当前是1，如果不存在结尾为0的串，那么无解，否则加入末尾为0的串

*注意保证每个串以0开头和结尾*

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
const int maxn=2e5+6;
int n,m,k,num;char s[maxn];
vector<int> a[maxn];
int main()
{
	scanf("%s",s+1);
	int len=strlen(s+1);
	rep(i,1,len){
		if(s[i]=='0')a[++num].pb(i);
		else{
			if(!num){puts("-1");return 0;}
			a[num--].pb(i);
		}
		k=max(k,num);
	}
	if(k!=num){puts("-1");return 0;}
	printf("%d\n",k);
	rep(i,1,k){
		printf("%d ",a[i].size());
		for(int j=0;j<a[i].size();j++)printf("%d ",a[i][j]);printf("\n");
	}
	return 0;
}
```

# D. A Leapfrog in the Array

题意：有n个数，数i在2*i-1的位置，每次操作将最后一个元素，放到与其最近的空位置，直到1-n位置被填满，有q个询问，每次询问x的最终位置

找规律

发现奇数位置为(x+1)/2

偶数如下图Y位置所示，粉色框内都已经填上了数字

那么Y一定会跳到X位置，设X位置之前有i/2个数字

那X后面就是N-i/2了

那Y就是从i+(N-i/2)=N+i/2位置跳来的

可以把偶数Y一直跳，直到其变为奇数

![这里写图片描述](http://img.blog.csdn.net/20180310130857199?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXdlcnR5MTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

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
ll n,q,x;
int main()
{
	n=read(),q=read();
	while(q--){
		x=read();
		while(!(x&1))x=(x>>1)+n;
		printf("%I64d\n",(x+1)>>1);
	}
	return 0;
}
```

# E. Data Center Maintenance

题意太烦，说不清了qwq

都怪我和班主任老余没认真学语文

对于c1,c2两个信息中心来说来说，如果t[c1]位于t[c2]前面一个时间，那么c1向c2建一条边，表示如果c1变化，那么c2随之变化，反之同理

拓扑图，对于每个强连通分量来说，答案一定是无出度中最小值

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
#define inf 0x3f3f3f3f
const int maxn=1e5+6;
struct edge{int to,next;}e[maxn<<2];
int last[maxn],n,top,cnt,m,h,tim,scc,t[maxn],dfn[maxn],low[maxn],que[maxn<<2];
int dg[maxn],belong[maxn],re=inf,pos;
bool inque[maxn];
vector<int>ans[maxn];
void insert(int u,int v){
	e[++cnt]=(edge){v,last[u]};last[u]=cnt;
}
void tarjan(int x){
	int now=0;
	dfn[x]=low[x]=++tim;
	que[++top]=x;inque[x]=1;
	reg(x)
		if(!dfn[e[i].to]){tarjan(e[i].to);low[x]=min(low[x],low[e[i].to]);}
		else if(inque[x])low[x]=min(low[x],dfn[e[i].to]);
	if(low[x]==dfn[x]){
		scc++;
		while(now!=x){
			now=que[top--];
			inque[now]=0;belong[now]=scc;ans[scc].pb(now);
		}
	}
}
		
int main()
{
	n=read(),m=read(),h=read();
	rep(i,1,n)t[i]=read();
	rep(i,1,m){
		int c1=read(),c2=read();
		if((t[c1]+1)%h==t[c2])insert(c1,c2);
		if((t[c2]+1)%h==t[c1])insert(c2,c1);
	}
	rep(i,1,n)
		if(!dfn[i])tarjan(i);
	rep(u,1,n)
		reg(u)
			if(belong[u]!=belong[e[i].to])dg[belong[u]]++;
	rep(i,1,scc)
		if(dg[i]==0&&ans[i].size()<re)re=ans[i].size(),pos=i;
	printf("%d\n",re);
	for(int i=0;i<ans[pos].size();i++)printf("%d ",ans[pos][i]);
	printf("\n");
	return 0;
}
```

# 总结

少有的下午场CF

原本打算认真地切题，结果10min秒了AB，之后又浑浑噩噩浪了

C题题意卡了半小时（感觉我每次打的实时CF场都是阅读理解大赛）

欠考虑了一种情况，之后把锅甩给了常老师，不幸gg

最后二十分钟推D题公式，mfs也来帮忙（划水）

挺刺激的吧，好怀念NOIP2017前整个机房打模拟赛的场面啊

------

AB写题这次准确度挺高的

Pascal->C++选手对STL使用还不是很熟练啊（话说熟练运用些常用的应该就行了吧）

D题应该分类讨论的，我一直往各种数列上去想，还是要多做做数学题啊



