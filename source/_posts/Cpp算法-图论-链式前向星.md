---
title: Cpp算法-图论-链式前向星
toc: true
mathjax: true
date: 2019-01-10 13:12:07
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
#### 说明
`cnt`记数<br/>
`head[]`记录边的头<br/>
`struct Edge{int, int, int}`边信息: 开始点、结束点、权值<br/>
`add_edge(int, int, int)`添加边

#### 实现
```cpp
int cnt, head[maxn];
struct Edge
{
	int next, to, val;
}edge[maxm];
void add_edge(int from, int to, int val)
{
	edge[++cnt].next = head[from];
	edge[cnt].to     = to;
	edge[cnt].val    = val;
	head[from]       = cnt;
}
```