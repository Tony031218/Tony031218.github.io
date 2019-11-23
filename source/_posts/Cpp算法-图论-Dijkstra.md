---
title: Cpp算法-图论-Dijkstra
toc: true
mathjax: true
date: 2019-01-10 12:57:53
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
#### 说明
`n, m`点数、边数<br/>
`G[][]`邻接矩阵存图<br/>
`dist[]`路径长度<br/>
`pre[]`路径<br/>
`make()`建图

#### 实现
```cpp
const int maxn = 10010;
int n, m, G[maxn][maxn], dist[maxn], pre[maxn], s;

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

void Dijkstra()
{
    int k, min;
    bool p[maxn];
    for (int i = 1; i <= n; ++i)
    {
        p[i] = false;
        if (i != s)
        {
            dist[i] = G[s][i];
            pre[i]  = s;
        }
    }
    dist[s] = 0; p[s] = true;
    for (int i = 1; i <= n - 1; ++i)
    {
        min = INT_MAX; k = 0;
        for (int j = 1; j <= n; ++j)
        {
            if (!p[j] && dist[j] < min)
            {
                min = dist[j];
                k = j;
            }
        }
        if (k == 0) return;
        p[k] = true;
        for (int j = 1; j <= n; ++j)
        {
            if (!p[j] && G[k][j] != INT_MAX && dist[j] > dist[k] + G[k][j])
            {
                dist[j] = dist[k] + G[k][j];
                pre[j] = k;
            }
        }
    }
    return;
}
```

#### 堆优化(链式前向星)
```cpp
struct Edge
{
    int to, nxt, t;
}edge[maxm << 1];
int head[maxn], cnt;
void add(int a, int b, int t)
{
    edge[++cnt].to  = b;
    edge[cnt].nxt = head[a];
    edge[cnt].t   = t;
    head[a]       = cnt;
}

struct heap
{
    int u, d;
    bool operator < (const heap& a) const
    {
        return d > a.d;
    }
};

void Dijkstra()
{
    priority_queue<heap> q;
    for (int i = 0; i <= n; ++i) dist[i] = INF;
    dist[1] = 0;
    q.push((heap){1, 0});
    while (!q.empty())
    {
        heap top = q.top();
        q.pop();
        int tx = top.u;
        int td = top.d;
        if (td != dist[tx]) continue;
        for (int i = head[tx]; i; i = edge[i].nxt)
        {
            int v = edge[i].to;
            if (dist[v] > dist[tx] + edge[i].t)
            {
                dist[v] = dist[tx] + edge[i].t;
                dy[v] = i; 
                dx[v] = tx;    //记录路径
                q.push((heap){v, dist[v]});
            }
        }
    }
}
```
##### 路径
```cpp
int q = n, p[maxm];
while (q != 1)
{
    p[++tot] = dy[q];
    q = dx[q];
}
```