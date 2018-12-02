title: Linear Algebra - 重點分析 - LU分解
author: Willy Wang
tags:
  - LU分解
  - LU Decomposition
  - 重點
categories:
  - Linear Algebra
date: 2018-05-07 16:01:00
---
# Linear Algebra - 重點分析 LU分解

---


LU 分解的外表看似平淡無奇，但它可以用來解線性方程，逆矩陣和計算行列式，堪稱是最具實用價值的矩陣分解式之一。 

令 $A$ 為一個  $n\cdot n$ 階矩陣。LU 分解是指將 $A$ 表示為兩個 $n \cdot n$ 階三角矩陣的乘積


$$
A = L\cdot U
$$
其中 $L$ 是下三角矩陣，$U$ 是上三角矩陣，如下例，

$$
\begin{bmatrix}3 & 1 & 2 \\ 6 & -1 & 5 \\ -9 & 7 & 3\end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ -3 & 4 & 1 \end{bmatrix} \begin{bmatrix}3 & -1 & 2\\ 0 & 1 & 1\\ 0 & 0 & 5 \end{bmatrix}
$$

高斯消去法可以通過一連串的矩陣乘法來實現。每一個基本列運算等同於左乘一個基本矩陣，而對應列取代的基本矩陣 $E_{ij}$ 的 $(i, j)$ 元即為 $-l_{ij}$。 

消去程序可用下列矩陣乘法表示：

$$
E{32}E{31}E_{21}A=U
$$

因為基本矩陣 $E_{ij}$ 都是可逆的


$$
A=E_{21}^{-1}E_{31}^{-1}E_{32}^{-1}U=LU
$$

# 存在性

---

然而，並非任何可逆矩陣都具有 LU 分解形式，例如：$A=\begin{bmatrix}  0&2\\  1&3  \end{bmatrix}$。假若  $A$ 可以寫為


$$
A=LU=\begin{bmatrix}  1&0\\  l_{21}&1  \end{bmatrix}\begin{bmatrix}  u_{11}&u_{12}\\  0&u_{22}  \end{bmatrix}
$$


則必有 $u_{11}=0$， $U$ 是不可逆的，這與  為 $LU=A$ 可逆矩陣的事實相互矛盾。矩陣  之所 $A$ 以不存在  分解的$LU$ 原因在於 $0$ 占據了 $(1,1)$ 元，但軸元必須不為零才能產生乘數。根據這項觀察，即知可逆矩陣 $A$ 的 LU 分解存在條件是：列運算過程中，$0$ 不得在軸元位置。萬一碰上零軸元的情況，還是有補救辦法，那就是使用列交換運算設法產生其他非零軸元，不過 LU 分解要修改成 $PA=LU$，$P$ 是排列矩陣。例如，


$$
\begin{aligned}  PA&=\begin{bmatrix}  0&1\\  1&0  \end{bmatrix}\begin{bmatrix}  0&2\\  1&3  \end{bmatrix}=\begin{bmatrix}  1&3\\  0&2  \end{bmatrix}\\  &=\begin{bmatrix}  1&0\\  0&1  \end{bmatrix}\begin{bmatrix}  1&3\\  0&2  \end{bmatrix}=LU.\end{aligned}
$$

# 應用

---

最後討論 LU 分解的應用。LU 分解不僅僅只是記錄消去過程，它還有一個非常重要的實際用途：LU 分解具備快速求解線性方程 $A\mathbf{x}=\mathbf{b}​$ 的良好結構。一旦得到了可逆矩陣 $A​$ 的 LU 分解 $A=LU​$，我們大可把 $A​$ 拋棄，將 $A\mathbf{x}=\mathbf{b}​$ 改為 $L(U\mathbf{x})=\mathbf{b}​$，再令 $\mathbf{y}=U\mathbf{x}​$，原線性方程等價於兩組三角形系統：

$$
\begin{aligned}  L\mathbf{y}&=\mathbf{b}\\  U\mathbf{x}&=\mathbf{y}.  \end{aligned}
$$
接著使用兩次迭代即可得到解。上例中，先以正向迭代解出 $\mathbf{y}$，

$$
\left[\!\!\begin{array}{rcc}    1&0&0\\  2&1&0\\  -3&4&1  \end{array}\!\!\right]\begin{bmatrix}  y_1\\  y_2\\  y_3  \end{bmatrix}=\left[\!\!\begin{array}{r}    10\\  22\\  -7  \end{array}\!\!\right]\Rightarrow\begin{cases}  y_1=10&\\  y_2=-2y_1+22=2&\\  y_3=3y_1-4y_2-7=15&  \end{cases}
$$

再以反向迭代解出 ，

$$
\left[\!\!\begin{array}{crc}    3&-1&2\\  0&1&1\\  0&0&5  \end{array}\!\!\right]\begin{bmatrix}  x_1\\  x_2\\  x_3  \end{bmatrix}=\left[\!\!\begin{array}{r}    10\\  2\\  15  \end{array}\!\!\right]\Rightarrow\begin{cases}  x_1=(x_2-2x_3+10)/3=1&\\  x_2=-x_3+2=-1&\\  x_3=15/5=3&  \end{cases}
$$

對於  階矩陣 ，LU 分解耗費的乘法運算量大約是 $\mathbf{O}(\frac{1}{3}n^3)$，與高斯消去法相同。這個數字其實不算太糟，因為兩個 $n$ 階方陣相乘就使用了 $n^3$ 次運算。另外，正向迭代或反向迭代的運算量都只有$\mathbf{O}(\frac{1}{2}n^2)$ ，遠比 LU 分解來的少。所以如果只要解出單一線性系統 ，直接用消去法化簡增廣矩陣  $\begin{bmatrix}    A\vert\mathbf{b}    \end{bmatrix}$ 和 LU 分解的兩段式解法兩者之間並沒有多大差別，但如果稍後還要解多個係數矩陣相同但常數向量改變的系統 ，LU 分解便能夠派上用場。舉例來說，LU 分解可以用來計算逆矩陣 $A^{-1}$。將矩陣方程  看成三個線性方程：

$$
A\mathbf{x}_1=\begin{bmatrix}  1\\  0\\  0  \end{bmatrix},~ A\mathbf{x}_2=\begin{bmatrix}  0\\  1\\  0  \end{bmatrix},~ A\mathbf{x}_3=\begin{bmatrix}  0\\  0\\  1  \end{bmatrix}
$$

解出的未知向量 $\mathbf{x}_i$，$i=1,2,3$，就是逆矩陣 $A^{-1}$ 行向量。LU 分解還可以用來計算 $n\times n$ 階行列式。根據矩陣乘積的行列式可乘公式

$$
\det A=\det(LU)=(\det L)(\det U)
$$

三角矩陣的行列式等於主對角元乘積，因此 $\mathrm{det}L=1$，推論

$$
\det A=\det U=\displaystyle\prod_{i=1}^nu_{ii}
$$

方陣 $A$ 的行列式即為消去法所得到的上三角矩陣 $U$ 主對角元之積 (關於其他行列式計算方法的介紹，請見“[Chiò 演算法──另類行列式計算法](https://ccjou.wordpress.com/2009/11/24/%e5%8f%a6%e9%a1%9e%e8%a1%8c%e5%88%97%e5%bc%8f%e8%a8%88%e7%ae%97%e6%b3%95-chio-%e6%bc%94%e7%ae%97%e6%b3%95/)”)。

# 參考

---

[LU 分解| 線代啟示錄](https://ccjou.wordpress.com/2010/09/01/lu-%E5%88%86%E8%A7%A3/)