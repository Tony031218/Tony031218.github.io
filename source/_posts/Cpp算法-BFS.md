---
title: Cpp算法-BFS
mathjax: true
toc: true
date: 2019-01-10 12:55:14
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
## 说明
本文实现只是框架，应当灵活运用，bfs()函数内部根据情况灵活更改
广搜算法基于树、队列实现，具体思路: `将当前点的子节点入队，当前点出队，如果子节点满足条件则记录`并重复此过程

## 框架
### 数组模拟队列
```cpp
void bfs()
{
    int head = 1, tail = 2;
    vis[start_x][start_y] = true;            //标记起始点
    que[head][0] = start_x; 
    que[head][1] = start_y;                  //起始点入队
    while(head < tail)                       //队不为空
    {
        int x = que[head][0], y = que[head][1]  //获取队首点
        for (int i = 0; i < 子节点数; ++i)
        {
            int x2 = x子节点, y2 = y子节点;
            if (x2, y2满足条件 && !vis[x2][y2])
            {
                记录结果;
                vis[x2][y2] = true;
                que[tail][0] = x2;
                que[tail][0] = y2;
                tail++;                         //入队
            }
        }
        head++;                                 //队首出队
    }
    return
}
```
### STL-queue
```cpp
struct Node
{
    int x, y;
}node, top;

queue<Node> que;

void bfs()
{
    vis[sx][sy] = true;
    node.x = sx;
    node.y = sy;
    que.push(node);
    ans[sx][sy] = 0;
    while(!que.empty())
    {
        top = que.front();
        for (int i = 0; i < 8; ++i)
        {
            int x2 = ..., y2 = ...;
            if (x2, y2满足条件 && !vis[x2][y2])
            {
                记录结果;
                vis[x2][y2] = true;
                node.x = x2;
                node.y = y2;
                que.push(node);
            }
        }
        que.pop();
    }
    return;
}
```

## 例
[洛谷P1443](https://www.luogu.org/problemnew/show/P1443)
```cpp
#include<bits/stdc++.h>
using namespace std;

int n, m, sx, sy, vis[210][210], ans[210][210];
int gox[8] = {-2, -1, 1, 2, 2, 1, -1, -2};
int goy[8] = {1, 2, 2, 1, -1, -2, -2, -1};

struct horse
{
    int x, y;
}node, top;

queue<horse> que;

void bfs()
{
    vis[sx][sy] = 1;
    node.x = sx;
    node.y = sy;
    que.push(node);
    ans[sx][sy] = 0;
    while(!que.empty())
    {
        top = que.front();
        for (int i = 0; i < 8; ++i)
        {
            int x2 = top.x + gox[i];
            int y2 = top.y + goy[i];
            if (x2 >= 1 && x2 <= n && y2 >= 1 && y2 <= m && !vis[x2][y2])
            {
                ans[x2][y2] = ans[top.x][top.y] + 1;
                vis[x2][y2] = 1;
                node.x = x2;
                node.y = y2;
                que.push(node);
            }
        }
        que.pop();
    }
    return;
}

int main()
{
    scanf("%d %d %d %d", &n, &m, &sx, &sy);
    memset(ans, -1, sizeof(ans));
    bfs();
    for (int i = 1; i <= n; ++i)
    {
        for (int j = 1; j <= m; ++j)
            printf("%-5d", ans[i][j]);
        printf("\n");
    }
    return 0;
}
```

```cpp
#include<bits/stdc++.h>
using namespace std;

int n, m, sx, sy, vis[210][210], que[50000][2], ans[210][210];
int gox[8] = {-2, -1, 1, 2, 2, 1, -1, -2};
int goy[8] = {1, 2, 2, 1, -1, -2, -2, -1};

void bfs()
{
    int head = 1, tail = 2;
    vis[sx][sy] = 1;
    que[head][0] = sx;
    que[head][1] = sy;
    ans[sx][sy] = 0;
    while (head < tail)
    {
        int x, x2, y, y2;
        x = que[head][0];
        y = que[head][1];
        for (int i = 0; i < 8; ++i)
        {
            x2 = x + gox[i];
            y2 = y + goy[i];
            if (x2 >= 1 && x2 <= n && y2 >= 1 && y2 <= m && !vis[x2][y2])
            {
                ans[x2][y2] = ans[x][y] + 1;
                vis[x2][y2] = 1;
                que[tail][0] = x2;
                que[tail][1] = y2;
                tail++;
            }
        }
        head++;
    }
    return;
}

int main()
{
    scanf("%d %d %d %d", &n, &m, &sx, &sy);
    memset(ans, -1, sizeof(ans));
    bfs();
    for (int i = 1; i <= n; ++i)
    {
        for (int j = 1; j <= m; ++j)
            printf("%-5d", ans[i][j]);
        printf("\n");
    }
    return 0;
}
```