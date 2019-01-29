title: Linear algebra - Orthogonal complement
author: Willy Wang (willywangkaa)
tags:
  - Orthogonal complement
categories:
  - Linear Algebra
  - ''
date: 2018-10-27 11:30:00
---
# Minimal solution



假設 $A \in F^{m\times n}$，$b \in F^{m\times 1}$，其中 Ax = b 有解，若 s 為 Ax = b 的**某一解**滿足：

「Ax = b 的其他解 u 使得 $\left \| s \right \| \leq \left \| u \right \|$」，則稱 s 為 Ax = b 的極小解 ( Minimal solution )。



## Theorem



假設 $A \in F^{m\times n}​$，$b \in F^{m}​$，若 Ax = b 有解，則

- **唯一存在** $s \in R(A^H)$ 使得 s 為 Ax = b 的**極小解**
- 若 u 滿足 $AA^Hu = b$ 則 s = $A^H u$



![minimalsol_1](\willywangkaa\images\minimalsol_1.png)



![minimalsol_2](\willywangkaa\images\minimalsol_2.png)



![minimalsol_3](\willywangkaa\images\minimalsol_3.png)



### 猜想



Example 求下列線性系統的極小解



$\left\{\begin{matrix} x + 2y + z = 4 \\ x - y +2z = -11  \\ x + 5y = 19\end{matrix}\right.$


$$
[A|b] = \left[
\begin{array}{ccc|c}
1 & 2 & 1 & 4 \\ 
1 & -1 & 2 & -11 \\ 
1 & 5 & 0 & 19 \\ 
\end{array}
\right] \\
\Rightarrow \left[
\begin{array}{ccc|c}
1 & 2 & 1 & 4 \\ 
0 & -3 & 1 & -15 \\ 
0 & 3 & -1 & 15 \\ 
\end{array}
\right] \\
\Rightarrow \left[
\begin{array}{ccc|c}
1 & 2 & 1 & 4 \\ 
0 & 1 & \frac {-1}{3} & 5 \\ 
0 & 0 & 0 & 0 \\ 
\end{array}
\right] \\
\Rightarrow \left[
\begin{array}{ccc|c}
1 & 0 & \frac 53 & -6 \\ 
0 & 1 & \frac {-1}{3} & 5 \\ 
0 & 0 & 0 & 0 \\ 
\end{array}
\right]
$$
$x = -6 - \frac53z \\ y = 5 + \frac 13 z \\ \Rightarrow \left[ \begin{array}{} -6 \\ 5 \\ 0 \end{array} \right] + \left[ \begin{array}{} -\frac53 \\ \frac 13 \\ 1 \end{array} \right]z,  z\in R$

> $\left[ \begin{array}{} -6 \\ 5 \\ 0 \end{array} \right]$ 為特解向量，$ \left[ \begin{array}{} 5 \\ -1 \\ -3 \end{array} \right]z,  z\in R$ 為 Ker(A) ，
> 將特解向量投影至 Ker(A) 的分量為 $-\left[ \begin{array}{} 5 \\ -1 \\ -3 \end{array} \right]$ ，
> 所以 $\left[ \begin{array}{} -6 \\ 5 \\ 0 \end{array} \right] + \left[ \begin{array}{} 5 \\ -1 \\ -3 \end{array} \right] = \left[ \begin{array}{} -1 \\ 4 \\ 3 \end{array} \right]$ 為特解在 $Im(A^T)$ 的分量，即為極小解。

使用上述解法與 $AA^T u = b$ 解出之 $A^Tu = x$ 一致。