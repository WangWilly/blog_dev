title: Algorithm - Recursive time function
author: Willy Wang (willywangkaa)
tags:
  - Function
  - Time complexity
  - Recursive
categories:
  - Data structure
date: 2018-10-21 13:12:00
---
# Substution method



- Example 1


$$
T(n) = T(\sqrt n) + 1 \\
\Rightarrow T(n^{\frac 14}) + 1 + 1 = T(n^{\frac 14}) + 2 \\
\Rightarrow T(n^{\frac 18}) + 3 \\
\vdots \\
\Rightarrow T(2) + \lg (\lg n) = 1 + \lg \lg n \\
\therefore T(n) = O(\lg \lg n)
$$

> $$
> n^{\frac 1{k} } = 2 \\
> \Rightarrow \frac 1k \cdot \lg n = 1 \\
> \Rightarrow k = \lg n
> $$
>



- Example 2


$$
T(n) = 2\cdot T(\sqrt n) + \log n, T(2) = 1 \\
\Rightarrow 2\cdot(2\cdot T(2^{\frac 14}) + \log \sqrt n) + \log n \\
\Rightarrow 4\cdot T(n^\frac 14) + 2\log n \\
\vdots \\
\Rightarrow \lg n \cdot T(2) + (\lg \lg n )\cdot \log n \\
\Rightarrow \lg n + \lg \lg n \cdot \lg n \\
\therefore T(n) = O(\lg \lg n \cdot \lg n)
$$


- Example 3 (無法使用「**Master theory**」)


$$
T(n) = 2\cdot T(\frac n2) + n \log n, T(1) = 1 \\
\Rightarrow 2\cdot (2 \cdot T(\frac n4) + \frac n2\log \frac n2) + n\log n \\
\Rightarrow 4\cdot T(\frac n4) + n(\log n - 1) + n(\log n) \\
\vdots \\
\Rightarrow n \cdot T(1) + n(\lg n - (\lg n-1)) +\ldots+n(\log n - 1) + n(\log n)\\
\Rightarrow n + n(1 + 2 + \ldots +\log n - 1+\log n) \\
\Rightarrow n + n\frac {(1+\lg n)\lg n}{2} \\
\Rightarrow n + \frac {n\lg n + n(\lg n)^2}{2} \\
\therefore T(n) = O(n\lg^2 2)
$$

> $$
> \frac n {2^k} = 1 \\
> \Rightarrow k = \lg n
> $$
>



- Example 4 (無法使用「**Master theory**」)


$$
T(n) = 2\cdot T(\frac n2) + \frac {n}{\log n}, T(1) = 1 \\
\Rightarrow 2\cdot (2\cdot T(\frac n4)+\frac{\frac n2}{\log \frac n2}) + \frac {n}{\log n} \\
\Rightarrow 4\cdot T(\frac n4) + \frac {n}{\log n - 1} + \frac {n}{\log n} \\
\vdots \\
\Rightarrow  n\cdot T(1) + \frac {n}{\log n - (\log n -1)} + \cdots+\frac {n}{\log n - 1} + \frac {n}{\log n} \\
\Rightarrow  n+ n(\frac {1}{1} + \cdots+\frac {1}{\log n - 1} + \frac {1}{\log n}) \\
\Rightarrow  n+ n\log\log n \quad （調和級數）\\
\therefore T(n) = O(n\log\log n)
$$

- Example 5 (無法使用「**Master theory**」)

$$
T(n) = 8\cdot T(\frac n2) + n^3 \cdot \log^2 n \\
\Rightarrow 8\cdot(8 \cdot T(\frac n4) + (\frac n2)^3\cdot \log^2(\frac n2))+n^3 \cdot \log^2 n\\
= 64 \cdot T(\frac n4) + n^3 \cdot \log ^2 (\frac n2)+n^3 \cdot \log^2 n \\
\Rightarrow 512\cdot T(\frac n8) + n^3 \cdot \log ^2 (\frac n4) + n^3 \cdot \log ^2 (\frac n2)+n^3 \cdot \log^2 n\\
= 512\cdot T(\frac n8) + n^3 \cdot (\log ^2 (\frac n4) + \log ^2 (\frac n2)+ \log ^2 n) \\
 = 512\cdot T(\frac n8) + n^3 \cdot ((\log n - 2)^2 + (\log n - 1)^2+ (\log n)^2) \\
 \vdots \\
 \Rightarrow n \cdot T(1) + n^3 \cdot ((1)^2 + \ldots + (\log n - 1)^2+ (\log n)^2) \\
 \Rightarrow n + n^3 \cdot (\log n)^3 \\
 \therefore T(n) = O(n^3\log^3 n)
$$



# Recursive tree



主要見 [Algorithm-Recurrence](https://wangwilly.github.io/willywangkaa/2018/09/01/Algorithm-Recurrence/)



- **＜近似＞**


$$
T(n) = T(\lceil\frac n2 \rceil + 17) + n \\
當\; n \rightarrow \infty \Rightarrow \lceil\frac n2 \rceil + 17 = \frac n2 \\
\therefore T(n) = T(\frac n2) + n \Rightarrow T(n) = \Theta(n)
$$
