---
title: Cpp算法-树状数组
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: 72c90980
date: 2019-01-10 12:57:04
---
# 树状数组

模板：[洛谷P3374](https://www.luogu.org/problemnew/show/P3374)

#### 说明
`tree[]`树状数组<br/>
`lowbit(int)`神奇的函数<br/>
`add(int x, int k)`第 $x$ 个数加上 $k$ <br/>
`sum(int x)`前 $x$ 个数的和

#### 实现
```cpp
int tree[2000010];

int lowbit(int k)
{
    return k & -k;
}

void add(int x, int k)
{
    while (x <= n)
    {
        tree[x] += k;
        x += lowbit(x);
    }
}

int sum(int x)
{
    int ans = 0;
    while (x != 0)
    {
        ans += tree[x];
        x -= lowbit(x);
    }
    return ans;
}
```