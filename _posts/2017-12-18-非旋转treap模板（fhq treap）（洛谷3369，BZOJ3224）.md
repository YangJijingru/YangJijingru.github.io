---
tags: 
 - 知识学习
 - 数据结构-treap
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P7.jpg"
preview-img: "/img/preview/P47.jpg"
---
# 题目

[题目传送门](http://www.lydsy.com/JudgeOnline/problem.php?id=3224)

Description

您需要写一种数据结构（可参考题目标题），来维护一些数，其中需要提供以下操作：
1. 插入x数
2. 删除x数(若有多个相同的数，因只删除一个)
3. 查询x数的排名(若有多个相同的数，因输出最小的排名)
4. 查询排名为x的数
5. 求x的前驱(前驱定义为小于x，且最大的数)
6. 求x的后继(后继定义为大于x，且最小的数)
Input

第一行为n，表示操作的个数,下面n行每行有两个数opt和x，opt表示操作的序号(1<=opt<=6)
Output

对于操作3,4,5,6每行输出一个数，表示对应答案
Sample Input
10

1 106465

4 1

1 317721

1 460929

1 644985

1 84185

1 89851

6 81968

1 492737

5 493598

Sample Output
106465

84185

492737

HINT

1.n的数据范围：n<=100000

2.每个数的数据范围：[-2e9,2e9]

# code

```
#include<iostream>
#include<cstdlib>
#include<cstdio>
#include<cmath>
#include<cstring>
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
const int maxn=1e5+6;
struct tree{int l,r,size,w,f;}t[maxn<<1];
int n,len=0,root=0;
inline int getrank(){return (rand()<<16)+rand();}
void change(int x){
	if(x==0)return;
	t[x].size=t[t[x].l].size+t[t[x].r].size+1;
}
int merge(int x,int y){
	if(x==0||y==0)return x+y;
	if(t[x].f>t[y].f){
		t[x].r=merge(t[x].r,y);change(x);return x;}
	else{t[y].l=merge(x,t[y].l);change(y);return y;}
}
void splite(int x,int val,int &new1,int &new2){
	if(x==0){new1=0;new2=0;return;}
	if(t[x].w<val){
		new1=x;splite(t[x].r,val,t[x].r,new2);
	}else{new2=x;splite(t[x].l,val,new1,t[x].l);}
	change(x);
}
int insert(int now,int want){
	if(now==0)return want;
	if(t[now].f>t[want].f){
		if(t[now].w<t[want].w)t[now].r=insert(t[now].r,want);else t[now].l=insert(t[now].l,want);
		change(now);return now;
	}
	splite(now,t[want].w,t[want].l,t[want].r);change(want);return want;
}

int find(int now,int want){
	if(t[t[now].l].size==want-1)return t[now].w;
	if(t[t[now].l].size>=want)return find(t[now].l,want);
	return find(t[now].r,want-t[t[now].l].size-1);
}
int findleft(int now){while(t[now].l)now=t[now].l;return t[now].w;}
int findright(int now){while(t[now].r)now=t[now].r;return t[now].w;}

int main()
{
	srand(1125);
	n=read();
	mem(t,0);
	rep(i,1,n){
		int opt=read();
		if (opt==1){int x=read();t[++len].f=getrank();t[len].w=x;t[len].size=1;root=insert(root,len);}
		else if(opt==2){int x=read(),new1,new2,new3;splite(root,x,new1,new2);splite(new2,x+1,new2,new3);root=merge(merge(new1,t[new2].l),merge(t[new2].r,new3));}
		else if(opt==3){int x=read(),new1,new2;splite(root,x,new1,new2);printf("%d\n",t[new1].size+1);root=merge(new1,new2);}
		else if(opt==4){int x=read();printf("%d\n",find(root,x));}
		else if(opt==5){int x=read(),new1,new2;splite(root,x,new1,new2);printf("%d\n",findright(new1));root=merge(new1,new2);}
		else{int x=read(),new1,new2;splite(root,x+1,new1,new2);printf("%d\n",findleft(new2));root=merge(new1,new2);}
	}
	return 0;
}
```