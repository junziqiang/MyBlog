---
title: trie
math: true
date: 2023/1/19 20:46:25
categories:
- [AcWing,数据结构]
tags: [trie, 算法基础课]
---

# trie字符串统计
[题目链接](https://www.acwing.com/problem/content/837/)

维护一个字符串集合，支持两种操作：
`I x` 向集合中插入一个字符串 x；
`Q x` 询问一个字符串在集合中出现了多少次。
共有 N个操作，所有输入的字符串总长度不超过$10^5$，字符串仅包含小写英文字母。

**输入格式**
第一行包含整数 N，表示操作数。

接下来 N 行，每行包含一个操作指令，指令为 I x 或 Q x 中的一种。

**输出格式**
对于每个询问指令 Q x，都要输出一个整数作为结果，表示 x 在集合中出现的次数。
每个结果占一行。

**数据范围**
$1≤N≤2∗10^4$
**输入样例：**
>5
I abc
Q abc
Q ab
I ab
Q ab

**输出样例：**
>1
0
1

```cpp
#include<bits/stdc++.h>
using namespace std;
// 0号点既是根节点，又是空节点
// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量
const int N = 1e5 + 10;

int son[N][26], cnt[N], idx;

void insert(const string& str)
{
    int p = 0; // 根节点
    for (int i = 0; i < str.size(); i++) {
        int u = str[i] - 'a';
        // 如果没有节点，那么就创建出来
        if (son[p][u] == 0) son[p][u] = ++idx;
        // 指向下一个节点
        p = son[p][u];
    }
    cnt[p]++;
}
int query(const string& str)
{
    int p = 0;
    for (int i = 0; i < str.size(); i++) {
        int u = str[i] - 'a';
        if (son[p][u] == 0) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

int main()
{
    int n;
    cin >> n;
    while (n--) {
        string op, str;
        cin >> op >> str;
        if (op == "I") insert(str);
        else cout << query(str) << endl;
    }
    return 0;
}
```

# 最大异或对
[题目链接](https://www.acwing.com/problem/content/145/)
在给定的 N 个整数 $A_1，A_2……A_N$中选出两个进行 xor（异或）运算，得到的结果最大是多少？

**输入格式**
第一行输入一个整数 N。
第二行输入 N 个整数 $A_1～A_N$。

**输出格式**
输出一个整数表示答案。

**数据范围**
$1≤N≤10^5,0≤Ai<2^31$
**输入样例：**
>3
1 2 3

**输出样例：**
>3

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10, M = 31 * 1e5 + 10;

int idx, son[M][2], num[N];

// 将数字x按照位数插进去
void insert(int x)
{
    int p = 0;
    // 从最高位开始插入
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if(!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
}
// 查找与x异或最大的值
int search(int x)
{
    int res = 0, p = 0;
    // 大于等于0的条件可以简写成~i
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        // 如果能找到与x对应位数不同的值
        if (son[p][!u]) {
            // 对应位数异或之后就是1
            res += 1 << i;
            p = son[p][!u];
        } else p = son[p][u];
    }
    return res;
}
int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> num[i];
        insert(num[i]);
    }
    int res = 0;
    for (int i = 0; i < n; i++) res = max(res, search(num[i]));
    cout << res << endl;
    return 0;
}
```