---
title: Cpp算法-STL标准库
mathjax: true
toc: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: ac108281
date: 2019-01-10 13:15:15
---
## 模板
---
```cpp
template <typename T>
/**
 * 写函数/结构体
 */
```

### 例
```cpp
template <typename T>
struct Point
{
    T x, y;
    Point(T x = 0, T y = 0):x(x), y(y) {}
};

template <typename T>
Point<T> operator + (const Point<T>& A, const Point<T>& B)
{
    return Point<T>(A.x + B.x, A.y + B.y);
}

template <typename T>
ostream& operator << (ostream& out, const Point<T>& p)
{
    out << "(" << p.x << "," << p.y << ")";
    return out;
}
```

## vector(不定长数组)
---
### 声明
`vector<数据类型> 名;` 例 `vector<int> a;`
### 简单用法
`a.size();`读取大小<br/>
`a.resize();`改变大小<br/>
`a.push_back(x);`尾部添加元素x<br/>
`a.pop_back();`删除最后一个元素<br/>
`a.clear();`清空<br/>
`a.empty()`询问是否为空(bool类型)<br/>
`a[]`访问元素(可修改)<br/>

## priority_queue(优先队列/堆)
---
### 声明
__头文件__: `#include <queue>`<br/>
__参数__: `priority_queue<Type, Container, Functional>`<br/>
&emsp;`Type`数据类型 *不可省*<br/>
&emsp;`Container`容器(vector,deque)默认vector<br/>
&emsp;`Functional`比较方式,默认`operator <`大根堆

### 使用
与queue类似<br/>
#### 小根堆
`priority_queue<int, vector<int>, greater<int> > q;`使用仿函数`greater<>`<br/>
#### 自定义类型(struct)
```cpp
struct Node
{
    int x, y;
    Node(int a = 0, int b = 0):x(a), y(b){}
};
```
##### 重载operator <
```cpp
bool operator < (Node a, Node b)
{
    if (a.x == b.x) return a.y > b.y;
    return a.x > b.x;
}

priority_queue<Node> q;
```
x值大的优先级低,排在队前<br/>
x相等,y大的优先级低
##### 重写仿函数
```cpp
struct cmp
{
    bool operator () (Node a, Node b)
    {
        if (a.x == b.x) return a.y > b.y;
        return a.x > b.x;
    }
}

priority_queue<Node, vector<Node>, cmp> q;
```