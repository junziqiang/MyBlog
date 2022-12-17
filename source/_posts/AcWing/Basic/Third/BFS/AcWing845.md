---
title: BFS845
date: 2022/12/17 20:46:25
categories:
- [AcWing,基础课]
tags:
---
```C++
/**
 * @file AcWing845.cpp
 * @author your name (you@domain.com)
 * @brief 
 * @version 0.1
 * @date 2022-11-16
 * 
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