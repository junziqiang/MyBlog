---
title: DFS
date: 2022/12/17 20:46:25
categories:
- [AcWing,搜索]
tags:
---
## 排列数字
给定一个整数 n，将数字 1∼n 排成一排，将会有很多种排列方法。

现在，请你按照字典序将所有的排列方法输出。

输入格式
共一行，包含一个整数 n。

输出格式
按字典序输出所有排列方案，每个方案占一行。

数据范围
1≤n≤7
输入样例：
3
输出样例：
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```cpp
/**
 * @file AcWing842.cpp
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

int n;
const int N = 10;
vector<int> path;
bool st[N];
// 遍历第u个数应该填写什么
void dfs(int u)
{
    // 搜索结束
    if (u == n) {
        for (const auto& item : path) {
            cout << item << " ";
        }
        cout << endl;
        return;
    }
    // 枚举可以填入的数据
    for (int i = 1; i <= n; i++) {
        if (!st[i]) {
            st[i] = true;
            path.push_back(i);
            dfs(u + 1);
            path.pop_back();
            st[i] = false;
        }
    }

}

int main()
{
    cin >> n;
    dfs(0);
    return 0;
}
```
## n皇后问题
n− 皇后问题是指将 n 个皇后放在 n×n 的国际象棋棋盘上，使得皇后不能相互攻击到，即任意两个皇后都不能处于同一行、同一列或同一斜线上。

现在给定整数 n，请你输出所有的满足条件的棋子摆法。

输入格式
共一行，包含整数 n。

输出格式
每个解决方案占 n 行，每行输出一个长度为 n 的字符串，用来表示完整的棋盘状态。

其中 . 表示某一个位置的方格状态为空，Q 表示某一个位置的方格上摆着皇后。

每个方案输出完成后，输出一个空行。

注意：行末不能有多余空格。

输出方案的顺序任意，只要不重复且没有遗漏即可。

数据范围
1≤n≤9
输入样例：
4
输出样例：
.Q..
...Q
Q...
..Q.

..Q.
Q...
...Q
.Q..
```cpp
/**
 * @file AcWing843.cpp
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

int n;
// 对角线个数为2倍
const int N = 20;
char g[N][N];
bool col[N], dg[N], udg[N];
void dfs(int u) {
    if (u == n) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                cout << g[i][j];
            }
            cout << endl;
        }
        cout << endl;
    }
    // 枚举皇后能放在第几列
    for (int i = 0; i < n; i++) {
        // 对角线的方程根据坐标系来定
        if (!col[i] && !dg[u - i + n] && !udg[u + i]) {
            col[i] = dg[u - i + n] = udg[u + i] = true;
            g[u][i] = 'Q';
            dfs(u + 1);
            g[u][i] = '.';
            col[i] = dg[u - i + n] = udg[u + i] = false;
        }
    }
}
int main()
{
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            g[i][j] = '.';
        }
    }
    dfs(0);
    return 0;
}
```
## 树的重心
给定一颗树，树中包含 n 个结点（编号 1∼n）和 n−1 条无向边。

请你找到树的重心，并输出将重心删除后，剩余各个连通块中点数的最大值。

重心定义：重心是指树中的一个结点，如果将这个点删除后，剩余各个连通块中点数的最大值最小，那么这个节点被称为树的重心。

输入格式
第一行包含整数 n，表示树的结点数。

接下来 n−1 行，每行包含两个整数 a 和 b，表示点 a 和点 b 之间存在一条边。

输出格式
输出一个整数 m，表示将重心删除后，剩余各个连通块中点数的最大值。

数据范围
1≤n≤105
输入样例
9
1 2
1 7
1 4
2 8
2 5
4 3
3 9
4 6
输出样例：
4
```cpp
/**
 * @file AcWing846.cpp
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
int n;
const int N = 1e5 + 10, M = 2 * N;
// 邻接表的头结点， 节点值， next
int h[N], e[M], ne[M], idx;

int ans = INT_MAX;
bool st[N];

void add(int a, int b)
{
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx++;
}
// 以u为根节点的树，节点总数量
int dfs(int u)
{
    st[u] = true;
    int sum = 1, res = 0;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (!st[j]) {
            int s = dfs(j);
            res = max(res, s);
            sum += s;
        }
    }
    // 抛去子树后的节点数量
    res = max(res, n - sum);
    ans = min(ans, res);
    return sum;
}
int main()
{
    cin >> n;
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i++) {
        int a, b;
        cin >> a >> b;
        add(a, b);
        add(b , a);
    }
    dfs(1);
    cout << ans <<endl;
}
```