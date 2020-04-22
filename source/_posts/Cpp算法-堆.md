---
title: Cpp算法-堆
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: bb233fbc
date: 2019-01-10 13:13:20
---
#### 说明
`heap[]`堆<br/>
`heap_size`堆大小<br/>
`put(int)`压入一个数<br/>
`get()`弹出堆顶

#### 普通实现
```cpp
int heap[maxn];
int heap_size = 0;

void put(int d)
{
    int now, next;
    heap[++heap_size] = d;
    now = heap_size;
    while (now > 1)
    {
        next = now >> 1;
        if (heap[now] <= heap[next]) break;
        swap(heap[now], heap[next]);
        now = next;
    }
    return;
}

int get()
{
    int now, next, res;
    res = heap[1];
    heap[1] = heap[heap_size--];
    now = 1;
    while (now * 2 <= heap_size)
    {
        next = now * 2;
        if (next < heap_size && heap[next + 1] < heap[next]) next++;
        if (heap[now] <= heap[next]) break;
        swap(heap[now], heap[next]);
        now = next;
    }
    return res;
}
```

#### STL实现
```cpp
int heap[maxn];
int heap_size = 0;

void put(int d)
{
    heap[++heap_size] = d;
    push_heap(heap + 1, heap + heap_size + 1);
    //push_heap(heap + 1, heap + heap_size + 1, greater<int>()); 
    return;
}

int get()
{
    pop_heap(heap + 1, heap + heap_size + 1);                   
    //pop_heap(heap + 1, heap + heap_size + 1, greater<int>()); 
    return heap[heap_size--];
}
```