---
title: Cpp算法-图论-Prim
toc: true
mathjax: true
date: 2019-01-10 13:11:14
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
#### 说明
`n, m, _map[][]`点数、边数、邻接矩阵<br/>
`dist[]`树根到各点路径长<br/>
`pre[]`生成树路径 

#### 实现
```cpp
const int maxn = 101;
int n, m, dist[maxn], _map[maxn][maxn], pre[maxn];

void make()
{
	scanf("%d %d", &n, &m);
	for (int i = 1; i <= n; ++i)
		for (int j = 1; j <= n; ++j)
			_map[i][j] = INT_MAX;
	for (int i = 1; i <= n; ++i) _map[i][i] = 0;
	for (int i = 1; i <= m; ++i)
	{
		int from, to, w;
		scanf("%d %d %d", &from, &to, &w);
		_map[from][to] = w;
	}
	return;
}

void Prim()
{
	int i, j, k;
	int min;
	bool p[maxn];
	for (int i = 2; i <= n; ++i)
	{
		p[i] = false;
		dist[i] = _map[1][i];
		pre[i] = 1;
	}
	dist[1] = 0;
	p[1] = true;
	for (int i = 1; i <= n - 1; ++i)
	{
		min = INT_MAX;
		k = 0;
		for (int j = 1; j <= n; ++j)
		{
			if (!p[j] && dist[j] < min)
			{
				min = dist[j]
				k = j;
			}
		}
		if (k == 0) return;
		p[k] = true;
		for (int j = 1; j <= n; ++j)
		{
			if (!p[j] && _map[k][j] != INT_MAX && dist[j] > _map[k][j])
			{
				dist[j] = _map[k][j];
				pre[j] = k;
			}
		}
	}
	return;
}
```