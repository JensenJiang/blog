---
title: Magical Balls
date: 2016-05-13T11:07:11+08:00
mathjax: true
<!-- type: post -->
tags:
- algorithm questions
---
# H.Magical Balls
[原题传送门](http://poj.openjudge.cn/practice/C16H/)
## 描述
Wenwen has a magical ball. When put on an infinite plane, it will keep duplicating itself forever.

Initially, Wenwen puts the ball on the location $(x\_0, y\_0)$ of the plane. Then the ball starts to duplicate itself right away. For every unit of time, each existing ball on the plane will duplicate itself, and the new balls will be put on the adjacent locations. The duplication rule of these balls is, during the i-th unit of time, a ball, which locates at $(x, y)$, will duplicate $u\_i$ balls to $(x, y+1)$, $d\_i$ balls to $(x, y-1)$, $l\_i$ balls to $(x-1, y)$ and $r\_i$ balls to $(x+1, y)$.

The duplication rule has a period of $M$. In another words, $u\_i=u\_i-M$, $d\_i=d\_i-M$, $l\_i=l\_i-M$, $r\_i=r\_i-M$, for $i=M+1,M+2$,...

Wenwen is very happy because she will get many balls. It is easy to calculate how many balls she will get after $N$ units of time. However, she wants to know the sum of x-coordinates and y-coordinates of all balls after $N$ units of time. This is a bit difficult for her. Could you help her? Since the sum might be very large, you should give the sum modulo 1,000,000,007 to her. 
## 输入格式
The first line contains an integer T (1 ≤ T ≤ 25), indicating the number of test cases.

For each test case:

The first line contains four integers N (1 ≤ N ≤ 10^18), M (1 ≤ M ≤ 20,000), x0 and y0 (-10^18 ≤ x0,y0 ≤ 10^18);

Then follows M lines, the i-th line contains four integers: ui, di, li and ri (0 ≤ ui,di,li,ri ≤ 10,000).
## 输出格式
For each test case, output one integer on a single line, indicating the sum of x-coordinates and y-coordinates of all balls after N units of time, modulo 1,000,000,007.
## 样例输入
```
1
2 2 1 1
2 0 0 0
0 0 0 1
```
## 样例输出
```
19
```

# 解法
记$S\_i$和$N\_i$分别为“经过$i$时间后所有小球x，y坐标的和”以及“经过$i$时间后所有小球的数量”。$S\_0 = x\_0 + y\_0 , N\_0 = 1$。不难得出：
$$\left\\{
\begin{aligned}
N\_i&=N\_{i-1}(u\_i+d\_i+l\_i+r\_i+1) \\\ 
S\_i&=(u\_i+d\_i+l\_i+r\_i+1)S\_{i-1}+N\_{i-1}(u\_i-d\_i+r\_i-l\_i)
\end{aligned}
\right.$$
记$a\_i = u\_i+d\_i+l\_i+r\_i+1 , b\_i = u\_i-d\_i+r\_i-l\_i$，则：
$$\left\\{
\begin{aligned}
N\_i&=N\_{i-1}a\_i \\\ 
S\_i&=a\_iS\_{i-1}+N\_{i-1}b\_i
\end{aligned}
\right.$$
由归纳法可得：
$$\left\\{
\begin{aligned}
N\_k&=\prod\limits\_{i=1}^ka\_k \\\ 
S\_k&=S\_0N\_k + \sum\limits\_{i=1}^k\frac{N\_kb\_i}{a\_i}
\end{aligned}
\right.$$
由于存在循环结，$N\_k$的值可以通过先求出前$M$个的积再快速幂求出，$\sum\limits\_{i=1}^k\dfrac{N\_kb\_i}{a\_i}$的值则可以先求出前M个的和再做乘法得到。其中$\dfrac{N\_k}{a\_i}$要转变成$N\_k$乘以$a\_i$的逆来实现。

# 吐槽
比赛的时候队友很快就把递推式算了出来，我把式子化简后看着能用快速幂求就试着写了，写的过程也挺顺利的，结果一直WA到最后也没调出来。最后比赛出来后又检查了很久才发现有一个地方long long * long long溢出了……(我的锅TUT
