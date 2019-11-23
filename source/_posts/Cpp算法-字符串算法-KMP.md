---
title: Cpp算法-字符串算法-KMP
toc: true
mathjax: true
date: 2019-01-10 13:09:57
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
例：[洛谷P3375](https://www.luogu.org/problemnew/show/P3375)

#### 说明
`pre()`求前缀数组<br/>
`kmp()`匹配字符串<br/>

#### 实现
```cpp
char s1[1000010], s2[1000010];
int nxt[1000010], l1, l2;

void pre()
{
    nxt[1] = 0;
    int j = 0;
    for (int i = 1; i < l2; ++i)
    {
        while (j > 0 && s2[j + 1] != s2[i + 1]) j = nxt[j];
        if (s2[j + 1] == s2[i + 1]) j++;
        nxt[i + 1] = j;
    }
}

void kmp()
{
    int j = 0;
    for (int i = 0; i < l1; ++i)
    {
        while (j > 0 && s2[j + 1] != s1[i + 1]) j = nxt[j];
        if (s2[j + 1] == s1[i + 1]) j++;
        if (j == l2)
        {
            printf("%d\n", i - l2 + 2);
            j = nxt[j];
        }
    }
}

int main()
{
    cin >> s1 + 1;
    cin >> s2 + 1;
    l1 = strlen(s1 + 1);
    l2 = strlen(s2 + 1);
    pre();
    kmp();
    return 0;
}
```