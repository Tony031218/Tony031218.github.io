---
title: Cpp算法-数论-线性筛素数
toc: true
mathjax: true
date: 2019-01-10 13:07:43
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
#### 说明
`p[]` 最终结果

#### 实现
```cpp
bool vis[N];
int p[N], cnt;

void get_prime()
{
    for (int i = 2; i < N; ++i)
    {
        if (!vis[i])
            p[++cnt] = i;
        for (int j = 1; j <= cnt; ++j)
        {
            int v = i * p[j];
            if (v >= N) break;
            vis[v] = true;
            if (i % p[j] == 0) continue;
        }
    }
}
```