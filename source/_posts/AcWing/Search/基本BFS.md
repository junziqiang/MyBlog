---
title: 基本BFS
date: 2022/12/17 20:46:25
categories:
- [AcWing,搜索]
tags:
---
# 走迷宫
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
# 八数码
在一个 3×3 的网格中，1∼8 这 8 个数字和一个 x 恰好不重不漏地分布在这 3×3 的网格中。

例如：

1 2 3
x 4 6
7 5 8
在游戏过程中，可以把 x 与其上、下、左、右四个方向之一的数字交换（如果存在）。

我们的目的是通过交换，使得网格变为如下排列（称为正确排列）：

1 2 3
4 5 6
7 8 x
例如，示例中图形就可以通过让 x 先后与右、下、右三个方向的数字交换成功得到正确排列。

交换过程如下：

1 2 3   1 2 3   1 2 3   1 2 3
x 4 6   4 x 6   4 5 6   4 5 6
7 5 8   7 5 8   7 x 8   7 8 x
现在，给你一个初始网格，请你求出得到正确排列至少需要进行多少次交换。

输入格式
输入占一行，将 3×3 的初始网格描绘出来。

例如，如果初始网格如下所示：

1 2 3 
x 4 6 
7 5 8 
则输入为：1 2 3 x 4 6 7 5 8

输出格式
输出占一行，包含一个整数，表示最少交换次数。

如果不存在解决方案，则输出 −1。

输入样例：
2 3 4 1 5 x 7 6 8
输出样例
19
```cpp
/**
 * @file AcWing845.cpp
 * @author your name (you@domain.com)
 * @brief 
 * @version 0.1
 * @date 2022-11-16
 * 

 * @copyright Copyright (c) 2022
 * 
 */
#include<bits/stdc++.h>
using namespace std;

int bfs(string state)
{
    queue<string> q;
    unordered_map<string, int> d;
    q.push(state);
    d[state] = 0;
    int dx[] = {0, -1, 0, 1}, dy[] = {-1, 0, 1, 0};
    string end("12345678x");
    while(!q.empty()) {
        auto t = q.front();
        q.pop();
        if (t == end) return d[end];
        int curDis = d[t];
        int index = t.find('x');
        int row = index / 3, col = index % 3;
        for (int i = 0; i < 4; i++) {
            int nx = row + dx[i], ny = col + dy[i];
            if (nx >= 0 && nx < 3 && ny >= 0 && ny < 3) {
                swap(t[nx * 3 + ny], t[index]);
                if (d.find(t) == d.end()) {
                    d[t] = curDis + 1;
                    q.push(t);
                }
                swap(t[nx * 3 + ny], t[index]);
            }
        }
    }
    return -1;
}
int main()
{
    char c;
    std::string state;
    for (int i = 0; i < 9; i++) {
        cin >> c;
        state += c;

    }
    cout << bfs(state) <<endl;
    return 0;
}
```
# 图中点的层次
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
```cpp
/**
 * @file AcWing847.cpp
 * @author your name (you@domain.com)
 * @brief 
 * @version 0.1
 * @date 2022-11-17
 * 


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
