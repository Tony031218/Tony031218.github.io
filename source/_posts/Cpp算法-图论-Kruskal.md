---
title: Cpp算法-图论-Kruskal
toc: true
mathjax: true
date: 2019-01-10 13:10:40
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
#### 说明
`ufs[], find(int), unionn(int, int)`并查集结构<br/>
`edge[]`链式前向星<br/>
`cmp(Edge, Edge)`边排序方案

#### 实现
```cpp
const int maxn = 1010;
int ufs[maxn];

int find(int x)
{
    if (ufs[x] != x) return ufs[x] = find(ufs[x]);
    return ufs[x] = x;
}

void unionn(int x, int y)
{
    int a = find(x);
    int b = find(y);
    if (a != b)
    {
        ufs[a] = b;
    }
}

const int maxm = 100010;

struct Edge
{
    int a, b, w;
    bool select;
}edge[maxm];

bool cmp(Edge a, Edge b)
{
    if (a.w != b.w) return a.w < b.w;
    if (a.a != b.a) return a.a < b.a;
    return a.b < b.b; 
}

void kruskal()
{
    for (int i = 1; i <= n; ++i)
    {
        ufs[i] = i;
    }
    int k = 0, x, y;
    sort(edge + 1, edge + 1 + m, cmp);
    for (int i = 1; i <= m; ++i)
    {
        if (k == n - 1) break;
        x = find(edge[i].a);
        y = find(edge[i].b);
        if (x != y)
        {
            unionn(x, y);
            k++;
            edge[i].select = true;
        } 
    }
}
```