---
title: Cpp算法-图论-欧拉回路
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: '57662471'
date: 2019-01-10 13:06:14
---
## 邻接矩阵

#### 说明
`G[][]`邻接矩阵<br/>
`deg[]`度<br/>
`ans[]`欧拉回路<br/>
`n, e`点数、边数

#### 实现
```cpp
int G[maxn][maxn], deg[maxn], ans[maxn];
int n, e, x, y, ansi, s;

void Euler(int i)
{
	for (int j = 1; j <= n; ++j)
	{
		if (G[i][j])
		{
			G[i][j] = G[j][i] = 0;
			Euler(j);
		}
	}
	ans[++ansi] = i;
}

int main()
{
	scanf("%d %d", &n, &e);
	for (int i = 1; i <= e; ++i)
	{
		scanf("%d %d", &x, &y);
		G[x][y] = G[y][x] = 1;
		deg[x]++;
		deg[y]++;
	}
	s = 1;
	for (int i = 1; i <= n; ++i)
		if (deg[i] % 2 == 1)
			s = i;
	Euler(s);
	for (int i = 1; i <= ansi; ++i)
		printf("%d ", ans[i]);
	printf("\n");
	return 0;
}
```

## 链式前向星

#### 说明
`n, m`点数、边数<br/>
`head, edge[]`链式前向星<br/>
`ans[], ansi`路径、数组大小<br/>
`vis[]`记录<br/>
`make()`建图

#### 实现
```cpp
int head[maxn];
struct Node
{
	int to, next;
}edge[maxm];

void make()
{
	scanf("%d %d", &n, &m);
	for (int k = 1; k <= m; ++k)
	{
		int i, j;
		scanf("%d %d", &i, &j);
		edge[k].to = i;
		edge[k].next = head[i];
		head[i] = k;
	}
	return;
}

int ans[maxm];
int ansi = 0;
bool vis[2 * maxm];

void dfs(int now)
{
    for (int k = head[now]; k != 0; k = edge[k].next)
	{
        if (!vis[k])
        {
            vis[k] = true;
            vis[k ^ 1] = true;
            dfs(edge[k].to);
            ans[ansi++] = k;
        }
	}
}
```