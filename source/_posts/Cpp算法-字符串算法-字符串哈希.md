---
title: Cpp算法-字符串算法-字符串哈希
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: 114af1e8
date: 2019-01-10 13:08:31
---
例：[洛谷P3370](https://www.luogu.org/problemnew/show/P3370)

#### 单哈希(自然溢出)
```cpp
typedef unsigned long long ULL;
ULL base = 131, a[10010];
char s[10010];

ULL hash(char* s)
{
    int len = strlen(s);
    ULL Ans = 0;
    for (int i = 0; i < len; i++)
        Ans = Ans * base + (ULL)s[i];
    return Ans & 0x7fffffff;
}
```

#### 单哈希(单模数)
```cpp
typedef unsigned long long ULL;
ULL base = 131, a[10010], mod = 19260817;
char s[10010];

ULL hash(char* s)
{
    int len = strlen(s);
    ULL Ans = 0;
    for (int i = 0; i < len; i++)
        Ans = (Ans * base + (ULL)s[i]) % mod;
    return Ans;
}
```

#### 单哈希(大模数)
```cpp
typedef unsigned long long ULL;
ULL base = 131, a[10010], mod = 212370440130137957LL;
char s[10010];
int prime = 233317;

ULL hash(char* s)
{
    int len = strlen(s);
    ULL Ans = 0;
    for (int i = 0; i < len; ++i)
        Ans = (Ans * base + (ULL)s[i]) % mod + prime;
    return Ans;
}
```

#### 双哈希
```cpp
typedef unsigned long long ULL;
ULL base = 131, mod1=19260817, mod2=19660813;
char s[10010];
struct data
{
    ULL x,y;
}a[10010];

ULL hash1(char* s)
{
    int len = strlen(s);
    ULL Ans = 0;
    for (int i = 0; i < len; i++)
        Ans = (Ans * base + (ULL)s[i]) % mod1;
    return Ans;
}

ULL hash2(char* s)
{
    int len = strlen(s);
    ULL Ans = 0;
    for (int i = 0; i < len; i++)
        Ans = (Ans * base + (ULL)s[i]) % mod2;
    return Ans;
}
```