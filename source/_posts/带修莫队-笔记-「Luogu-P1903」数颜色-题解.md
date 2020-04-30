---
title: 带修莫队-笔记  /「Luogu P1903」数颜色-题解
mathjax: true
toc: true
abbrlink: 838c5e29
date: 2020-04-29 09:00:14
tags: [Cpp, 莫队, 算法, 笔记, 题解, NOIp, Luogu]
categories: [C++算法, 笔记]
---

通过[Luogu P1903 数颜色/维护序列](https://www.luogu.com.cn/problem/P1903)这道题目来学习一下 __带修莫队__
顾名思义,__带修莫队__ 不仅要支持普通莫队的查询操作,还要支持数据中途的修改

比如这道题目,需要实现以下目标
1. 查询$[L,R]$区间内不同颜色画笔的种数
2. 将$pos$处的画笔替换为$color$颜色

达到这个目标,可以在普通莫队的基础上加一个时间维度,实现 __带修莫队__
<!--more--> 

## 带修莫队

### 时间戳
这里的每个查询的时间戳规定为 __最近修改操作的时间戳__,即最近一次修改是第几次修改
修改操作会增加总时间戳,查询操作不会增加时间戳

### 思想
在普通莫队的左右两个指针的基础之上 *增加* 一个 __时间戳指针__
当左右端点及时间戳移动到均和当前查询的一致,就可以记录下当前答案

所以需要在普通莫队的基础之上加上修改时间戳的修改操作,并加上以下两个判断
```cpp
while (t < q[i].t) chg(++t);
while (t > q[i].t) chg(t--);
```
当当前时间小于询问时间时,先将当前时间$+1$,再修改
当当前时间大于询问时间时,先修改,再将当前时间$-1$

与普通莫队还有一点不同:
所有询问的排序方法,先按照左端点分块升序,再按照右端点 __分块升序__,最后按照时间戳升序
这样复杂度才会达到最优,节省了一系列不必要的操作

### 时间复杂度
当分块的大小为$n^{\frac{2}{3}}$时,复杂度最小为$O(n^{\frac{5}{3}})$
具体分析见上一篇文章:[浅析莫队算法的时间复杂度](681257d9.html)

## 针对$\mathcal{P1903}$的具体实现

在每个询问`Query`的结构体内加一个时间戳$t$,并且按照上文实现排序
```cpp
struct Query {
    int l, r, t, id;
}q[maxn];
bool cmp(Query a, Query b) {
    if (block[a.l] != block[b.l]) return block[a.l] < block[b.l];
    if (block[a.r] != block[b.r]) return block[a.r] < block[b.r];
    return a.t < b.t;
}
```

再建一个结构体`Change`,表示每次修改操作的数据,需要$pos$和$color$
```cpp
struct Change {
    int pos, color;
}c[maxn];
```

正常的`add/del`操作不再赘述
现在来看一下修改时间对应数据的操作
1. 当当前时间的操作的位置$pos$在当前区间$[l,r]$时,对答案有影响,需要调整当前答案
先将$pos$位置上的贡献删去,再将当前修改操作的$color$添加进去
2. 将$pos$位置上的数与$color$交换,这样可以保证之后可以再换回来

实现如下:
```cpp
void chg(int t) {
    if (l <= c[t].pos && c[t].pos <= r) {
        if (--cnt[a[c[t].pos]] == 0) Ans--; // 删除贡献
        if (cnt[c[t].color]++  == 0) Ans++; // 添加贡献
    }
    swap(a[c[t].pos], c[t].color); // 交换
}
```

另外这题修改数据后严重卡常,手动开了O3,Ofast,inline才过

### 代码
```cpp
#pragma GCC optimize(3)
#pragma GCC optimize("Ofast")
#pragma GCC optimize("inline")
#include <bits/stdc++.h>
using namespace std;

inline int read() {
    int x = 0; int f = 1; char ch = getchar();
    while (!isdigit(ch)) {if (ch == '-') f = -1; ch = getchar();}
    while (isdigit(ch))  {x = x * 10 + ch - 48; ch = getchar();}
    return x * f;
}

const int maxn = 140000;

int n, m, l, r, t, len, cntq, cntr, Ans;
int a[maxn], cnt[1000010], ans[maxn], block[maxn];

struct Query {
    int l, r, t, id;
}q[maxn];
bool cmp(Query a, Query b) {
    if (block[a.l] != block[b.l]) return block[a.l] < block[b.l];
    if (block[a.r] != block[b.r]) return block[a.r] < block[b.r];
    return a.t < b.t;
}

struct Change {
    int pos, color;
}c[maxn];

void add(int x) {
    if (cnt[a[x]] == 0) Ans++;
    cnt[a[x]]++;
}
void del(int x) {
    if (cnt[a[x]] == 1) Ans--;
    cnt[a[x]]--;
}
void chg(int t) {
    if (l <= c[t].pos && c[t].pos <= r) {
        del(c[t].pos);
        if (cnt[c[t].color] == 0) Ans++;
        cnt[c[t].color]++;
    }
    swap(a[c[t].pos], c[t].color);
}

int main() {
    n = read(); m = read(); len = pow(n, 2.0 / 3.0);
    for (int i = 1; i <= n; ++i) {
        a[i] = read();
        block[i] = (i - 1) / len + 1;
    }
    for (int i = 1; i <= m; ++i) {
        char opt[10]; scanf("%s", opt);
        if (opt[0] == 'Q') {
            q[++cntq].l = read(); q[cntq].r = read();
            q[cntq].id = cntq; q[cntq].t = cntr;
        } else {
            c[++cntr].pos = read();
            c[cntr].color = read();
        }
    }
    sort(q + 1, q + 1 + cntq, cmp); l = 1;
    for (int i = 1; i <= cntq; ++i) {
        while (l < q[i].l) del(l++);
        while (r > q[i].r) del(r--);
        while (l > q[i].l) add(--l);
        while (r < q[i].r) add(++r);
        while (t < q[i].t) chg(++t);
        while (t > q[i].t) chg(t--);
        ans[q[i].id] = Ans;
    }
    for (int i = 1; i <= cntq; ++i) {
        printf("%d\n", ans[i]);
    }
    return 0;
}
```