---
title: Cpp算法-网络流相关
mathjax: true
toc: true
tags:
  - Cpp
  - 算法
categories: C++算法
abbrlink: c3cdab79
date: 2019-04-30 12:56:50
---

网络流算法相关模板,讲解__以后再说吧__,咕咕咕

<!--more-->

注:本文使用`vector`建图

## 网络最大流
### Edmonds-Karp算法
```cpp
int n, m, s, t, ans;

struct Edge {
    int from, to, cap, flow;
    Edge(int u, int v, int c, int f): from(u), to(v), cap(c), flow(f){}
};

vector<Edge> edges;
vector<int> G[maxn];
int a[maxn], p[maxn];

void add(int from, int to, int cap) {
    edges.push_back(Edge(from, to, cap, 0));
    edges.push_back(Edge(to, from, 0, 0));
    int mm = edges.size();
    G[from].push_back(mm - 2);
    G[to].push_back(mm - 1);
}

void EdmondsKarp() {
    ans = 0;
    for (;;) {
        memset(a, 0, sizeof(a));
        queue<int> Q;
        Q.push(s);
        a[s] = 0x3f3f3f3f;
        while (!Q.empty()) {
            int x = Q.front(); Q.pop();
            for (int i = 0; i < G[x].size(); ++i) {
                Edge& e = edges[G[x][i]];
                if (!a[e.to] && e.cap > e.flow) {
                    p[e.to] = G[x][i];
                    a[e.to] = min(a[x], e.cap - e.flow);
                    Q.push(e.to);
                }
            }
            if (a[t]) break;
        }
        if (!a[t]) break;
        for (int u = t; u != s; u = edges[p[u]].from) {
            edges[p[u]].flow += a[t];
            edges[p[u] ^ 1].flow -= a[t];
        }
        ans += a[t];
    }
    return;
}
```

### Dinic算法(常用)
```cpp
const int maxn = 10010;

int n, m, s, t, d[maxn], cur[maxn];
struct Edge {
    int from, to, cap, flow;
};
vector<Edge> edges;
vector<int> G[maxn];
void add(int from, int to, int cap) {
    edges.push_back((Edge) {from, to, cap, 0});
    edges.push_back((Edge) {to, from, 0, 0});
    int mm = edges.size();
    G[from].push_back(mm - 2);
    G[to].push_back(mm - 1);
}

bool vis[maxn];

bool BFS() {
    memset(vis, 0, sizeof(vis));
    queue<int> Q;
    Q.push(s);
    d[s] = 0;
    vis[s] = 1;
    while (!Q.empty()) {
        int x = Q.front(); Q.pop();
        for (int i = 0; i < G[x].size(); ++i) {
            Edge& e = edges[G[x][i]];
            if (!vis[e.to] && e.cap > e.flow) {
                vis[e.to] = 1;
                d[e.to] = d[x] + 1;
                Q.push(e.to);
            }
        }
    }
    return vis[t];
}

int DFS(int x, int a) {
    if (x == t || a == 0) return a;
    int flow = 0, f;
    for (int& i = cur[x]; i < G[x].size(); ++i) {
        Edge& e = edges[G[x][i]];
        if (d[x] + 1 == d[e.to] && (f = DFS(e.to, min(a, e.cap - e.flow))) > 0) {
            e.flow += f;
            edges[G[x][i] ^ 1].flow -= f;
            flow += f;
            a -= f;
            if (a == 0) break;
        }
    }
    return flow;
}

int MaxFlow(int s, int t) {
    int flow = 0;
    while (BFS()) {
        memset(cur, 0, sizeof(cur));
        flow += DFS(s, 0x3f3f3f3f);
    }
    return flow;
}
```

