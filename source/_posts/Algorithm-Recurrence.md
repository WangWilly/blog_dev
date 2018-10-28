title: Algorithm - Recurrence
author: Willy Wang (willywangkaa)
tags:
  - Recurrence
  - Ackermann
categories:
  - Algorithm
date: 2018-09-01 01:49:00
---
# Recurrence



## Substutution method



- Concept

  1. **以經驗猜測一個邊界(bound)**。

  2. 假設**子**問題符合此邊界，證明**母**問題也符合此邊界。

- **適用時機**

  - 題目比分重。
  - 單向；單邊：只需要證明 $O, \Omega$ 單向，不必證明 $\Theta$ 。
  - 題目要求。



### 經典範例



- Ex
  - $T(n) = 2 T(floor(\frac n2))+n$，問$T(n) = O(？)$，單向證明
  - (substution method) 猜：$T(n) = (n \lg n)$
  - ＜想法＞已知$g(n) = 2 g(\frac n2)+n = O(n\lg n)$
  - 欲證$T(n) = O(n \lg n)$，即證明$\exists C, n_0>0 \ni n \geq n_0, T(n) \leq C \cdot n\lg n$；<br>假設**子問題** $T(floor(n)) \leq C \cdot floor(\frac n2)\lg floor(\frac n 2)$ 成立；<br>考慮(consider)：$T(n) = 2\cdot T(floor(\frac n2))+n \leq 2 \cdot C \cdot floor(\frac n2)\lg floor(\frac n2)+n$<br>$\Rightarrow T(n) = 2\cdot T(floor(\frac n2))+n \leq 2 \cdot C \cdot \frac n2\lg \frac n2+n$<br>$\Rightarrow T(n) = 2\cdot T(floor(\frac n2))+n \leq C \cdot n(\lg n - \lg2)+n = C \cdot n\lg n + (1 - C)n$<br>**取** $C \geq 2, T(n) = 2\cdot T(floor(\frac n2))+n \leq C \cdot n \lg n$<br>$\therefore T(n) = O(n\lg n)$
- Ex (97成大資工) (10%)
  - 解 $T(n) = 2T(\lfloor \sqrt n \rfloor)+\lg n$, 用 O 表示。( 比分重、單邊、$\lfloor\rfloor$ )<br>考慮使用 substution method 而不使用疊代法。
  -  $\lg n \lg \lg n$



## Recurrence tree



- 名詞解釋
  - $T(n) = T(\frac n3)+T(\frac 23 n) + n$
    - $r = \frac 13+\frac 23 = 1$**：子問題的 n 係數的相加。**
  - $T(n)$：母問題。
  - $T(\frac n3), T(\frac 23n)$：子問題。
  - $n$：Cost，將子問題合併的代價。
- **適用時機**
  - 子問題個數大於二
  - 子問題型態為$T(\frac n a)$



### 經典範例




![tree_1](\willywangkaa\images\tree_1.png)



- $r < 1$
  - Ex ( 94 成大資工 )

    - $T(n) = T(\frac n2) + T(\frac n4) + T(\frac n8) + n$，求 $\Theta$
  - Sol
    - $r = \frac n 2 + \frac n 4 + \frac n 8 = \frac 7 8 < 1$
    - 建立遞迴樹
      $T(n) = n + \frac 78 n +\frac{49}{64}n + \ldots + C_l$ (等比級數)
    - **求取其邊界 (夾擠法)**
      求上邊界：$T(n) = n + \frac 78 n +\frac{49}{64}n + \ldots + C_l \leq n + \frac 78 n +\frac{49}{64}n + \ldots + (\frac 78)^h$ 
      **(有限等比級數小於無限等比級數)**
      $\Rightarrow \frac{n}{1 - \frac 7 8} = 8 n \Rightarrow T(n) = O(n)$

      求下邊界：$T(n) = n + \frac 78 n +\frac{49}{64}n + \ldots + C_l \geq n \Rightarrow T(n) = \Omega(n)$
    - $\Rightarrow T(n) = \Theta(n)$




![tree_1](\willywangkaa\images\tree_2.png)



- $r = 1$
  - Ex (100 政大)(90 ,91 台大)
    - $T(n) = T(\frac n3) + T(\frac 23) + n$
  - Sol
    - $r = \frac 13+\frac{2}{3} = 1$
    - 建立遞迴樹
      $T(n) = n+n+n+\ldots+C_l$
    - 求取其邊界
      求上邊界：因為$(\frac23)^k*n= 1 \Rightarrow k = \log_{\frac32}n \Rightarrow 高度 = k + 1$ 
      ，所以$T(n) = n+n+n+\ldots+C_l \leq n \cdot \log_{\frac32}n+1$
      $\Rightarrow T(n) = O(n\log n)$
      br>求下邊界：$T(n) = n+n+n+\ldots+C_l \geq n \cdot \log_3n + 1$
      $\Rightarrow T(n) = \Omega(n\log n)$
    - $\Rightarrow T(n) = \Theta(n\lg n)$



- $r > 1$
  - 無，因為子問題的數量比母問題大。



## Master theorem



令 $T(n) = aT(\frac{n}{b})+f(n), a \geq 1, b > 1$ 。



- 若 $\exists \epsilon > 0 \ni f(n) = O(n^{\log_ba-\epsilon})$ ，則 $T(n) = \Theta(n^{\log_ba})$。
- 若 $\exists \epsilon > 0, 0 < c < 1 \ni f(n) = O(n^{\log_ba+\epsilon}) \;and\; af(\frac nb) < cf(n)$，則 $T(n) = \Theta(f(n))$。 
- ☆**若** $f(n) = \Theta(n^{\log_ba}\cdot \lg^kn), k \geq 0$，則 $T(n) = \Theta(n^{\log_ba}\cdot\lg^{k+1}n)$ (與 f(n) 相差 $lg$ 的次方)。



