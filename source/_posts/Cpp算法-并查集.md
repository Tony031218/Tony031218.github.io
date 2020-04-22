---
title: Cpp算法-并查集
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: 9603fc0b
date: 2019-01-10 13:16:01
---

#### 说明
`n, m, q`点数、边数、问题数
`x, y`需要合并的两个数
`ufs[]`并查集
`find(int)`查找并查集中一个数的祖先
`unionn(int, int)`合并两个数所在集合

#### 实现
```cpp
const int maxn = 10010;
int ufs[maxn];
int n, m, x, y, q;

int find(int x)
{
    if (ufs[x] != x) return ufs[x] = find(ufs[x]);
    return ufs[x] = x;
}

void unionn(int x, int y)
{
    int fx = find(x);
    int fy = find(y);
    if (fx != fy)
    {
        ufs[fx] = fy;
    }
}

int main()
{
    scanf("%d %d", &n, &m);
    for (int i = 1; i <= n; ++i) ufs[i] = i;
    for (int i = 1; i <= m; ++i)
    {
        scanf("%d %d", &x, &y);
        unionn(x, y);
    }
    scanf("%d", &q);
    for (int i = 1; i <= q; ++i)
    {
        scanf("%d %d", &x, &y);
        if (find(x) == find(y)) printf("YES\n");
        else printf("NO\n");
    }
    return 0;
}
```