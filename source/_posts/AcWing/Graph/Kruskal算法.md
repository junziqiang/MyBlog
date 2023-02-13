---
title: kruskal算法
math: true
date: 2023/2/13 14:43:25
categories:
- [AcWing,图论]
tags: [kruskal算法, 算法基础课]
---
# 算法
1. 所有边按照权重从小到大排序
2. 枚举每条边， 如果a,b不连通（并查集），将这条边加入集合中
# kruskal算法求最小生成树
[题目链接](https://www.acwing.com/problem/content/861/)
给定一个 n 个点 m 条边的无向图，图中可能存在重边和自环，边权可能为负数。

求最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

给定一张边带权的无向图 G=(V,E)，其中 V 表示图中点的集合，E 表示图中边的集合，n=|V|，m=|E|。
由 V中的全部 n 个顶点和 E 中 n−1 条边构成的无向连通子图被称为 G 的一棵生成树，其中边的权值之和最小的生成树被称为无向图 G 的最小生成树。

**输入格式**
第一行包含两个整数 n 和 m。

接下来 m 行，每行包含三个整数 u,v,w，表示点 u 和点 v 之间存在一条权值为 w 的边。

**输出格式**
共一行，若存在最小生成树，则输出一个整数，表示最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

**数据范围**
$1≤n≤10^5$

$1≤m≤2∗10^5$

图中涉及边的边权的绝对值均不超过 1000。

**输入样例：**
>4 5
1 2 1
1 3 2
1 4 3
2 3 2
3 4 4

**输出样例：**
>6

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10, M = 2 * N;
int n, m;
int p[N];

int find(int x)
{
    if (x != p[x]) p[x] = find(p[x]);
    return p[x];
}


struct Edge{
    int a, b, w;
    bool operator< (const Edge& lhs) {
        return w < lhs.w;
    }
}edges[M];

void kruskal()
{
    // 1.排序 
    sort(edges, edges + m);
    for (int i = 1; i <= n; i++) p[i] = i;
    int res = 0, cnt = 0;
    // 2. 遍历所有边
    for (int i = 0; i < m; i++) {
        int a = edges[i].a, b = edges[i].b, w = edges[i].w;
        a = find(a);
        b = find(b);
        if (a != b) {
            // 3. 合并集合
            p[a] = b;
            res += w;
            cnt++;
            
        }
    }
    if (cnt < n - 1) cout << "impossible" << endl;
    else cout << res << endl;
}
int main()
{
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int a, b, w;
        cin >> a >> b >> w;
        edges[i] = {a, b, w};
    }
    kruskal();
    return 0;
}
```