### 經典例題



- Ex ( 100 政大 )
  - $T(n) = 7T(\frac n2) + n^2$
  - sol
    - By master theorem $\Rightarrow \exists \epsilon = \lg 7 - 2  \ni n^2 = O(n^{\log_27-\epsilon})$，所以$T(n) = \Theta(n^{\lg 7})$。
- Ex ( 99 交大 )
  - $T(n) = 3T(\frac n4) + n\lg n$
  - sol
    - By master theorem $\Rightarrow n^{log_43} = n^{1-\epsilon}, where\; 0 < \epsilon < 1$<br>$\Rightarrow n\log n  = \omega(n) =  \omega(n^{1-\epsilon})$，所以$T(n) = \Theta(n\log n)$。
- Ex ( 98 交大 )
  - $T(n) = 3T(\frac n2)+ n \lg n$
  - sol
    - By master theorem $\Rightarrow n^{log_23} = n^{1+\epsilon}, where\; 0 < \epsilon < 1$<br>$\Rightarrow n\log n = o(n^{1+\epsilon})$，所以$T(n) = \Theta(n^{\log_23})$。
  - $T(n) = 4T(\frac n2) + n\lg n$
  - sol
    - By master theorem $\Rightarrow n^{log_24} = n^2$<br>$\Rightarrow n^{2} = \omega(n\log n)$，所以$T(n) = \Theta(n^2)$。
- **＜Note＞** ☆ 與 $f(n)$ 比較，$f(n)$ 多了$\frac{1}{\lg n}$
  - $T(n) = 4 T(\frac n2) + \frac {n^2}{\lg n} $
  - sol
    - 不可使用 Master Theorem。
    - $n^{\log_ba = n^2}, f(n) = \frac{n^2}{\lg n} \Rightarrow T(n) = n^2\lg \lg n$ 
- **＜Note＞** ☆ 與 $f(n)$ 比較，$f(n)$ 多了$\frac{1}{\lg^a n} , a > 1$
  - 直接為 $O(n^{\log_ab})$



# 進階遞迴問題



## Ackermann's function




$$
\left\{\begin{matrix}
A(0, n) & = & n+1 & (1)\\
A(m , 0) & = & A(m-1, 1) & (2)\\
A(m , n) & = & A(m-1, A(m, n-1)) & (3) 
\end{matrix}\right.
$$


- 求取 $A(1, n)$ 的通解。




$$
\begin{matrix}
令 \; a_n &=& A(1, n) & \\ 
& = & A(0, A(1, n-1)) & By\;(3) \\
& = & A(0, a_{n-1}) & By\; 自定義的a_n\\
& = & a_{n-1} + 1 & By \; (1) \\
\end{matrix}
$$



且 


$$
a_0  =  A(1, 0) = A(0, 1) = 2
$$



所以，


$$
\begin{matrix}
a_n & = & a_{n-1} + 1 \\
& = & (a_{n-2} + 1) + 1 \\
& = & ((a_{n-3} + 1) + 1) + 1 \\
& \ldots & \\
& = & a_0 + \sum_{i = 1}^{n-1} + 1 \\
& = & 2 + n & \forall n \geq 0
\end{matrix}
$$



- 求取 $A(2, n)$ 的通解。


$$
\begin{matrix}
令 \; b_n &=& A(2, n) & \\ 
& = & A(1, A(2, n-1)) & By\;(3) \\
& = & A(1, b_{n-1}) & By\; 自定義的a_n\\
& = & a_{b_{n-1}} & By \; 第一題結論 \\
& = & b_{n-1} + 2 \\ 
& = & (b_{n-2} + 2) + 2 \\
& = & ((b_{n-3}+2) + ) + 2 \\
& \ldots & \\
& = & b_0 + \sum_{i = 1}^{n-1}2 + 2 \\
& = & b_0 + 2n & b_0 = A(2, 0) = A(1, 1) = a_1 = 3\\
& = & 2n + 3 & \forall n \geq 0
\end{matrix}
$$


- 求取 $A(3, n)$ 的通解。


$$
\begin{matrix}
令 \; c_n &=& A(3, n) & \\ 
& = & A(2, A(3, n-1)) & By\;(3) \\
& = & A(2, c_{n-1}) & By\; 自定義的a_n\\
& = & b_{c_{n-1}} & By \; 第一題結論 \\
& = & 2c_{n-1} + 3
\end{matrix}
$$
先求出 $c_0 = A(3, 0) = A(2, 1) = b_1 = 5$ ，再來

$c_n$ 特徵多項式為 $\alpha - 2= 0 \Rightarrow \alpha = 2$，所以令齊次解為 $c_n^{(h)} = C_0\times2^n$，接著 $c_n$ 的特解為 $c_n^{(p)} = C_1\times (1)$ 代回求 $C_1$


$$
\begin{matrix}
\Rightarrow C_1 = 2\times C_1 + 3 \\
\Rightarrow C_1 = -3 \\
\therefore c_n = c_n^{h} + c_n^{(p)} = C_0\times2^n - 3 \\
c_0 = 5 = C_0 - 3 \\
\Rightarrow C_0 = 8 \\
\therefore c_n = 2^{n+3} - 3, \forall n \geq 0
\end{matrix}
$$


