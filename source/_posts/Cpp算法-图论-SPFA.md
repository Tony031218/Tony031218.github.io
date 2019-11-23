---
title: Cpp算法-图论-SPFA
toc: true
mathjax: true
date: 2019-01-10 13:12:40
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
#### 说明
`n, m, s`点数、边数、源点<br/>
`cnt, head[], edge[], add(int, int, int)`链式前向星<br/>
`dist[]`各点到源点路径长<br/>
`vis[]`记录

#### 实现
```cpp
const int maxn = 10010;
const int maxm = 500010;

int n, m, s, dist[maxn], vis[maxn];

int cnt, head[maxn];
struct Edge
{
    int next, to, dis;
}edge[maxm];
void add(int from, int to, int dis)
{
    edge[++cnt].next = head[from];
    edge[cnt].to     = to;
    edge[cnt].dis    = dis;
    head[from]       = cnt;
}

void SPFA()
{
    queue<int> q;
    for (int i = 1; i <= n; ++i)
    {
        dist[i] = INT_MAX;
    }
    q.push(s);
    dist[s] = 0; vis[s] = true;
    while (!q.empty())
    {
        int u = q.front();
        q.pop(); vis[u] = 0;
        for (int i = head[u]; i; i = edge[i].next)
        {
            int v = edge[i].to;
            if (dist[v] > dist[u] + edge[i].dis)
            {
                dist[v] = dist[u] + edge[i].dis;
                if (!vis[v])
                {
                    vis[v] = true;
                    q.push(v);
                }
            }
        }
    }
    return;
}
```