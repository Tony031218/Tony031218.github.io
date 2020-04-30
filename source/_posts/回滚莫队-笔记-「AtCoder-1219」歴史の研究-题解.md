---
title: 回滚莫队-笔记  /「AtCoder 1219」歴史の研究-题解
mathjax: true
toc: true
abbrlink: 7d7b5548
date: 2020-04-30 10:58:35
tags: [Cpp, 莫队, 算法, 笔记, 题解, NOIp, AtCoder]
categories: [C++算法, 笔记]
---

通过[AtCoder 1219 歴史の研究](https://www.luogu.com.cn/problem/AT1219)这道题目来学习一下 __回滚莫队__
__回滚莫队__ 适用于容易进行`add`操作,而不容易实现`del`的情况

通过莫队的分块,指针移动的思想,可以让左指针进行回滚操作, *近似* 达到`del`的效果
<!--more-->

## 回滚莫队
### 思想
由于莫队对所有询问离线排序后,当左端点在同一个块内时,右端点递增
所以对于每个块,右指针直接向右依次执行`add`操作即可

对于左指针,在一个块内时,可以每次都从块的右边界向左进行`add`,由于不方便进行`del`操作,所以可以先记录下左指针在右边界时的`Ans`,然后每次向左移动到`q[i].l`时,将左指针再移回右边界,并且将`Ans`回滚到移动之前的值。由于分块,这样做的复杂度也不会很大

综上,对于每个块,__右指针依次向右推进,左指针在右边界和查询的左端点之间反复横跳__
这样,执行的就只剩`add`操作,通过左指针的横跳,避免了`del`操作

注意,当左右端点都在同一个块时,只要暴力求出结果就可以了
__一定要注意__: 不要使用奇偶排序,必须保证右端点的 __单调递增__

对于每个块内的处理,大概如下图:
![橙色箭头:左指针的移动 蓝色箭头:右指针的移动](/7d7b5548/RollBackMosAlgo.png)

### 时间复杂度
时间复杂度由以下几个方面组成
1. 询问排序
2. 同一个块内的暴力求解
3. 左指针的移动(~~横跳~~)
4. 右指针的顺次移动

下面来 ~~不严谨~~ 简要地计算一下时间复杂度
1. __排序__:$O(n\log n)$
2. __暴力__:暴力的区间最长为$\sqrt{n}$,所以单次暴力的复杂度为$O(\sqrt{n})$,$n$次暴力的复杂度为$O(n\sqrt{n})$~~其实到不了n次~~
3. __左指针移动__: 进行`add`操作的复杂度为$O(1)$,块长$\sqrt{n}$,每次左移最坏复杂度$O(\sqrt{n})$,回滚时仍需要$O(\sqrt{n})$清除贡献
所以对于所有块,一共要移动$q$次,总的复杂度为$O(2q\sqrt{n})$
4. __右指针移动__: 对于每个块,最坏只要移动$n$次,一共$\sqrt{n}$个块,所以复杂度为$O(n\sqrt{n})$

综上,总的复杂度为$O(n\log n)+O(2q\sqrt{n})+O(n\sqrt{n})\ \sim\ O(n\sqrt{n})$

## 针对$\mathcal{AT1219}$的具体实现
添加贡献的`add`操作很容易实现
```cpp
void add(int x) {
    cnt[a[x]]++;
    Ans = max(Ans, 1LL * cnt[a[x]] * old[a[x]]);
}
```

同一块内的暴力也很容易实现
```cpp
LL solve(int l, int r) {
    LL res = 0;
    for (int i = l; i <= r; ++i) cnt2[a[i]] = 0;
    for (int i = l; i <= r; ++i) {
        cnt2[a[i]]++;
        res = max(res, 1LL * cnt2[a[i]] * old[a[i]]);
    }
    return res;
}
```

其余情况下根据前面所说,可以实现
```cpp
while (r < q[i].r) add(++r);  // 右指针右移,添加贡献
LL tmp = Ans;                 // 记录左指针移动前的答案
while (l > q[i].l) add(--l);  // 左指针左移,添加贡献
ans[q[i].id] = Ans;
while (l < rpos[k] + 1) cnt[a[l++]]--; // 左指针移动回右边界,并途中删除对cnt的贡献
Ans = tmp;                    // 回滚到移动前的答案
```

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

const int maxn = 100010;

int n, m, len, l, r;
int a[maxn], cnt[maxn], rpos[maxn], old[maxn], cnt2[maxn];
LL Ans, ans[maxn];

struct Query {
    int l, r, id, pos;
}q[maxn];
bool cmp(Query a, Query b) {
    if (a.pos != b.pos) return a.pos < b.pos;
    return a.r < b.r;
}

LL solve(int l, int r) {
    LL res = 0;
    for (int i = l; i <= r; ++i) cnt2[a[i]] = 0;
    for (int i = l; i <= r; ++i) {
        cnt2[a[i]]++;
        res = max(res, 1LL * cnt2[a[i]] * old[a[i]]);
    }
    return res;
}

void add(int x) {
    cnt[a[x]]++;
    Ans = max(Ans, 1LL * cnt[a[x]] * old[a[x]]);
}

int main() {
    n = read(); m = read(); len = sqrt(n); int num = ceil((double)n / len);
    for (int i = 1; i <= num; ++i) rpos[i] = len * i; rpos[num] = n;
    for (int i = 1; i <= n; ++i) old[i] = a[i] = read();
    sort(old + 1, old + 1 + n);
    int len_ = unique(old + 1, old + 1 + n) - old - 1;
    for (int i = 1; i <= n; ++i) a[i] = lower_bound(old + 1, old + 1 + len_, a[i]) - old;
    for (int i = 1; i <= m; ++i) {
        q[i].l = read(); q[i].r = read();
        q[i].id = i; q[i].pos = (q[i].l - 1) / len + 1;
    }
    sort(q + 1, q + 1 + m, cmp); l = 1;
    for (int k = 1, i = 1; k <= num; ++k) {
        l = rpos[k] + 1, r = rpos[k], Ans = 0;
        memset(cnt, 0, sizeof(cnt));
        while (q[i].pos == k) {
            if (q[i].l / len == q[i].r / len) {
                ans[q[i].id] = solve(q[i].l, q[i].r);
                i++; continue;
            }
            while (r < q[i].r) add(++r);
            LL tmp = Ans;
            while (l > q[i].l) add(--l);
            ans[q[i].id] = Ans;
            while (l < rpos[k] + 1) cnt[a[l++]]--;
            Ans = tmp; i++;
        }
    }
    for (int i = 1; i <= m; ++i) {
        printf("%lld\n", ans[i]);
    }
    return 0;
}
```