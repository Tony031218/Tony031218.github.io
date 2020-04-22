---
title: 「Luogu P1516」青蛙的约会-题解
mathjax: true
toc: true
top: 4
tags:
  - Cpp
  - 题解
  - Luogu
  - 数论
categories: 题解
abbrlink: 1f4bfad8
date: 2019-05-08 16:17:20
---

题目传送门: [「Luogu P1516」青蛙的约会](https://www.luogu.org/problemnew/show/P1516)

<!--more-->

### 题目大意
(规定纬度线上东经0度处为原点，由东往西为正方向，单位长度1米，这样我们就得到了一条首尾相接的数轴)
现有两只青蛙$A,B$
设青蛙$A$的出发点坐标是$x$，青蛙$B$的出发点坐标是$y$
青蛙$A$一次能跳$m$米，青蛙$B$一次能跳$n$米，两只青蛙跳一次所花费的时间相同
纬度线总长$l$米
求两只青蛙跳了几次以后才会碰面

### 题解
__*同余方程*__模板题
求解$x + km\equiv y + kn \pmod l$

$Solve:$
$$
x+km−(y+kn)=lz,\ \ z\in Z\\\\
(x-y)+k(m-n)-lz=0\\\\
k(n-m)+lz=(x-y)
$$
令$a=x-y,b=n-m$
上式可化为:
$$
kb+lz=a
$$
求这个方程的最小整数解
化为求此不定方程最小整数解
$$k'b+lz'=gcd(b,l)$$
使用扩展欧几里得算法可得一组特解$(k',b')$
最小解为$k_{min} = k'\bmod \frac{l}{gcd(b,l)}$
以上解$k_{min}$的方程右边是$gcd(b,l)$而不是$a$
所以结果为
$$\boxed{ (k'\times \frac{a}{gcd(b,l)})\bmod \frac{l}{gcd(b,l)} }$$

### 代码
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

void exgcd(LL a, LL b, LL& d, LL& x, LL& y) {
    if (!b) { d = a; x = 1; y = 0; }
    else { exgcd(b, a % b, d, y, x); y -= x * (a / b); }
}

int main() {
    LL n, m, x, y, l, gcd, x1, y1;
    scanf("%lld %lld %lld %lld %lld", &x, &y, &m, &n, &l);
    LL b = n - m, a = x - y;
    if (b < 0) { b = -b, a = -a; }
    exgcd(b, l, gcd, x1, y1);
    if (a % gcd) {
        printf("Impossible\n");
    } else {
        LL ans = ((x1 * (a / gcd)) % (l / gcd) + (l / gcd)) % (l / gcd);
        printf("%lld\n", ans);
    }
    return 0;
}
/* 26ms 916kB */
```