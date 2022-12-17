---
title: topSort848
date: 2022/12/17 20:46:25
categories:
- [AcWing,基础课]
tags:
---
```C++
/**
 * @file AcWing848.cpp
 * @author your name (you@domain.com)
 * @brief 
 * @version 0.1
 * @date 2022-11-17
 * 
给定一个 n 个点 m 条边的有向图，点的编号是 1 到 n，图中可能存在重边和自环。

请输出任意一个该有向图的拓扑序列，如果拓扑序列不存在，则输出 −1。

若一个由图中所有点构成的序列 A 满足：对于图中的每条边 (x,y)，x 在 A 中都出现在 y 之前，则称 A 是该图的一个拓扑序列。

输入格式
第一行包含两个整数 n 和 m。

接下来 m 行，每行包含两个整数 x 和 y，表示存在一条从点 x 到点 y 的有向边 (x,y)。

输出格式
共一行，如果存在拓扑序列，则输出任意一个合法的拓扑序列即可。

否则输出 −1。

数据范围
1≤n,m≤105
输入样例：
3 3
1 2
2 3
1 3
输出样例：
1 2 3
 * @copyright Copyright (c) 2022
 * 
 */

#include<bits/stdc++.h>
using namespace std;

int n, m;
const int N = 100010;
unordered_map<int, set<int>> g;
vector<int> d(N);
vector<int> res;

void topSort()
{
    queue<int> q;
    for (int i = 1; i <= n; i++) {
        if (d[i] == 0) {
            q.push(i);
        }
    }
    while (!q.empty()) {
        int t = q.front();
        res.push_back(t);
        q.pop();
        for (const int item : g[t]) {
            if (--d[item] == 0) {
                q.push(item);
            }
        }
    }
    if (res.size() != n) cout << "-1" <<endl;
    else for (int item : res) cout << item << " ";
}

int main()
{
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        if (a == b) {
            cout << "-1"<<endl;
            return 0;
        }
        if (g[a].find(b) == g[a].end()) {
            g[a].insert(b);
            d[b]++;   
        }
    }
    topSort();
}
```