---
title: BFS847
date: 2022/12/17 20:46:25
categories:
- [AcWing,基础课]
tags:
---
```C++
/**
 * @file AcWing847.cpp
 * @author your name (you@domain.com)
 * @brief 
 * @version 0.1
 * @date 2022-11-17
 * 
给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环。

所有边的长度都是 1，点的编号为 1∼n。

请你求出 1 号点到 n 号点的最短距离，如果从 1 号点无法走到 n 号点，输出 −1。

输入格式
第一行包含两个整数 n 和 m。

接下来 m 行，每行包含两个整数 a 和 b，表示存在一条从 a 走到 b 的长度为 1 的边。

输出格式
输出一个整数，表示 1 号点到 n 号点的最短距离。

数据范围
1≤n,m≤105
输入样例：
4 5
1 2
2 3
3 4
1 3
1 4
输出样例：
1

 * @copyright Copyright (c) 2022
 * 
 */

#include<bits/stdc++.h>
using namespace std;

int n, m;
const int N = 1e5 + 10;
unordered_map<int, set<int>> g;
int dis[N];

void bfs()
{
    queue<int> q;
    dis[1] = 0;
    q.push(1);
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        for (const auto& item : g[t]) {
            if (dis[item] == -1) {
                q.push(item);
                dis[item] = dis[t] + 1;
            }
        }
    }
    if (dis[n] == -1) cout << "-1" <<endl;
    else cout << dis[n] <<endl;
}


int main()
{
    cin >> n >> m;
    memset(dis, -1, sizeof dis);
    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        g[a].insert(b);
    }
    bfs();
    return 0;
}
```