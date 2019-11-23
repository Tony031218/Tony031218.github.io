---
title: 「图论算法」树上并查集 dsu on tree
mathjax: true
toc: true
date: 2019-09-22 15:36:46
tags: [Cpp, 算法, 笔记, 图论, dsu on tree]
categories: [C++算法, 笔记, 图论] 
---

$dsu\ on\ tree$($disjoint\ set\ union\ \text{on tree}$)算法，也称 __树上并查集__。使用了并查集的*按秩合并*(__启发式合并__)的方法，结合 *树链剖分* 中的 __轻重儿子划分__ ，对 __树上暴力统计__ 进行了优化。使用这个算法需要满足以下两个条件：

<!--more-->

- 每次只对子树进行查询
- 过程中没有对树的修改

可以将$O(n^2)$的暴力操作优化到$O(n\log_2n)$

## $pre.\ $树链剖分 - 轻重链划分

在$dsu\ on\ tree$算法中，只需要统计出每个节点的*轻重儿子*，而不需要划分轻重链，所以我们只需要树链剖分中的$\texttt{dfs1()}$，并且只保留和轻重儿子有关的操作即可

> 关于 __轻重儿子__ ：
>
> - 一个节点的所有子节点中，子树所含节点个数最多的子节点称为 __重儿子__
> - 除重儿子外，一个节点的其余儿子均为 __轻儿子__

```cpp
void dfs1(int x, int fa) {
    siz[x] = 1; // 以x为根的子树大小初始化为1
    for (int i = 0; i < G[x].size(); ++i) {
        Edge& e = edges[G[x][i]];
        if (e.to == fa) continue;
        dfs1(e.to, x);
        siz[x] += siz[e.to]; // 加上子节点的大小
        if (siz[e.to] > siz[son[x]]) {
            son[x] = e.to; // 更新重儿子
        }
    }
}
```

## $I.\ $算法流程

对于每一个节点$x$:

- 遍历所有子节点
    - 递归计算所有轻儿子的子树，并删除其对答案的贡献
- 递归计算重儿子，保留对答案的贡献
- 统计所有轻儿子对答案的贡献
- 更新节点答案

伪代码：

```cpp
void dfs(int x, int fa, int opt) {
    for (all edge from x) {
        if (e.to == fa || e.to == son[x]) continue;
        dfs(e.to, x, 0); // 暴力统计轻儿子的子树的贡献，并删除(opt=0)
    }
    if (son[x]) dfs(son[x], x, 1); // 统计重儿子的贡献，不删除
    upd(x); // 暴力统计该节点所有轻儿子的贡献
    ans[x] = now; // 更新答案
    if (!opt) {
        del(x); // 删除需要删除的贡献
    }
}
```

```cpp
void upd(int x, int fa, int val) {
    solve(x); // 计算当前答案
    for (int i = 0; i < G[x].size(); ++i) {
        Edge& e = edges[G[x][i]];
        if (e.to == fa || e.to == Son) continue;
        upd(e.to, x, val);
    }
}
```

由于暴力统计的只是轻儿子，而每个节点的轻儿子不超过$\log_2n$个，所以整个算法复杂度保证在了$O(n\log_2n)$

## $II.\ $模板题

[$CodeForces\ 600E\ \ Lomsat\ gelral$](https://www.luogu.org/problemnew/solution/CF600E) 

 ```cpp
#include <bits/stdc++.h>
using namespace std;

inline int read() {
    int x = 0; int f = 1; char ch = getchar();
    while (!isdigit(ch)) {if (ch == '-') f = -1; ch = getchar();}
    while (isdigit(ch))  {x = x * 10 + ch - 48; ch = getchar();}
    return x * f;
}

const int maxn = 100010;

struct Edge {
    int from, to;
    Edge(int u, int v): from(u), to(v) {}
};
vector<Edge> edges;
vector<int> G[maxn];
void add(int u, int v) {
    edges.push_back(Edge(u, v));
    int mm = edges.size();
    G[u].push_back(mm - 1);
}

int n, Mx, Son, col[maxn], son[maxn], siz[maxn], cnt[maxn];
long long sum = 0, ans[maxn];

void dfs1(int x, int fa) {
    siz[x] = 1;
    for (int i = 0; i < G[x].size(); ++i) {
        Edge& e = edges[G[x][i]];
        if (e.to == fa) continue;
        dfs1(e.to, x);
        siz[x] += siz[e.to];
        if (siz[e.to] > siz[son[x]]) {
            son[x] = e.to;
        }
    }
}

void Add(int x, int fa, int val) {
    cnt[col[x]] += val;
    if (cnt[col[x]] > Mx) {
        Mx = cnt[col[x]];
        sum = col[x];
    } else if (cnt[col[x]] == Mx) {
        sum += (long long)col[x];
    }
    for (int i = 0; i < G[x].size(); ++i) {
        Edge& e = edges[G[x][i]];
        if (e.to == fa || e.to == Son) continue;
        Add(e.to, x, val);
    }
}

void dfs2(int x, int fa, int opt) {
    for (int i = 0; i < G[x].size(); ++i) {
        Edge& e = edges[G[x][i]];
        if (e.to == fa) continue;
        if (e.to != son[x]) dfs2(e.to, x, 0);
    }
    if (son[x]) {
        dfs2(son[x], x, 1);
        Son = son[x];
    }
    Add(x, fa, 1); Son = 0;
    ans[x] = sum;
    if (!opt) {
        Add(x, fa, -1);
        sum = 0, Mx = 0;
    }
}

int main() {
    n = read();
    for (int i = 1; i <= n; ++i) {
        col[i] = read();
    }
    for (int i = 1; i < n; ++i) {
        int u = read(), v = read();
        add(u, v); add(v, u);
    }
    dfs1(1, 0);
    dfs2(1, 0, 0);
    for (int i = 1; i <= n; ++i) {
        printf("%lld ", ans[i]);
    }
    return 0;
}
 ```