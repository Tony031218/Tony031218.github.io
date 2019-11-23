---
title: 「Luogu P1993」小K的农场-题解 & 差分约束系统
mathjax: true
toc: true
date: 2019-08-31 15:24:33
tags: [Cpp, 题解, Luogu, 图论, 差分约束]
categories: 题解
---


题目传送门: [「Luogu P1993」小K的农场](https://www.luogu.org/problem/P1993)

<!--more-->

### 题目大意
给出$m$个约束条件,$n$个农场
有三种条件,其中$a,b$表示编号为$a,b$的农场的植物个数
1. $a-b≥c$
2. $a-b≤c$
3. $a=b$
求是否存在一种方案,使农场中的植物数满足约束要求

### $I.$ 差分约束系统
以第$1$种约束为例:
$$a-b\ge c\Rightarrow b\le a+(-c)$$
与求最短路径中的三角形不等式$dis[e.to]\le dis[u] + e.val$类似
所以我们对于约束条件$a-b\ge c$,从$a$到$b$建一条边权为$-c$的边
同理三种约束条件依次为
1. $a-b\ge c$, 从$a$到$b$建$-c$单向边
2. $a-b\le c$, 从$b$到$a$建$c$单向边
3. $a=b$, 从$a$到$b$建权值为$0$的双向边

如果存在一组解${x_1, x_2, \ldots, x_n}$,则对任意常数$\Delta$, ${x_1+\Delta, x_2+\Delta, \ldots, x_n+\Delta}$也是一组解
不妨先求一组负数解,于是就有了条件$x_i-x_0\le 0$
即,从$0$向所有节点建一条边权为$0$的单向边

求解时,设$\mathtt{dis[0]=0}$,然后以$0$为源点求单源最短路
如果存在负环,则系统无解
不存在负环,则$\mathtt{dis[i]}$为系统的一组解

### $II.$ 负环
如果任意一条边被修改大于$n$次(执行$n$次松弛操作),这个图内一定存在至少一个负环
我们可以使用一个数组$cnt$来记录每条边执行松弛操作的次数
当向队列中添加节点$\mathtt{e.to}$时,$\mathtt{cnt[e.to]++}$
然后再判断$\mathtt{cnt[e.to]>n}$,如果返回$\mathtt{true}$则存在负环

### $III.$ $SPFA$的$SLF$优化
本题如果直接跑$SPFA$的话,会$TLE\ 3$个点,即使手动开了$O3$优化,还是会$TLE\ 1$个点
所以我们可以使用$SLF(Small\ Label\ First)$优化

$SLF:$
当加入一个新节点$v$的时候
- 如果此时的$\mathtt{dis[v]}$比队首$\mathtt{dis[q.front()]}$小的话，就把$v$点加入到队首
- 否则把他加入到队尾

因为先扩展最小的点可以尽量使程序尽早的结束

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

const int maxn = 10010;
const int inf  = 0x3f3f3f3f;

struct Edge {
    int from, to, val;
    Edge(int u, int v, int w): from(u), to(v), val(w) {}
};
vector<Edge> edges;
vector<int> G[maxn];
void add(int u, int v, int w) {
    edges.push_back(Edge(u, v, w));
    int mm = edges.size();
    G[u].push_back(mm - 1);
}

int n, m;
int dis[maxn], vis[maxn], cnt[maxn];

bool SPFA(int s) {
    deque<int> q; q.push_back(s);
    dis[s] = 0; vis[s] = true;
    while (!q.empty()) {
        int u = q.front(); q.pop_front(); vis[u] = false;
        for (int i = 0; i < G[u].size(); ++i) {
            Edge& e = edges[G[u][i]];
            if (dis[e.to] > dis[u] + e.val) {
                dis[e.to] = dis[u] + e.val;
                if (!vis[e.to]) {
                    vis[e.to] = true;
                    if (!q.empty() && dis[e.to] < dis[q.front()]) {
                        q.push_front(e.to);
                    } else {
                        q.push_back(e.to);
                    }
                    cnt[e.to]++;
                }
                if (cnt[e.to] > n) {
                    return false;
                }
            }
        }
    }
    return true;
}

int main() {
    n = read(); m = read();
    for (int i = 1; i <= m; ++i) {
        int opt = read();
        if (opt == 1) {
            int a = read(), b = read(), c = read();
            add(a, b, -c);
        } else if (opt == 2) {
            int a = read(), b = read(), c = read();
            add(b, a, c);
        } else {
            int a = read(), b = read();
            add(a, b, 0); add(b, a, 0);
        }
    }
    for (int i = 1; i <= n; ++i) {
        add(0, i, 0);
        dis[i] = inf;
    }
    printf(SPFA(0) ? "Yes\n" : "No\n");
    return 0;
}
/* 1.94s 1.61MB */
```