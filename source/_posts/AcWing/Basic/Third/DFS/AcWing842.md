---
title: DFS842
date: 2022/12/17 20:46:25
categories:
- [AcWing,基础课]
tags:
---
```C++
/**
 * @file AcWing842.cpp
 * @author your name (you@domain.com)
 * @brief 
 * @version 0.1
 * @date 2022-11-16
 * 
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