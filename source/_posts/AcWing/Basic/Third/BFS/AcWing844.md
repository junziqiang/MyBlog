---
title: BFS844
date: 2022/12/17 20:46:25
categories:
- [AcWing,基础课]
tags:
---
```cpp
/**
 * @file AcWing844.cpp
 * @author your name (you@domain.com)
 * @brief 
 * @version 0.1
 * @date 2022-11-16
 * 
 * @copyright Copyright (c) 2022
 * 
给定一个 n×m 的二维整数数组，用来表示一个迷宫，数组中只包含 0 或 1，其中 0 表示可以走的路，1 表示不可通过的墙壁。

最初，有一个人位于左上角 (1,1) 处，已知该人每次可以向上、下、左、右任意一个方向移动一个位置。

请问，该人从左上角移动至右下角 (n,m) 处，至少需要移动多少次。

数据保证 (1,1) 处和 (n,m) 处的数字为 0，且一定至少存在一条通路。

输入格式
第一行包含两个整数 n 和 m。

接下来 n 行，每行包含 m 个整数（0 或 1），表示完整的二维数组迷宫。

输出格式
输出一个整数，表示从左上角移动至右下角的最少移动次数。

数据范围
1≤n,m≤100
输入样例：
5 5
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
输出样例：
8
 */

#include<bits/stdc++.h>
using namespace std;

int n, m;
const int N = 110;
int g[N][N];
int dis[N][N];

void bfs()
{
    memset(dis, -1, sizeof dis);
    queue<pair<int, int>> q;
    int dx[] = {0, -1, 0, 1}, dy[] = {-1, 0, 1, 0};
    q.push({0, 0});
    dis[0][0] = 0;
    while(!q.empty()) {
        auto t = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nx = t.first + dx[i], ny = t.second + dy[i];
            if (nx >= 0 && nx < n && ny >= 0 && ny < m && g[nx][ny] == 0 && dis[nx][ny] == -1) {
                dis[nx][ny] = dis[t.first][t.second] + 1;
                q.push({nx, ny});
            }
        }
    }
}
int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> g[i][j];
        }
    }
    bfs();
    cout << dis[n - 1][m - 1];
}
```    

