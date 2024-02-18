---
title: 优化输入输出
date: 2023/1/15 12:00:00
categories:
- [AcWing]
tags: [编码技巧]
---
# 测试结果
[题目链接](https://www.acwing.com/problem/content/description/798/)
**数据范围**
1≤n,m≤1000,
1≤q≤200000,
1≤x1≤x2≤n,
1≤y1≤y2≤m,
−1000≤矩阵内元素的值≤1000
![测试结果](/myPic/测试截图.png)
# 原因1
> 因为在默认情况下，cin与stdin总是保持同步的，也就是说这两种方法可以混用，而不必担心文件指针混乱，同时cout和stdout也一样，两者混用不会输出顺序错乱。由于这个特性，所以导致cin和cout有许多额外的开销。

# 解决方法
std::ios::sync_with_stdio(false)
这个函数是一个“是否兼容 stdio”的开关，C++ 为了兼容 C，保证程序在使用了 printf 和 std::cout 的时候不发生混乱，将输出流绑到了一起。
这其实是 C++ 为了兼容而采取的保守措施。我们可以在进行 IO 操作之前将 stdio 解除绑定，但是在这样做之后要注意不能同时使用 std::cin/std::cout 和 scanf/printf。更严格的来说：关闭之后C++ IO和C IO 两者不能混用，否则会造成IO混乱。

# 原因2
还有一种影响速度的原因是：在默认的情况下std::cin 绑定的是 std::cout ，每次执行 << 操作符的时候都要调用 flush()，这样会增加 IO 负担。

# 解决方法
ie 是将两个 stream 绑定的函数，空参数的话返回当前的输出流指针。 可以通过std::cin.tie(0) 和std:cout.tie(0)（0 表示 NULL）来解除 std::cin与 std::cout的绑定，进一步加快执行效率。

在这两种情况优化下，std::cin和std::cout的速度就和scanf和printf基本一样了，甚至更快。

```cpp
std::ios::sync_with_stdio(false);
std::cin.tie(nullptr);
std::cout.tie(nullptr);
```
