---
title: spfa算法
math: true
date: 2023/2/05 13:23:25
categories:
- [AcWing,图论]
tags: [spfa算法, 算法基础课]
---
# 算法
spfa是对bellman-ford算法进行了优化
```cpp
// bellman-ford算法
for 循环n次
    for 所有边
        // 松弛操作， 这里如果dis[a]没有变化的话，那么修改也将没有任何意义，因此spfa在这里进行了优化
        dis[b] = min(dis[b], dis[a] + w);
        // 松弛操作之后 dis[b] <= dis[a] + w; 三角不等式
```
```cpp
spfa使用一个队列存储了所有变化的边
```
# spfa求最短路
[题目链接](https://www.acwing.com/problem/content/853/)

给定一个 n个点 m条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你求出 1号点到 n号点的最短距离，如果无法从 1号点走到 n号点，则输出 `impossible`。

数据保证不存在负权回路。

**输入格式**
第一行包含整数 n和 m。

接下来 m行每行包含三个整数 x,y,z，表示存在一条从点 x到点 y的有向边，边长为 z。

**输出格式**
输出一个整数，表示 1号点到 n号点的最短距离。

如果路径不存在，则输出 `impossible`。

**数据范围**
$1≤n,m≤10^5$
图中涉及边长绝对值均不超过 10000。

**输入样例：**
>3 3
1 2 5
2 3 -3
1 3 4

**输出样例：**
>2

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;

int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dis[N];
// 标记是否在队列中
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

void spfa()
{
    dis[1] = 0;
    queue<int> q;
    q.push(1);
    st[1] = true;
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i];
            if (dis[j] > dis[t] + w[i]) {
                dis[j] = dis[t] + w[i];
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
}
int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);
    memset(dis, 0x3f, sizeof dis);
    while (m--) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    spfa();
    if (dis[n] == 0x3f3f3f3f) cout << "impossible" << endl;
    else cout << dis[n] << endl;
    return 0;
}
```

# spfa判断负环
[题目链接](https://www.acwing.com/problem/content/854/)
给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你判断图中是否存在负权回路。

**输入格式**
>第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

**输出格式**
如果图中存在负权回路，则输出 Yes，否则输出 No。

**数据范围**
1≤n≤2000
1≤m≤10000
图中涉及边长绝对值均不超过 10000。

**输入样例：**
>3 3
1 2 -1
2 3 4
3 1 -4
**输出样例：**
>Yes

```cpp
#include<bits/stdc++.h>
using namespace std;

int n, m;
const int N = 2010, M = 10010;
int h[N], e[M], w[M], ne[M], idx;
int dis[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}
bool spfa()
{
    queue<int> q;
    // 将所有点插入队列
    for (int i = 1; i <= n; i++) {
        st[i] = true;
        q.push(i);
    }
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i];
            if (dis[j] > dis[t] + w[i]) {
                dis[j] = dis[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] > n) return true;
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return false;
}
int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);
    memset(dis, 0x3f, sizeof dis);
    while (m--) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    if (spfa()) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}

```