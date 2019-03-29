---
subtitle: "以梦为马，不负韶华"
tags: 
 - dairy
 - 知识学习
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P73.jpg"
preview-img: "/img/preview/P49.jpg"
---

**本文长文/高能预警**

由于博主过于懒惰和菜鸡，打算把未来50天内的大多数训练题目全部放在此博文中，将会不定时更新

所有代码全部位于[Onedrive共享文件夹](https://1drv.ms/f/s!AsHiSMC_ys6wgdFoKVfBinFOz1Vb4w)内

# DP

### CF802K 树形DP

无意中做到了瑞士HC2 EPFL的题目，果然很SOI，很侧重思维性

------

题意：给定一棵树，可以从一个点开始在树上行走，每条边上有一个宝物价值为$w$，每个节点最多访问$k$次，求能够得到的最大价值

$f[x][1]$表示访问完以$x$节点为根的子树后**返回**到$x$的父节点所能得到的最大价值

$f[x][0]$表示访问玩以$x$节点为根的子树后**不返回**$x$的父节点所能得到的最大价值

### CF261D 结论+暴力

题意：给出数列$B$，请假定一个数列$A$，使得其长度为$n\times t$，使得$A$序列为若干个$B$组成，求出序列$A$的最长上升子序列数

表面上看十分不可做，无法把$A$序列求出来并计算LCS

但显然当$t\geq min(n,maxb)$时，答案是$a$中不同的个数

剩下的可以直接暴力

因为LCS的总增量一定小于等于$maxnb\times n$
