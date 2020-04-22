---
title: Cpp算法-DFS
mathjax: true
toc: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: 9d047ada
date: 2019-01-10 12:54:06
---
## 说明
本文实现只是框架，应当灵活运用，dfs(...)函数返回值类型、参数列表根据情况灵活更改

## 框架
```cpp
void dfs(参数列表)
{
    if (到达目的地) 输出结果;
    else
    {
        for (int i = 0; i < 行动方法数; ++i)
        {
            if (下一步可行)
            {
                记录此步;
                dfs(改动后的参数列表);
                取消记录此步;
            }
        }
    }
    return;
}
```

## 例
[洛谷P1605](https://www.luogu.org/problemnew/show/P1605)
```cpp
#include<bits/stdc++.h>
using namespace std;

int n, m, t, sx, sy, fx, fy, ans;
int mg[6][6], now[6][6];
int go[4][2] = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};

void dfs(int x, int y)
{
    int x2, y2;
    if (x == fx && y == fy) ans++;
    else
    {
        for (int i = 0; i < 4; ++i)
        {
            x2 = x + go[i][0];
            y2 = y + go[i][1];
            if (x2 > 0 && x2 <= n && y2 > 0 && y2 <= m && mg[x2][y2] == 0 && now[x2][y2] == 0)
                
            {
                now[x2][y2] = 1;
                dfs(x2, y2);
                now[x2][y2] = 0;
            }
        }
    }
    return;
}

int main()
{
    memset(mg, 0, sizeof(mg));
    scanf("%d %d %d", &n, &m, &t);
    scanf("%d %d %d %d", &sx, &sy, &fx, &fy);
    for (int i = 1; i <= t; ++i)
    {
        int x, y;
        scanf("%d %d", &x, &y);
        mg[x][y] = 1;
    }
    now[sx][sy] = 1;
    ans = 0;
    dfs(sx, sy);
    printf("%d", ans);
    return 0;
}
```