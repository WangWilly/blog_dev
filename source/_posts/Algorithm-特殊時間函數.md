title: Algorithm - 特殊時間函數
author: Willy Wang (willywangkaa)
tags:
  - Function
  - Time complexity
categories:
  - Algorithm
date: 2018-10-15 20:51:00
---
# 特殊的函數



- $(\log n)^{\log n}$

$$
\Rightarrow n^{\log \log n} \\
 令 \;d \;為一常數\\
\because d\times \log n < \log \log n\times \log n \\
\log \log n \times \log n< \log n \times \log n \\
\log n \times \log n < n \\
n < n \times \log 2 \\
\therefore n^{\log \log n} > n^d \\
n^{\log \log n} < 2^n
$$



- $(\log n)!$

$$
\because n! \approx n^{n+\frac{1}{2}} \cdot e^{-n} \quad(Stirling \;approximation)\\
\therefore (\log n)! \approx (\log n)^{\log n + \frac 12}\cdot e^{-\log n} \\
\Rightarrow (\log n)^{\log n + \frac 12}\cdot n^{-\log e} \\
\Rightarrow (\log n)^{\log n + \frac 12}\cdot n^{-1} \\
\Rightarrow n^{\log \log n} \cdot (\log n)^{\frac12}\cdot n^{-1} \\
\therefore (\log n)! < (\log n)^{\log n}
$$



- $n^{\frac {1}{\log n}}$

$$
\because n = 2^{\log n} \\
\therefore (2^{\log n})^{\frac{1}{\log n}} \Rightarrow 2^{\log n \cdot \frac{1}{\log n}} \\
\Rightarrow 2^1 = 2
$$

- $2^{\sqrt{2 \log n}}$

$$
2 = n^{\frac{1}{\log n}} \\
\Rightarrow n ^{\frac{\sqrt{2\log n}}{\log n}} \\
\because 0 < \frac{\sqrt{2\log n}}{\log n} < 1 \\
\therefore \log n < n^{\frac{\sqrt{2\log n}}{\log n}} < n^c, \forall c \in Z^+ , c \geq 1
$$

- $(\log \log n)!$

> |    f(n)     |                   log f(n)                   |
> | :---------: | :------------------------------------------: |
> |     n!      |          $n\log n \notin O(\log n)$          |
> |    $2^n$    |          $n\log 2 \notin O(\log n)$          |
> | $(\log n)!$ | $(\log n)\cdot \log \log n \notin O(\log n)$ |
> |    $n^2$    |           $2\log n \in O(\log n)$            |
> |  $\log n$   |         $\log \log n \in O(\log n)$          |
> |    $2^4$    |           $4\log 2 \in O(\log n)$            |
>
> $\Rightarrow$ f(n) 為「Polynominal bound」$\Leftrightarrow$ $\log f(n) \in O(\log n)$

$$
\Rightarrow \log((\log \log n)!) = (\log \log n)\cdot(\log \log \log n) \\
\Rightarrow (\log \log n)\cdot(\log \log \log n) \leq (\log \log n)^2 \\
\Rightarrow (\log \log n)\cdot(\log \log \log n) = O(\log n) \\
\therefore (\log \log n)! \;為\;「Polynominal \;bound」
$$

