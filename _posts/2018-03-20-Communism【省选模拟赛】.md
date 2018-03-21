---
subtitle: "用莫队和稀疏表乱搞"
tags: 
 - 数据结构-线段树
 - 特殊-扫描线
 - 数据结构-稀疏表
 - 数据结构-单调栈
 - 特殊-莫队
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P45.jpg"
preview-img: "/img/preview/P45.jpg"
---
标签：莫队，扫描线，线段树，单调栈

# 题目

![这里写图片描述](//img-blog.csdn.net/20180320214359821?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3F3ZXJ0eTExMjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

# 分析

官方题解：

显然，最大值一定出现在相邻的城市中

记$$d[i]=|a[i]-a[i+1]|$$

那么每次询问的答案为$$\sum_{l\leq i,j\leq r} max_{k=i}^{j-1} d_i$$

分别令next和pre为di前后第一个比di要大的位置

那么考虑每对城市产生的贡献，答案可以转化为$$\sum_{i=l}^{r-1} (min(r,next_i)-max(l-1,pre_i)-1)*d_i$$

然后将每个询问[l,r]放到二维平面上去看，可以转化为一个点

可以用离线+差分+扫描线+线段树解决了，处理矩形加和单点的询问

------

实际上可以用单调栈+倍增+莫队水过

对1e5数据范围的询问显然可以用莫队

瓶颈在于怎么求题面中描述的W数组

可以先用单调栈求出pre，next，然后计算出i到pre和next的距离，存储在s1,s2数组中

稀疏表存储区间内最大的di

查询的时候对于区间[l,r]，可以利用之前处理的倍增稀疏表O(1)查询[l,r]区间内贡献最大的点的位置，O(1)对答案进行计算

当然正解的复杂度为$$O(n log n)$$

莫队的时间复杂度为$$O(n \sqrt n)$$

# code
```
#include<bits/stdc++.h>
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
//**********head by yjjr**********
const int maxn=1e6+6;
struct node{int l,r,id;}q[maxn];
int n,Q,top,blo,l=1,r=0,lg[maxn],a[maxn],st[maxn],belong[maxn],f[maxn][26];
ll ans[maxn],re,s1[maxn],s2[maxn];
inline int maxa(int x,int y){return a[x]>=a[y]?x:y;}
inline bool cmpQ(node x,node y){return belong[x.l]==belong[y.l]?x.r<y.r:belong[x.l]<belong[y.l];}
inline int query(int x,int y){return maxa(f[x][lg[y-x+1]],f[y-(1<<lg[y-x+1])+1][lg[y-x+1]]);}
inline ll cal_l(int l,int r){int x=query(l,r);return s2[l]-s2[x]+(ll)(r-x+1)*a[x];}
inline ll cal_r(int l,int r){int x=query(l,r);return s1[r]-s1[x]+(ll)(x-l+1)*a[x];}
int main()
{
	n=read(),Q=read(),blo=sqrt(n-1);
	rep(i,1,n-1)belong[i]=(i-1)/blo+1;
	rep(i,2,n-1)lg[i]=lg[i>>1]+1;
	rep(i,1,n)a[i]=read();
	rep(i,1,n-1)a[i]=abs(a[i]-a[i+1]),f[i][0]=i;
	rep(i,1,n-1){
		while(top&&a[st[top]]<a[i])top--;
		s1[i]=s1[st[top]]+(ll)(i-st[top])*a[i];
		st[++top]=i;
	}
	top=0;st[0]=n;
	dep(i,n-1,1){
		while(top&&a[st[top]]<=a[i])top--;
		s2[i]=s2[st[top]]+(ll)(st[top]-i)*a[i];
		st[++top]=i;
	}
	for(int j=1;(1<<j)<n;j++)
		for(int i=1;i+(1<<j-1)<n;i++)	
			f[i][j]=maxa(f[i][j-1],f[i+(1<<j-1)][j-1]);
	rep(i,1,Q)q[i]=(node){read(),read()-1,i};
	sort(q+1,q+1+Q,cmpQ);
	rep(i,1,Q)
		if(q[i].l<=q[i].r){
			while(l>q[i].l)re+=cal_l(--l,r);
			while(r<q[i].r)re+=cal_r(l,++r);
			while(l<q[i].l)re-=cal_l(l++,r);
			while(r>q[i].r)re-=cal_r(l,r--);
			ans[q[i].id]=re;
		}
	rep(i,1,Q)printf("%lld\n",ans[i]);
	return 0;
}
```