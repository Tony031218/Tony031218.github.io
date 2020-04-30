---
title: 树上莫队-笔记  /「SPOJ 10707」COT2-题解
mathjax: true
toc: true
abbrlink: 24f5ddbc
date: 2020-04-30 14:35:44
tags: [Cpp, 莫队, 算法, 笔记, 题解, NOIp, SPOJ]
categories: [C++算法, 笔记]
---

通过[SPOJ 10707 COT2-Count on a tree II](https://www.luogu.com.cn/problem/SP10707)这道题目来学习一下 __树上莫队__
当需要离线查询 __树上__ 的多区间问题时,可以使用 __树上莫队__ 来解决

主要通过 __欧拉序__ 将树转化为一条链,然后在链上执行普通莫队的操作
<!--more-->

## 树上莫队
### 欧拉序
正常进行`dfs`,在入和出时各加入序列中
比如样例的树如下:
![](/24f5ddbc/graph.png)
其欧拉序为`1 2 2 3 5 5 6 6 7 7 3 4 8 8 4 1`
可以很好地呈现出子树的关系,即两个相同的数$x$之间的部分为$x$子树中的节点
其有一个性质:__区间内出现两次的点不在其路径上__
根据这个性质,可以将树转化为链来求解了
### 思想
除了将树转化为欧拉序之外,还需要求出左右端点的$LCA$,以及一个点$\texttt{u}$在欧拉序中第一次出现的位置$\texttt{fst[u]}$,第二次(最后一次)出现的位置$\texttt{lst[u]}$

在进行莫队操作时,如果第一次经过这个点,则`add`其贡献,第二次经过这个点,则说明这个点不在所求链上,`del`其贡献
这个用一个`vis`数组,反复进行异或操作就可以解决

再考虑询问的区间的$l,r$应该赋值为$\texttt{fst}$还是$\texttt{lst}$
设左端点的深度小于右端点
1. 如果$LCA$和左端点相等,则说明$[l,r]$在一条链上,$l$和$r$均取$\texttt{fst}$即可
2. 否则是两条链$[l, LCA],[LCA,r]$, 防止左右端点被统计两次导致贡献被删除,需要$l$取$\texttt{lst}$,$r$取$\texttt{fst}$

最后考虑贡献
1. 若是上面第一种情况,在一条链上,直接统计欧拉序区间内所有点即可,重复两次的根据前文的性质会直接删掉
2. 若是上面第二种情况,由于左右端点都在$LCA$这颗子树内,所以区间中并不会出现$LCA$,但是却一定会经过,所以额外将$LCA$加入贡献,并且记录下当前结果之后,再将其贡献减去,防止影响下一个查询

__注意:__ 转化为欧拉序之后的序列长度为$2n$

### 时间复杂度
1. `dfs`: $O(n)$
2. 求$LCA$:$O(n\log n)$
3. 莫队: $O(n\sqrt{n})$

综上,树上莫队的复杂度 ~~差不多~~ 也是$O(n\sqrt{n})$

## 针对$\mathcal{SP10707}$的具体实现
没啥说的,模板题,做法全在上面了
### 代码
```cpp
#include <bits/stdc++.h>
using namespace std;

inline int read() {
    int x = 0; int f = 1; char ch = getchar();
    while (!isdigit(ch)) {if (ch == '-') f = -1; ch = getchar();}
    while (isdigit(ch))  {x = x * 10 + ch - 48; ch = getchar();}
    return x * f;
}

const int maxn = 200020;

int n, m, l, r, Ans, len, ocnt;
int cnt[maxn], fst[maxn], lst[maxn], vis[maxn], ans[maxn];
int ord[maxn], val[maxn], dep[maxn], fa[maxn][25], old[maxn];

struct Query {
    int l, r, id, pos, lca;
}q[maxn];
bool cmp(Query a, Query b) {
    if (a.pos != b.pos) return a.pos < b.pos;
    if (a.pos & 1) return a.r < b.r;
    return a.r > b.r;
}

struct Edge {
    int from, to;
    Edge(int f, int t): from(f), to(t) {}
};
vector<Edge> edges;
vector<int> G[maxn];
void add(int f, int t) {
    edges.push_back(Edge(f, t));
    edges.push_back(Edge(t, f));
    int mm = edges.size();
    G[t].push_back(mm - 1);
    G[f].push_back(mm - 2);
}

void dfs(int u, int f) {
    ord[++ocnt] = u; fst[u] = ocnt;
    for (int i = 0; i < G[u].size(); ++i) {
        Edge& e = edges[G[u][i]];
        if (e.to == f) continue;
        dep[e.to] = dep[u] + 1;
        fa[e.to][0] = u;
        for (int j = 1; j <= 20; ++j) {
            fa[e.to][j] = fa[fa[e.to][j - 1]][j - 1];
        }
        dfs(e.to, u);
    }
    ord[++ocnt] = u; lst[u] = ocnt;
}

int lca(int x, int y) {
    if (dep[x] > dep[y]) swap(x, y);
    for (int i = 20; i >= 0; --i) {
        if (dep[fa[y][i]] >= dep[x]) y = fa[y][i];
    }
    if (x == y) return x;
    for (int i = 20; i >= 0; --i) {
        if (fa[x][i] != fa[y][i]) {
            x = fa[x][i];
            y = fa[y][i];
        }
    }
    return fa[x][0];
}

void add(int x) {
    cnt[val[x]]--;
    if (!cnt[val[x]]) Ans--;
}
void del(int x) {
    cnt[val[x]]++;
    if (cnt[val[x]] == 1) Ans++;
}
void chg(int x) {
    if (vis[x]) add(x);
    else del(x);
    vis[x] ^= 1;
}

int main() {
    n = read(); m = read(); len = sqrt(2 * n);
    for (int i = 1; i <= n; ++i) old[i] = val[i] = read();
    sort(old + 1, old + 1 + n); int len_ = unique(old + 1, old + 1 + n) - old - 1;
    for (int i = 1; i <= n; ++i) val[i] = lower_bound(old + 1, old + 1 + len_, val[i]) - old;
    for (int i = 1; i < n; ++i) add(read(), read()); 
    dep[1] = 1; dfs(1, 0);
    for (int i = 1; i <= m; ++i) {
        int il = read(), ir = read();
        int LCA = lca(il, ir);
        if (fst[il] > fst[ir]) swap(il, ir);
        if (il == LCA) {
            q[i].l = fst[il]; q[i].r = fst[ir];
        } else {
            q[i].l = lst[il]; q[i].r = fst[ir]; q[i].lca = LCA;
        }
        q[i].id = i; q[i].pos = (q[i].l - 1) / len + 1;
    }
    sort(q + 1, q + 1 + m, cmp); l = 1;
    for (int i = 1; i <= m; ++i) {
        while (l < q[i].l) chg(ord[l++]);
        while (r > q[i].r) chg(ord[r--]);
        while (l > q[i].l) chg(ord[--l]);
        while (r < q[i].r) chg(ord[++r]);
        if (q[i].lca) chg(q[i].lca);
        ans[q[i].id] = Ans;
        if (q[i].lca) chg(q[i].lca);
    }
    for (int i = 1; i <= m; ++i) {
        printf("%d\n", ans[i]);
    }
    return 0;
}
```