### ISAP算法
```cpp
const int maxn = 10010;
const int inf  = 0x3f3f3f3f;

int n, m, s, t;
int d[maxn], p[maxn], num[maxn], cur[maxn];
bool vis[maxn];

struct Edge {
    int from, to, cap, flow;
    Edge(int u, int v, int c, int f): from(u), to(v), cap(c), flow(f){}
};
vector<Edge> edges;
vector<int> G[maxn];
void add(int from, int to, int cap) {
    edges.push_back(Edge(from, to, cap, 0));
    edges.push_back(Edge(to, from, 0, 0));
    int mm = edges.size();
    G[from].push_back(mm - 2);
    G[to].push_back(mm - 1);
}

void bfs() {
    memset(vis, 0, sizeof(vis));
    queue<int> Q;
    Q.push(t);
    d[t] = 0; vis[t] = 1;
    while (!Q.empty()) {
        int x = Q.front(); Q.pop();
        for (int i = 0; i < G[x].size(); ++i) {
            Edge& e = edges[G[x][i]];
            if (!vis[e.from] && e.cap > e.flow) {
                vis[e.from] = 1;
                d[e.from] = d[x] + 1;
                Q.push(e.from);
            }
        }
    }
}

int Augment() {
    int x = t, a = inf;
    while (x != s) {
        Edge& e = edges[p[x]];
        a = min(a, e.cap - e.flow);
        x = edges[p[x]].from;
    }
    x = t;
    while (x != s) {
        edges[p[x]].flow += a;
        edges[p[x] ^ 1].flow -= a;
        x = edges[p[x]].from;
    }
    return a;
}

int ISAP(int s, int t) {
    int flow = 0;
    bfs();
    memset(num, 0, sizeof(num));
    for (int i = 0; i < n; ++i) num[d[i]]++;
    int x = s;
    memset(cur, 0, sizeof(cur));
    while (d[s] < n) {
        if (x == t) {
            flow += Augment();
            x = s;
        }
        int ok = 0;
        for (int i = cur[x]; i < G[x].size(); ++i) {
            Edge& e = edges[G[x][i]];
            if (e.cap > e.flow && d[x] == d[e.to] + 1) {
                ok = 1;
                p[e.to] = G[x][i];
                cur[x] = i;
                x = e.to;
                break;
            }
        }
        if (!ok) {
            int mm = n - 1;
            for (int i = 0; i < G[x].size(); ++i) {
                Edge& e = edges[G[x][i]];
                if (e.cap > e.flow) mm = min(mm, d[e.to]);
            }
            if (--num[d[x]] == 0) break;
            num[d[x] = mm + 1]++;
            cur[x] = 0;
            if (x != s) x = edges[p[x]].from;
        }
    }
    return flow;
}
```

## 最小费用最大流
### Bellman-Ford算法
```cpp
const int maxn = 10010;
const int inf  = 0x3f3f3f3f;

int n, m, s, t, ansflow;
int vis[maxn], d[maxn], p[maxn], a[maxn];
long long anscost;

struct Edge {
    int from, to, cap, flow, cost;
    Edge(int u, int v, int c, int f, int w): from(u), to(v), cap(c), flow(f), cost(w){}
};
vector<Edge> edges;
vector<int> G[maxn];
void add(int u, int v, int c, int w) {
    edges.push_back(Edge(u, v, c, 0, w));
    edges.push_back(Edge(v, u, 0, 0,-w));
    int mm = edges.size();
    G[u].push_back(mm - 2);
    G[v].push_back(mm - 1);
}

bool BellmanFord(int& flow, long long& cost) {
    for (int i = 1; i <= n; ++i) d[i] = inf;
    memset(vis, 0, sizeof(vis));
    d[s] = 0; vis[s] = 1; p[s] = 0; a[s] = inf;
    queue<int> Q;
    Q.push(s);
    while (!Q.empty()) {
        int x = Q.front(); Q.pop();
        vis[x] = 0;
        for (int i = 0; i < G[x].size(); ++i) {
            Edge& e = edges[G[x][i]];
            if (e.cap > e.flow && d[e.to] > d[x] + e.cost) {
                d[e.to] = d[x] + e.cost;
                p[e.to] = G[x][i];
                a[e.to] = min(a[x], e.cap - e.flow);
                if (!vis[e.to]) {
                    Q.push(e.to);
                    vis[e.to] = 1;
                }
            }
        }
    }
    if (d[t] == inf) return false;
    flow += a[t];
    cost += (long long)d[t] * (long long)a[t];
    for (int u = t; u != s; u = edges[p[u]].from) {
        edges[p[u]].flow += a[t];
        edges[p[u] ^ 1].flow -= a[t];
    }
    return true;
}

int MinCostMaxFlow(long long& cost) {
    int flow = 0; cost = 0;
    while (BellmanFord(flow, cost));
    return flow;
}
```