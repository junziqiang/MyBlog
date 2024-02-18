---
title: bellman-ford算法
math: true
date: 2023/2/05 12:43:25
categories:
- [AcWing,图论]
tags: [bellman-ford算法, 算法基础课]
---
# 算法
```cpp
for 循环n次
    for 所有边
        // 松弛操作
        dis[b] = min(dis[b], dis[a] + w);
        // 松弛操作之后 dis[b] <= dis[a] + w; 三角不等式
```
bellman-ford算法可以处理右负权边的图，但是有负权回路则路径可能不存在

# 有边数限制的最短路
[题目链接](https://www.acwing.com/problem/content/855/)
给定一个 n个点 m条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你求出从 1号点到 n号点的最多经过 k条边的最短距离，如果无法从 1号点走到 n 号点，输出 `impossible`。

**注意：图中可能存在负权回路**。

**输入格式**
第一行包含三个整数 n,m,k。

接下来 m行，每行包含三个整数 x,y,z，表示存在一条从点 x到点 y的有向边，边长为 z。

点的编号为 1∼n。

**输出格式**
输出一个整数，表示从 1号点到 n号点的最多经过 k条边的最短距离。

如果不存在满足条件的路径，则输出 `impossible`。

**数据范围**
1≤n,k≤500
1≤m≤10000
1≤x,y≤n
任意边长的绝对值不超过 10000。

**输入样例：**
>3 3 1
1 2 1
2 3 1
1 3 3

**输出样例：**
>3

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 510, M = 10010;
struct Edge{
    int a, b, w;
}edges[M];
int n, m, k;

int dis[N], backup[N];
void bellman_ford()
{
    dis[1] = 0;
    for (int i = 0; i < k; i++) {
        // 备份先前的距离数组, 如果不备份，下面的循环会发生串联更改的情况
        memcpy(backup, dis, sizeof dis);
        for (int j = 0; j < m; j++) {
            auto e = edges[j];
            dis[e.b] = min(dis[e.b], backup[e.a] + e.w);
        }
    }
}
int main()
{
    cin >> n >> m >> k;
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        edges[i] = {a, b, c};
    }
    memset(dis, 0x3f, sizeof dis);
    bellman_ford();
    if (dis[n] > 0x3f3f3f3f / 2) cout << "impossible" << endl;
    else cout << dis[n] << endl;
    return 0;
}
```