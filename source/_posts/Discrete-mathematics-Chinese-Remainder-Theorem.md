title: Discrete mathematics - Chinese Remainder Theorem
author: Willy Wang
tags:
  - Chinese Remainder Theorem
categories:
  - Discrete Mathematics
  - ''
date: 2018-05-08 21:33:00
---
# Modular 反元素 (模反元素、數論倒數)

---



整數 $a$ 對同於 $n$ 之**模反元素**滿足以下：



$$a^{-1} \equiv b \quad (mod \; n)$$



等價於



$$a \cdot b \equiv 1 \quad (mod \; n)$$



**＜Note＞：**整數 $a$ 對模數 $n$ 之模反元素存在的**充分必要條件(iff)**是"$a$"與"$n$"**互質**。



## 求 Modular 反元素



- 輾轉相除法( 歐基里德演算法 )
  - 原理：$a, b$兩整數最大公因數等於**各自**$( a , b )$與**兩數相差( a - b )**的最大公因數。
  - $Ex$： 252 與 105 的最大公因數為 21 <br>$252 = 21 \cdot 12 \quad 105 = 21 \cdot 5$<br>$(21 \cdot 12) - (21\cdot 5) = 21 \cdot (12 - 5) = 21 \cdot 7 = 147$
  - $gcd(252, 105) = gcd(252, 147) = gcd(147, 105)$

使用**原理**的過程之中，較大的數的最大公因數可以由較小的數所代表，所以繼續進行同樣的計算可以不斷縮小$a, b$兩數，直到最後有一數變為 $0$ 。這時所剩下的**非零數**就是兩數的最大公因數。

由歐幾里得演算法的過程之中，可以推出兩數的最大公因數能用**兩數的整數倍**( $\forall k \in \mathbb{Z}$ )相加來表示，承上例：<br>$21 = 5 \cdot 105 + (-2) \cdot 252$



### Bézout's lemma - 貝組定理 ([丟番圖方程](https://zh.wikipedia.org/wiki/%E4%B8%9F%E7%95%AA%E5%9C%96%E6%96%B9%E7%A8%8B))



