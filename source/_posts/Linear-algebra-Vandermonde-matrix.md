title: Linear algebra - Vandermonde matrix
author: Willy Wang (willywangkaa)
tags:
  - Vandermonde matrix
categories:
  - Linear Algebra
  - ''
date: 2019-01-22 15:08:00
---
# Linear algebra - Vandermonde matrix



- 法國數學家范德蒙 (Alexandre-Théophile Vandermonde) 是**行列式的奠基者**之一，他在十八世紀提出行列式專有符號，將行列式應用於解線性方程組，並且對行列式理論進行了開創性的研究
- 兩百多年後，他的名字因為「Vandermonde 矩陣」而經常被提及，具有以下形式：
  - $A_n=\begin{bmatrix}
    1&x_1&x_1^2&\cdots&x_1^{n-1} \\
    1&x_2&x_2^2&\cdots&x_2^{n-1} \\
    \vdots&\vdots&\vdots&\ddots&\vdots\\
    1&x_n&x_n^2&\cdots&x_n^{n-1}
    \end{bmatrix}，其中\; x_1, x_2, x_3, \ldots, x_n\;全相異$
  - $(A_n)^T$ 也稱為 Vandermonde 矩陣



## 推導



令 $f(t) = V(x_1, x_2, \ldots, x_{n-1}, t) = A_n=\begin{bmatrix}1&x_1&x_1^2&\cdots&x_1^{n-1} \\1&x_2&x_2^2&\cdots&x_2^{n-1} \\\vdots&\vdots&\vdots&\ddots&\vdots\\1&t&t^2&\cdots&t^{n-1} \end{bmatrix}​$ 

> 上面方程式可以視為一個 t 的函數



1. 對**第 n 列作降階求行列式**得到 $c_0 + c_1t + \ldots + c_{n-1} t^{n-1}$，為一個「t 的 n-1 次多項式」

2. 其中 $c_0, c_1, \ldots, c_{n-1}$ 為**不含 t 的常數**

   -  $c_{n-1}$ 為去除**第 n 行與第 n 列**後的行列式：
     - $c_{n-1} = V(x_1, x_2, \ldots, x_{n-1}) $

3. 用 $x_1,\ldots , x_{n-1}$ 代入 f

   - $f(x_1) = \det\begin{bmatrix}
     1&x_1&x_1^2&\cdots&x_1^{n-1} \\
     1&x_2&x_2^2&\cdots&x_2^{n-1} \\
     \vdots&\vdots&\vdots&\ddots&\vdots\\
     1&x_1&x_1^2&\cdots&x_1^{n-1}
     \end{bmatrix} = 0 = f(x_2) = \ldots = f(x_{n-1})$

   - 可以知道 f(t) **有 n-1 個相異根** $x_1, x_2, \ldots, x_{n-1}$
     - $\Rightarrow f(t) = c_{n-1}(t-x_1)(t-x_2)\ldots(t-x_{n-1})$
     - 將 $x_n$ 代入：
       - $f(x_n) = V(x_1, x_2, \ldots, x_n) = V(x_1, x_2, \ldots, x_{n-1})(x_n-x_1)\ldots(x_n-x_{n-1})$

4. $V(x_1, x_2, \ldots, x_n) = V(x_1, x_2, \ldots, x_{n-1})(x_n-x_1)(x_n-x_2)\ldots(x_n-x_{n-1})$

   - $\Rightarrow V(x_1, \ldots, x_{n-2})[(x_{n-1}-x_1)\ldots(x_{n-1}-x_{n-2})][(x_n-x_1)\ldots(x_n-x_{n-1})]$
   - $\Rightarrow V(x_1, x_2)[(x_3-x_1)(x_3-x_2)]\ldots[(x_n-x_1)(x_n-x_2)...(x_n-x_{n-1})] \\$
   - $\Rightarrow \prod_{1\leq i\leq j \leq n} (x_j-x_i)$  


## 應用



- Vandermonde 矩陣常見於**數值分析的內插** (interpolation) 問題
  - 給出 n 個資料點 $(x_i, y_i)$，$i=1,2,\ldots,n$，求 n-1 次多項式
    - $p(x)=a_{n-1}x^{n-1}+a_{n-2}x^{n-2}+\cdots+a_1x+a_0$ 並滿足
    - $\begin{aligned}  p(x_1)&=a_0+a_1x_1+\cdots+a_{n-1}x_1^{n-1}=y_1\\ p(x_2)&=a_0+a_1x_2+\cdots+a_{n-1}x_2^{n-1}=y_2\\
      &\vdots \\
      p(x_n)&=a_0+a_1x_n+\cdots+a_{n-1}x_n^{n-1}=y_n
      \end{aligned}$
  - 將上面的線性方程組寫為矩陣形式 $A\mathbf{a}=\mathbf{y}$
    - A 為 n×n 階 Vandermonde 矩陣
    - 內插問題就是要解出係數向量 $\mathbf{a}=\begin{bmatrix}    a_0&a_1&\cdots&a_{n-1}\end{bmatrix}^T$
    - 如果 n 個參數 $x_1,x_2,\ldots,x_n$ 彼此相異，推知 $\mathrm{det}A_n\neq 0$， A 是可逆的，方程式必定存在**唯一解**