---
title: Cpp算法-动态规划
toc: true
mathjax: true
tags:
  - Cpp
  - 算法
description: ' '
categories: C++算法
abbrlink: 8364d7e9
date: 2019-01-10 12:58:38
---
__*待完成*__

## 多阶段过程决策的最优化问题
```mermaid
graph LR
    A --5--> B1
    A --3--> B2
    B1 --1--> C1
    B1 --6--> C2
    B1 --3--> C3
    B2 --8--> C2
    B2 --4--> C4
    C1 --5--> D1
    C1 --6--> D2
    C2 --5--> D1
    C3 --8--> D3
    C4 --3--> D3
    D1 --3--> E
    D2 --4--> E
    D3 --3--> E
```

!!! tldr "题目及注解"
    求上图从 $A$ 到 $E$ 的最短距离<br/><br/>
    $K$: 阶段<br/>
    $D(X_I, (X+1)_J)$: 从 $X_I$ 到 $(X+1)_J$ 的距离<br/>
    $F_K(X_I)$: $K$ 阶段下 $X_I$ 到终点 $E$ 的最短距离

倒推:
$$
K=4\qquad F_4(D_1)=3\qquad F_4(D_2)=4\qquad F_4(D_3)=3
$$
$$
K=5\qquad F_3(C_1)=min(D(C_1,D_1)+F_4(D_1),D(C_1,D_2)+F_4(D_2))=min(5+3,6+4)=8
F_3(C_2)
$$