$\forall a, b, m \in \mathbb{Z}$ ，求未知數 $x, y$ 的[線性](https://zh.wikipedia.org/wiki/%E7%B7%9A%E6%80%A7%E6%96%B9%E7%A8%8B)[丟番圖方程式](https://zh.wikipedia.org/wiki/%E4%B8%9F%E7%95%AA%E5%9C%96%E6%96%B9%E7%A8%8B)（稱為**貝祖等式**）：



$$a \cdot x + b \cdot y = m$$



當 $x, y $有**整數解**時若且唯若( $\Leftrightarrow$ ) $gcd(a, b)\; | \; m$ 。此等式有解時必然有無窮多個解，每組解 $x, y$ 都稱為**貝組數**可用[擴展歐幾里得演算法](https://zh.wikipedia.org/wiki/%E6%93%B4%E5%B1%95%E6%AD%90%E5%B9%BE%E9%87%8C%E5%BE%97%E6%BC%94%E7%AE%97%E6%B3%95)求得，也就是說若 $a$ 為負數$a \cdot (-x) + b \cdot y = m$ 有整數解時$gcd(|a|, b) \;| \; m$。

- $Ex. $ 求 $47 \cdot x + 30 \cdot y = 1$，求 $x, y$？
  - $47 = 1 \cdot 30 + 17 $ <br> $\Rightarrow 30 = 1\cdot 17 + 13$ <br> $\Rightarrow 17 = 1 \cdot 13 + 4$<br>$\Rightarrow 13 = 3\cdot 4 +1$ <br> 我們得知  $gcd(47, 30) = 1$且$1 \; | \; 1$，所以接著改寫成「餘數等於」的形式<br>$17 = 1\cdot 47 - 1 \cdot 30$ <br> $13 = 1\cdot 30 - 1 \cdot 17$ <br> $4 = 1\cdot 17 - 1 \cdot 13$<br>$1 = 1\cdot 13 - 3 \cdot 4$<br>最後再反著倒寫回去<br>$1 = 1\cdot 13 - 3 \cdot 4$<br>$\Rightarrow 1 = 1\cdot 13 - 3 \cdot (1\cdot 17 - 1\cdot 13)$<br>$\Rightarrow 1 = (-3)\cdot 17 - 4 \cdot 13$<br>$\Rightarrow 1 = (-3)\cdot 17 - 4 \cdot (1\cdot 30 - 1 \cdot 17)$ <br> $\Rightarrow 1 = 4\cdot 30 + (-7)\cdot 17 $ <br> $\Rightarrow 1 = 4\cdot 30 + (-7)\cdot ( 1 \cdot 47 - 1 \cdot 30 )$ <br> $\Rightarrow 1 = 47 \cdot (-7) + 30\cdot 11$ <br>其中， $x = -7$ 與 $y = 11$ 為其中一組**貝組數**，其無限解為 $x = -7 + 30\cdot k, y = 11 - 47 \cdot k, \forall k \in \mathbb{Z}$
- Modular 反元素
  - 若貝組等式 $a \cdot x + b \cdot y = 1$ (若 $\neq 1$ 則模反元素不存在)<br>則：$a \cdot x = 1 - b\cdot y \Leftrightarrow a\cdot x \equiv 1 \quad (mod \; b)$<br>所以$a \cdot a^{-1} \equiv 1 \quad (mod \; b)$ ，此時$x$為$a$的一個模反元素，其無限表示式為$a^{-1} = x + k\cdot b, \forall k \in \mathbb{Z}$。



# Modular - 同餘的性質

---



- 整除性
  - $a\equiv b \quad (\mod m) \Rightarrow c \cdot m  = a - b , c \in \mathbb{Z}$<br>$\Rightarrow a \equiv b\quad ( \mod m ) \Rightarrow m \; | \; a-b$
- 遞移性
  - 若 $a \equiv b \quad (\mod c) , b \equiv d \quad (\mod c)$<br>則 $a \equiv d (\mod c)$
- 保持基本運算
  - $\left \{ \begin{matrix} a \equiv b (\mod m)\\ c \equiv d (\mod m)\end{matrix}\right. \Rightarrow \left\{\begin{matrix}a \pm c \equiv b \pm d (\mod m)\\ a \cdot c \equiv b \cdot d (\mod m)\end{matrix}\right.$
- 放大縮小模數
  - 令$k \in \mathbb{Z}^+ , a \equiv b \quad (\mod m) \Leftrightarrow k \cdot a \equiv k \cdot b \quad (\mod k \cdot m)$
- 費馬小定理
  - 假設 $a \in \mathbb{Z}$ 且 $p$ 是質數 $\ni gcd (a, p) = 1$，則：<br>$a^p \equiv a \quad (\mod p)$<br>$\Leftrightarrow a^{p-1} \equiv 1 \quad (\mod p)$
- 由拉 $\phi$ - 函數
  - 假設 $n \in \mathbb{Z}^+$，定義$\phi (n)$ 為 $\\{ 1, 2, \ldots, n-1 \\}$ 中與$n$互質(coprime)的元素個數。
  - 假設 $m \in \mathbb{Z}, n \in \mathbb{Z}^+$且 $gcd(m, n) = 1$，則：<br>$m^{\phi (n)} \equiv 1 \quad (\mod n)$

# Chinese Remainder Theorem - 中國剩餘定理

---



用例子來推演整個過程：

- $Ex.$ 求 

$$
\left\{\begin{matrix} x & \equiv & 2 & (  \mod 3 & )\\ x & \equiv & 3 & (  \mod 5 & )\\ x & \equiv & 2 & ( \mod 7 & )\end{matrix}\right.
$$

  - 首先拆開來解方便計算：
    - $\left\{\begin{matrix} a_1 & \equiv & 2 & (  \mod 3 & )\\ a_1 & \equiv & 0 & (  \mod 5 & )\\ a_1 & \equiv & 0 & ( \mod 7 & )\end{matrix}\right. \Rightarrow a_1 = 35 \cdot n_1$
    - $\left\{\begin{matrix} a_2 & \equiv & 0 & (  \mod 3 & )\\ a_2 & \equiv & 3 & (  \mod 5 & )\\ a_2 & \equiv & 0 & ( \mod 7 & )\end{matrix}\right. \Rightarrow a_2 = 21 \cdot n_2$
    - $\left\{\begin{matrix} a_3 & \equiv & 0 & (  \mod 3 & )\\ a_3 & \equiv & 0 & (  \mod 5 & )\\ a_3 & \equiv & 2 & ( \mod 7 & )\end{matrix}\right. \Rightarrow a_3 = 15 \cdot n_3$
  - 接著使用**同餘的保持基本運算**，令$x = a_1 + a_2 + a_3$：<br>$x = a_1 + a_2 + a_3 \equiv 2 \quad (\mod 3)$<br>$x = a_1 + a_2 + a_3 \equiv 3 \quad (\mod 5)$<br>$x = a_1 + a_2 + a_3 \equiv 2 \quad (\mod 7)$
  - 計算 $a_1$：<br>$$\left\{\begin{matrix} a_1 & \equiv & 2 & (  \mod 3 & )\\ a_1 & \equiv & 0 & (  \mod 5 & )\\ a_1 & \equiv & 0 & ( \mod 7 & )\end{matrix}\right.$$ ，$a_1 = 35\cdot n_1 \equiv 2 \quad(\mod 3)$不好運算，轉成$b_1 = 35\cdot m_1 \equiv 1 \quad(\mod 3)$<br>$\Rightarrow 35 = 11 \cdot 3 + 2$<br>$\Rightarrow 3 = 2 \cdot 1 + 1$<br>$1 = 1 \cdot 3 - 1  \cdot 2$<br>$\Leftrightarrow 1 = 1 \cdot 3 - 1 \cdot ( 1 \cdot 35 - 11 \cdot 3 )$<br>$\Leftrightarrow 1 = (-1) \cdot 35 + 12 \cdot 3$<br>令$m_1 = -1 + 3 \cdot k$ (模反元素)<br>所以，$b_1 = 35\cdot m_1 \equiv 1 \quad(\mod 3) \Leftrightarrow 2 \cdot b_1 \equiv 2 \cdot 35 \cdot m_1 \equiv 2 \cdot 1 \quad (\mod 3)$<br>則可以令$a_1 = b_1\cdot 2$，取$k = 1, b_1 = 35 \cdot 2 = 70 \Rightarrow a_1 = 140$
  - 計算$a_2$：<br>$$\left\{\begin{matrix} a_2 & \equiv & 0 & (  \mod 3 & )\\ a_2 & \equiv & 3 & (  \mod 5 & )\\ a_2 & \equiv & 0 & ( \mod 7 & )\end{matrix}\right.$$ ，$a_2 = 21\cdot n_2 \equiv 3 \quad(\mod 5)$不好運算，轉成$b_2 = 21\cdot m_2 \equiv 1 \quad(\mod 5)$<br>$\Rightarrow 21 = 4 \cdot 5 + 1$<br>$1 = 21 \cdot 1 - 4 \cdot 5$<br>令$m_2 = 1 + 5 \cdot k$ (模反元素)<br>所以，$b_2 = 21 \cdot m_2 \equiv 1 \quad (\mod 5) \Leftrightarrow 3 \cdot b_2 = 3\cdot 21 \cdot m_2 \equiv 3 \cdot 1 \quad (\mod 5)$<br>所以令$a_2 = b_2 \cdot 3$，取$k = 0, b_2 = 21 \cdot 1 = 21 \Rightarrow a_2 = 63$
  - 計算$a_3$：<br>$$\left\{\begin{matrix} a_3 & \equiv & 0 & (  \mod 3 & )\\ a_3 & \equiv & 0 & (  \mod 5 & )\\ a_3 & \equiv & 2 & ( \mod 7 & )\end{matrix}\right.$$ ，$a_3 = 15\cdot n_3 \equiv 2 \quad(\mod 7)$不好運算，轉成$b_3 = 15\cdot m_3 \equiv 1 \quad(\mod 7)$<br>$\Rightarrow 15 = 2 \cdot 7 + 1$<br>$1 = 15 \cdot 1 - 2 \cdot 7$<br>令$m_3 = 1 + 7 \cdot k$ (模反元素)<br>所以，$b_3 = 15 \cdot m_3 \equiv 1 \quad (\mod 7) \Leftrightarrow 2 \cdot b_3 = 2\cdot 15 \cdot m_3 \equiv 2 \cdot 1 \quad (\mod 7)$<br>所以令$a_3 = b_3 \cdot 2$，取$k = 0, b_3 = 15 \cdot 1 = 15 \Rightarrow a_3 = 30$
  - 最後計算 $x = a_1 + a_2 + a_3 = 233$，驗算<br>$\left\{\begin{matrix} 233 & \equiv & 2 & (  \mod 3 & )\\ 233 & \equiv & 3 & (  \mod 5 & )\\ 233 & \equiv & 2 & ( \mod 7 & )\end{matrix}\right.$ ，OK。



## 常見題目類型



### Type 1 (模數都互質)



- $Ex.$(99 政大)
  - $\left\{\begin{matrix} x & \equiv & 5 & (  \mod 7 & )\\ x & \equiv & 4 & (  \mod 9 & )\\ x & \equiv & 3 & ( \mod 13 & )\end{matrix}\right.$



### Type 2 (模數不全是互質)



- $Ex.$(97 台科大)
  - $\left\{\begin{matrix} x & \equiv & 1 & (  \mod 2 & )\\ x & \equiv & 2 & (  \mod 3 & )\\ x & \equiv & 8 & ( \mod 15 & )\end{matrix}\right.$



### Type 3 (模數重疊)



- $Ex.$(97 高師大)
  - $\left\{\begin{matrix} x & \equiv & 1 & (  \mod 3 & )\\ x & \equiv & 13 & (  \mod 16 & )\\ x & \equiv & 73 & ( \mod 81 & )\end{matrix}\right.$



### Type 4 (矛盾 - 無解)



- $Ex.$(97 台科大 - 改)
  - $\left\{\begin{matrix} x & \equiv & 1 & (  \mod 2 & )\\ x & \equiv & 2 & (  \mod 3 & )\\ x & \equiv & 10 & ( \mod 15 & )\end{matrix}\right.$