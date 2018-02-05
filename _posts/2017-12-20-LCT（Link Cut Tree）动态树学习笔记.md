---
tags: 
 - 知识学习
 - 数据结构-LCT
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P17.jpg"
preview-img: "/img/preview/P37.jpg"
---
动态树LCT就是支持Link，Cut操作的树形数据结构

整体思想和树链剖分有些类似，每个点有一条实边（重边）与其儿子相连，剩下的都是虚边（轻边），然后用许多个splay来维护每条重链，记录splay中每个点的父亲节点，那么这个splay的根节点指向的是另一条链（对应的是虚边）

下面是LCT的几种基本操作

## isroot

判断这个节点是不是所处的splay的根，代码很显然

//c[x][0]表示x节点的左儿子，c[x][1]表示x节点的右儿子

```
bool isroot(int x){return c[fa[x]][0]!=x&&c[fa[x]][1]!=x;}
```

## access

重点来了，LCT的关键操作

取出当前根节点到x节点的一段路径然后放入splay中

过程实现起来很简单

```
void access(int x){
    for(int now=0;x;now=x,x=fa[x])splay(x),c[x][1]=now,update(x);
}
```

## makeroot

换根操作，将x旋转到它所在的树根，每次换根的时候会将它到根路径上的点关系翻转，对其余点都无影响

所以只需要将其路径上的点access，然后放入splay中，打上翻转rev标记即可

```
void makeroot(int x){access(x);splay(x);rev[x]^=1;}
```

## link

LCT基础操作

将x,y两点间新建一条边，同样代码很显然

将x换到所在树的根位置上，之后将这一整棵树连向y

```
void link(int x,int y){makeroot(x);fa[x]=y;}
```
## cut

LCT基础操作

link对应的就是cut操作

先将x换到根的位置，然后取出x到y的路径，放入splay中

把splay(y)旋转，那么此时y的左儿子必定是x，直接断开就可以了

```
void cut(int x,int y){makeroot(x);access(y);splay(y);c[y][0]=fa[x]=0;update(y);}
```

## query

询问操作查询x到y路径上的信息

和cut操作类似，先将x换到根的位置，然后access取x->y路径上的点用splay维护即可

```
void query(int x,int y){makeroot(x);access(y);splay(y);}
```

