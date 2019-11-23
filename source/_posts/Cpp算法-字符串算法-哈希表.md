---
title: Cpp算法-字符串算法-哈希表
toc: true
mathjax: true
date: 2019-01-10 13:09:06
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
例：[洛谷P4305](https://www.luogu.org/problemnew/show/P4305)

#### 说明
`hash[]`哈希表<br/>
`find(int x)`查找哈希表中 $x$ 的位置<br/>
`push(int x)`将 $x$ 插入到哈希表中<br/>
`check(int x)`查找 $x$ 是否在哈希表中

#### 实现
```cpp
#define p 100003
#define hash(a) a%p

int h[p], t, n, x;

int find(int x)
{
    int y;
    if (x < 0) y = hash(-x);
    else y = hash(x);
    while (h[y] && h[y] != x) y = hash(++y);
    return y;
}

void push(int x)
{
    h[find(x)] = x;
}

bool check(int x)
{
    return h[find(x)] == x;
}

int main()
{
    scanf("%d", &t);
    while (t--)
    {
        memset(h, 0, sizeof(h));
        scanf("%d", &n);
        while (n--)
        {
            scanf("%d", &x);
            if (!check(x))
            {
                printf("%d ", x);
                push(x);
            }
        }
        printf("\n");
    }
    return 0;
}
```