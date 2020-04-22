---
title: Cpp算法-背包问题
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: 990bbe9a
date: 2019-01-10 13:13:52
---
## 01背包问题

有 $n$ 件物品，和一容积为 $V$ 的背包，第 $i$ 件物品的体积为 $w_i$ ，价值为 $c_i$ 。将第几件物品装入，使体积不超过总体积，且价值和最大，求最大价值。

由题意易知状态转移方程： $F_{i,j} = max(F_{i-1,j}\ , F_{i-1,j-w_i} + c_i)$

$F_{i, j}$ 为前 $i$ 件物品放入容量为 $V$ 的背包中最大价值<br/>
时间复杂度 $O(n\times V)$ ，空间复杂度 $O(n\times V)$
#### 框架
注意倒序，保证`f[n][V]`为结果
```cpp
for (int i = 1; i <= n; ++i)
{
    for (int j = V; j >= w[i]; --j)
    {
        f[i][j] = max(f[i - 1][j], f[i - 1][j - w[i]] + c[i]);
    }
}
printf("%d", f[n][V])
```

#### 空间复杂度优化
降至一维数组<br/>
时间复杂度 $O(n\times V)$ ，空间复杂度 $O(V)$
```cpp
for (int i = 1; i <= n; ++i)
{
    for (int j = V; j >= w[i]; --j)
    {
        f[j] = max(f[j], f[j - w[i]] + c[i]);
    }
}
printf("%d", f[V]);
```

## 完全背包问题

有 $n$ 种物品（每种 __无限件__ ），和一容积为 $V$ 的背包，第 $i$ 种物品的体积为 $w_i$ ，价值为 $c_i$ 。将第几种物品取任意件装入，使体积不超过总体积，且价值和最大，求最大价值。

将[01背包](../pack#01背包问题)第二个循环改为正序即可<br/>
状态转移方程：$F_j = max(F_j\ , F_{j-w_i}+c_i)$
#### 框架
```cpp
for (int i = 1; i <= n; ++i)
{
    for (int j = w[i]; j <= V; ++j)
    {
        f[j] = max(f[j], f[j - w[i]] + c[i]);
    }
}
printf("%d", f[V]);
```

## 多重背包问题
有 $N$ 种物品，和一容积为 $V$ 的背包，第 $i$ 种物品有 $n_i$ 件，体积为 $w_i$ ，价值为 $c_i$ 。将第几件物品装入，使体积不超过总体积，且价值和最大，求最大价值。

### 解法 $I.$ 化为完全背包
状态转移方程：$F_{i,v} = max(F_{i-1,v-k\times w_i} + k\times c_i | 0\leqslant k\leqslant n_i)$<br/>
时间复杂度：$O(V\times \sum{n_i})$
#### 框架
```cpp
for (int i = 1; i <= N; ++i)
{
    for (int j = V; j >= 0; --j)
    {
        for (int k = 0; k <= n[i]; ++k)
        {
            f[i][j] = max(f[i - 1][j], [i - 1][j - k * w[i]] + k * c[i])
        }
    }
}
printf("%d", f[N][V]);
```

### 解法 $II.$ 化为01背包
把 $n_i$ 件一种物品化为单独的 $n_i$ 件物品即可<br/>
时间复杂度：$O(V\times \sum{n_i})$<br/>
框架略

### 解法 $III.$ 二进制优化

$$
n_i\to 1+2+4+\dots +2^{k-1}+\dots +(n_i-2^k+1)
$$
$$
\sum{n_i}\to \sum{\log_2{n_i}}
$$
时间复杂度：$O(V\times \sum{\log_2{n_i}})$
#### 框架
```cpp
for (int i = 1; i <= n; ++i)
{
    int w, c, n, t = 1;
    scanf("%d %d %d", &w, &c, &n);
    while(n >= t)
    {
        v[++N] = x * t;
        w[N]   = y * t;
        n -= t;
        t *= 2;
    }
    v[++N] = x * n;
    w[N]   = y * n;
}
for (int i = 1; i <= N; ++i)
    for (int j = V; j >= v[i]; --j)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
printf("%d", f[V]);
```

## 混合三种背包问题

有 $N$ 种物品，和一容积为 $V$ 的背包，第 $i$ 种物品有 $n_i$ 件或无穷件，体积为 $w_i$ ，价值为 $c_i$ 。将第几件物品装入，使体积不超过总体积，且价值和最大，求最大价值。

#### 伪框架
```cpp
for (int i = 1; i <= N; ++i)
{
    if (第i件是有穷件)
    {
        for (int j = V; j >= 0; --j)
            f[j] = max(f[j], f[j - w[i]] + c[i]);
    }
    else //有无穷件
    {
        for (int j = 0; j <= V; ++j)
            f[j] = max(f[j], f[j - w[i]] + c[i]);
    }
}
```

## 二维费用的背包问题

有 $N$ 件物品，容积为 $V,U$ 的两个背包，每件物品有两种费用，选择物品需要付出两种代价，第 $i$ 件代价为 $a_i,b_i$，价值为 $c_i$。将第几件物品装入，使体积不超过总体积，且价值和最大，求最大价值。

改为二维数组即可<br/>
状态转移方程：$F_{v,u} = max(F_{v,u}\ , F_{v-a_i,u-b_i} + c_i)$<br/>
$F_{v,u}$ 表示前面的物品付出代价分别为 $v,u$ 时的最大价值<br/>
框架略

循环顺序
- 类01背包：`v = V..0  u = U..0`<br/>
- 类完全背包：`v = 0..V  u = 0..U`<br/>
- 类多重背包：拆分物品

## 分组的背包问题

有 $K$ 组物品， $V$ 的背包，第 $k$ 组有 $N_k$ 件物品，第 $i$ 件物品的体积为 $w_i$ ，价值为 $c_i$ ，每组中只能选一件物品。将第几件物品装入，使体积不超过总体积，且价值和最大，求最大价值。

#### 框架
```cpp
for (int k = 1; k <= K; ++k)
{
    for (int v = V; v >= 0; --v)
    {
        for (int i = 1; i <= N[k]; ++i)
            f[v] = max(f[v], f[v - w[i]] + c[i]);
    }
}
```

## 背包问题的方案数
状态转移方程：$F_{i,v} = sum(F_{i-1,v}, F_{i-1,v-w_i})\ \ \ (F_{0,0} = 1)$
#### 框架
```cpp
f[0] = 1;
for (int i = 1; i <= N; ++i)
{
    for (int j = w[i]; j <= V; ++j)
        f[j] += f[j - w[i]];
}
printf("%d", f[V]);
```
