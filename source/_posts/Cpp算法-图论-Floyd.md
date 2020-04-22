---
title: Cpp算法-图论-Floyd
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: ba77762f
date: 2019-01-10 13:07:02
---
#### 说明
`n, m, G[][]`点数、边数、邻接矩阵<br/>
`dist[][]`每对顶点间路径长度<br/>
`pre[][]`每对顶点之间路径<br/>
`make()`建图

#### 实现
```cpp
const int maxn = 110;
int n, m, G[maxn][maxn], dist[maxn][maxn], pre[maxn][maxn];

void make()
{
    scanf("%d %d", &n, &m);
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= n; ++j)
            G[i][j] = INT_MAX;
    for (int i = 1; i <= n; ++i) G[i][i] = 0;
    for (int i = 1; i <= m; ++i)
    {
        int from, to, w;
        scanf("%d %d %d", &from, &to, &w);
        G[from][to] = w;
    }
    return;
}

void Floyd()
{
    for (int i = 1; i <= n; ++i)
    {
        for (int j = 1; j <= n; ++j)
        {
            dist[i][j] = G[i][j];
            pre[i][j]  = i;
        }
    }
    for (int k = 1; k <= n; ++k)
    {
        for (int i = 1; i <= n; ++i)
        {
            for (int j = 1; j <= n; ++j)
            {
                if (dist[i][k] != INT_MAX && dist[k][j] != INT_MAX && dist[i][k] + dist[k][j] < dist[i][j])
                {
                    dist[i][j] = dist[i][k] + dist[k][j];
                    pre[i][j]  = pre[k][j];
                }
            }
        }
    }
    return;
}
```