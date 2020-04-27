---
title: 「Luogu P1494」小Z的袜子-题解
mathjax: true
toc: true
abbrlink: '49548215'
date: 2020-04-27 14:53:47
tags:
  - Cpp
  - 题解
  - NOIp
  - 莫队
  - 数论
categories:
  - 题解
---

题目传送门: [「Luogu P1494」小Z的袜子](https://www.luogu.com.cn/problem/P1494)
一道推公式，后使用莫队 ~~玄学~~ 优化的题目

<!--more-->

### 题目大意
给出$n$个袜子,第$i$只袜子的颜色为$c_i$
有$m$个询问,用$L,R$表示
在区间$[L,R]$中随机取袜子,求取出两只袜子颜色相同的概率(最简分数)

### 题解
考虑区间$[L,R]$,其中颜色为$A$的袜子有$a$只,颜色为$B$的袜子有$b$只$...$

取出两只袜子的总情况数为
$$C_{R-L+1}^2=\\frac{(R-L+1)(R-L)}{2}$$
取出两只袜子颜色都为$A$的情况数为
$$C_a^2=\\frac{a(a - 1)}{2}$$
所以,取出两只袜子颜色相同的情况数为
$$\\sum_{i}C_i^2=C_a^2+C_b^2+...=\\frac{a(a-1)}{2}+\\frac{b(b-1)}{2}+...$$
所以最终的概率为
$$
\begin{aligned}
P&=\\frac{\\displaystyle\\sum_{i}C_i^2}{C_{R-L+1}^2}\\\\\\\\
&=\\dfrac{\\dfrac{a(a-1)}{2}+\\dfrac{b(b-1)}{2}+...}{ \\dfrac{(R-L+1)(R-L)}{2} }\\\\\\\\
&=\\dfrac{a^2+b^2+...-a-b-...}{(R-L+1)(R-L)}\\\\\\\\
&=\\dfrac{\\displaystyle\\sum_ii^2-\\displaystyle\\sum_ii}{(R-L+1)(R-L)}\\\\\\\\
&=\\dfrac{\\displaystyle\\sum_ii^2-(R-L+1)}{(R-L+1)(R-L)}
\end{aligned}
$$
所以要求的就是$\displaystyle\sum_ii^2$,可以用莫队来维护区间平方和得到

对于最终结果的表达式,令$a=$分子,$b=$分母,求出$ab$的最大公约数,并除去
最终答案即为$a/b$

### 代码
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

inline int read() {
    int x = 0; int f = 1; char ch = getchar();
    while (!isdigit(ch)) {if (ch == '-') f = -1; ch = getchar();}
    while (isdigit(ch))  {x = x * 10 + ch - 48; ch = getchar();}
    return x * f;
}
LL gcd(LL a, LL b) { return b == 0 ? a : gcd(b, a % b); }

const int maxn = 50010;

struct Query {
    int l, r, pos, id;
}q[maxn];
bool cmp(Query a, Query b) {
    if (a.pos != b.pos) return a.pos < b.pos;
    if (a.pos & 1) return a.r < b.r;
    return a.r > b.r;
}
struct Answer {
    LL a, b;
}ans[maxn];

int n, m, l, r, Ans, len;
LL c[maxn], cnt[maxn];

void del(int x) {
    Ans -= cnt[c[x]] * cnt[c[x]];
    cnt[c[x]]--;
    Ans += cnt[c[x]] * cnt[c[x]];
}
void add(int x) {
    Ans -= cnt[c[x]] * cnt[c[x]];
    cnt[c[x]]++;
    Ans += cnt[c[x]] * cnt[c[x]];
}

int main() {
    n = read(); m = read(); len = sqrt(n);
    for (int i = 1; i <= n; ++i) c[i] = read();
    for (int i = 1; i <= m; ++i) {
        q[i].l = read(); q[i].r = read();
        q[i].id = i; q[i].pos = q[i].l / len + 1;
    }
    sort(q + 1, q + 1 + m, cmp); l = 1;
    for (int i = 1; i <= m; ++i) {
        while (l < q[i].l) del(l++);
        while (r > q[i].r) del(r--);
        while (l > q[i].l) add(--l);
        while (r < q[i].r) add(++r);
        if (l == r) {
            ans[q[i].id].a = 0; ans[q[i].id].b = 1;
            continue;
        }
        LL a = Ans - (r - l + 1);
        LL b = 1LL * (r - l + 1) * (LL)(r - l);
        LL g = gcd(a, b);
        ans[q[i].id].a = a / g;
        ans[q[i].id].b = b / g;
    }
    for (int i = 1; i <= m; ++i) {
        printf("%lld/%lld\n", ans[i].a, ans[i].b);
    }
    return 0;
}
```