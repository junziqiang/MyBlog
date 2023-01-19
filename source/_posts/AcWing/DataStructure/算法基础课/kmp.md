---
title: kmp
math: true
date: 2023/1/19 20:46:25
categories:
- [AcWing,数据结构]
tags: [kmp, 算法基础课]
---

# KMP
## KMP字符串
[题目链接](https://www.acwing.com/problem/content/833/)
给定一个字符串 S，以及一个模式串 P，所有字符串中只包含大小写英文字母以及阿拉伯数字。

模式串P在字符串S中多次作为子串出现。

求出模式串P在字符串S中所有出现的位置的起始下标。

**输入格式**
第一行输入整数 N，表示字符串 P的长度。
第二行输入字符串 P。
第三行输入整数 M，表示字符串 S的长度。
第四行输入字符串 S。

**输出格式**
共一行，输出所有出现位置的起始下标（下标从 0开始计数），整数之间用空格隔开。

**数据范围**
1≤N≤105
1≤M≤106
**输入样例：**
>3
aba
5
ababa

**输出样例：**
>0 2

```cpp

#include<bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;
const int M = 1e6 + 10;

int n, m;
char s[M], p[N];
int ne[N];

int main()
{
    // 以数组名输入时，系统会自动添加结束符，以字符串的方式存储数组；
    // 以数组名输出时，遇到结束’\0’,输出才结束；
    // 下标从1开始
    cin >> n >> p + 1 >> m >> s + 1;
    // 求next数组
    // i 可以从2开始，因为第一个匹配失败的话就是要回退到0，已经初始化了
    for (int i = 2, j = 0; i <= n; i++) {
        while (j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j++;
        ne[i] = j;
    }
    for (int i = 0; i <= n; i++) {
        cout << ne[i] << " ";
    }
    cout << endl;
    // 先写匹配的代码
    for (int i = 1, j = 0; i <= m; i++) {
        while (j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j++;
        if (j == n) {
            cout << i - n << " ";
            // 匹配成功之后，回退j，也就是next[j]
            j = ne[j];
        }
    }
    return 0;
}

```