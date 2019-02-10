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

> **＜外傳＞**
>
> - 某些問題隨輸入資料大小成指數成長是無法避免的
>   - 雖然「Divide-and-conquer」無法獲致良好的執行效率但仍可採用
> - 河內塔問題
>   - 每呼叫一次就需搬動圓盤一次
>     - 當圓盤的個數 n 為 64
>     - 總共需要搬動圓盤 $2^{64}-1$ 次
>     - 演算法的複雜度等級是 $O(2^n)$
>   - 河內塔問題**圓盤搬動次序與 n 成指數關係**
>   - 經過証明，上述河內塔問題的演算法
>     - 為該問題的限制下**最佳的演算法**

## Substitution method



- Concept

  1. **以經驗猜測一個邊界(bound)**。

  2. 假設**子**問題符合此邊界，證明**母**問題也符合此邊界。

- **適用時機**

  - 題目比分重。
  - 單向；單邊：只需要證明 $O, \Omega$ 單向，不必證明 $\Theta$ 。
  - 題目要求。



### 經典範例



Example
- $T(n) = 2 T(\lfloor\frac n2\rfloor)+n$，$T(n) = O(？)$

（Substitution method）猜：$T(n) = O(n \lg n)$ 

> 已知 $g(n) = 2 g(\frac n2)+n = O(n\lg n)$

欲證 $T(n) = O(n \lg n)$，即證明 $\exists C, n_0>0 \ni n \geq n_0, T(n) \leq C \cdot n\lg n$

假設**子問題** $T(n) \leq C \cdot \lfloor\frac n2\rfloor\lg \lfloor\frac n 2\rfloor$ 成立

考慮 $T(n) = 2\cdot T(\lfloor\frac n2\rfloor)+n \leq 2 \cdot C \cdot \lfloor\frac n2\rfloor\lg \lfloor\frac n2\rfloor+n$

$\Rightarrow T(n) = 2\cdot T(\lfloor\frac n2\rfloor)+n \leq 2 \cdot C \cdot \frac n2\lg \frac n2+n$

$\Rightarrow T(n) = 2\cdot T(\lfloor\frac n2\rfloor)+n \leq C \cdot n(\lg n - \lg2)+n = C \cdot n\lg n + (1 - C)n$

取 $C \geq 2, T(n) = 2\cdot T(\lfloor\frac n2\rfloor)+n \leq C \cdot n \lg n$

$\therefore T(n) = O(n\lg n)$



Example（97成大資工）
- 解 $T(n) = 2T(\lfloor \sqrt n \rfloor)+\lg n$, 用 O 表示
  - （比分重、單邊、$\lfloor\rfloor$）
    - 考慮使用「Substitution method」而不使用疊代法

$\lg n \lg \lg n$



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


- 求取 $A(1, n)$ 的通解




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



所以


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



- 求取 $A(2, n)$ 的通解


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


- 求取 $A(3, n)$ 的通解


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



## Strassen 演算法（分治矩陣乘法）

長久以來人們普遍認為矩陣乘法定義本身即為最佳的算法，這個迷思直到1969年才被施特拉森打破──他提出了一個更快捷的分治（Divide-and-conquer）矩陣乘法，為 Strassen 演算法



- 給定兩個方陣 A、B
  - 其大小均為 n×n
    - n = 2k
  - 若 n 非 2 的冪次方
    - 可將 A 和 B 擴充為 $\begin{bmatrix}  A&0\\  0&I  \end{bmatrix}$ 和 $\begin{bmatrix}  B&0\\  0&I  \end{bmatrix}$

> 考慮 2×2 階**分塊矩陣乘法**
>
> $\begin{bmatrix}  C_{11}&C_{12}\\  C_{21}&C_{22}  \end{bmatrix}=\begin{bmatrix}  A_{11}&A_{12}\\  A_{21}&A_{22}  \end{bmatrix}\begin{bmatrix}  B_{11}&B_{12}\\  B_{21}&B_{22}  \end{bmatrix}$
>
> - 傳統矩陣乘法
>   - $C_{ij}=A_{i1}B_{1j}+A_{i2}B_{2j}$
>
> ![1549349573405](\willywangkaa\images\1549349573405.png)
>
> $C_{11} = A_{11}B_{11} + A_{12}B_{21} \\ C_{12} = A_{11}B_{12} + A_{12}B_{22} \\ C_{21} = A_{21}B_{11} + A_{22}B_{21} \\ C_{22} = A_{21}B_{12} + A_{22}B_{22}$
>
> - 遞迴式為：$T(n) = 8T(\frac n2) + cn^2$
>   - 8 個分塊矩陣乘法
>   - 4 個分塊矩陣加法
>   - $cn^2\Rightarrow 3\times2^2$
>
> - 「Master theorem」
>   - $T(n) = \Theta(n^3)$

- Strassen 演算法

$\begin{aligned}  P_1&=(A_{11}+A_{22})(B_{11}+B_{22})\\  P_2&=(A_{21}+A_{22})B_{11}\\  P_3&=A_{11}(B_{12}-B_{22})\\  P_4&=A_{22}(B_{21}-B_{11})\\  P_5&=(A_{11}+A_{12})B_{22}\\  P_6&=(A_{21}-A_{11})(B_{11}+B_{12})\\  P_7&=(A_{12}-A_{22})(B_{21}+B_{22})\\  C_{11}&=P_1+P_4-P_5+P_7\\  C_{12}&=P_3+P_5\\  C_{21}&=P_2+P_4\\  C_{22}&=P_1+P_3-P_2+P_6  \end{aligned}$

- 遞迴式為：$T(n) = 7T(\frac n2) + cn^2$
  - 7 個分塊矩陣乘法
  - 18 個分塊矩陣加法

- 「Master theorem」
  - $T(n) = \Theta(n^{\log_27}) \approx \Theta(n^{2.807})$

> - 根據 2000 年的硬體架構
>   - 矩陣尺寸必須超過1000，Strassen 演算法才可能擊敗經過高度優化的傳統算法
>   - 即便矩陣尺寸增大至 10000 相較於傳統算法
>     - Strassen 演算法所能提昇的運算效率依然十分有限（約小於10%）



### 算法推導



- 考慮 2×2 階矩陣 $\begin{bmatrix}  w&x\\  y&z  \end{bmatrix}=\begin{bmatrix}  a&b\\  c&d  \end{bmatrix}\begin{bmatrix}  p&q\\  r&s  \end{bmatrix}$
  - 乘開後 $\begin{aligned}  w&=ap+br\\  y&=cp+dr\\  x&=aq+bs\\  z&=cq+ds  \end{aligned}$
  - 將四個式子合併成矩陣表達式 $\begin{bmatrix}  w\\  y\\  x\\  z  \end{bmatrix}=\begin{bmatrix}  a&b&0&0\\  c&d&0&0\\  0&0&a&b\\  0&0&c&d  \end{bmatrix}\begin{bmatrix}  p\\  r\\  q\\  s  \end{bmatrix}$
- 欲將 4×4 階的矩陣分解成數個矩陣之和以減少乘法運算
  - **分解出來的矩陣至多僅含4個非零元，且所有非零元都位於一 2×2 階子陣**
  - 如：$\begin{bmatrix}  \ast&\ast&0&0\\  \ast&\ast&0&0\\  0&0&0&0\\  0&0&0&0  \end{bmatrix} 、\begin{bmatrix}  0&0&0&0\\  \ast&0&\ast&0\\  0&0&0&0\\  \ast&0&\ast&0  \end{bmatrix}、\begin{bmatrix}  \ast&0&0&\ast\\  0&0&0&0\\  0&0&0&0\\  \ast&0&0&\ast  \end{bmatrix}$

> 考慮三種分解型態
>
> - **型態 I**
>   - 子陣有相同的元
>   - 需要一個乘法運算
>   - $\begin{bmatrix}  a&a\\  a&a  \end{bmatrix}\begin{bmatrix}  u\\  v  \end{bmatrix}=\begin{bmatrix}  a(u+v)\\  a(u+v)  \end{bmatrix}$
> - **型態 II**
>   - 子陣的元有相同的絕對值
>   - **兩行或兩列不同號**
>   - 需要1個乘法運算
>   - $\begin{bmatrix}  a&-a\\  a&-a  \end{bmatrix}\begin{bmatrix}  u\\  v  \end{bmatrix}=\begin{bmatrix}  a(u-v)\\  a(u-v)  \end{bmatrix}$
>
> - **型態 III**
>   - 子陣為上或下三角矩陣
>   - （非零）非主對角元等於兩主對角元之差
>   - **需要2個乘法運算**
>   - $\begin{bmatrix}  a&0\\  a-b&b  \end{bmatrix}\begin{bmatrix}  u\\  v  \end{bmatrix}=\begin{bmatrix}  au\\  au+b(v-u)  \end{bmatrix}$
>     - 可分解為兩個退化的**型態 I**和**型態 II**（退化是指存在零行或零列）
>     - $\begin{bmatrix}  a&0\\  a-b&b  \end{bmatrix}=\begin{bmatrix}  a&0\\  a&0  \end{bmatrix}+\begin{bmatrix}  0&0\\  -b&-b  \end{bmatrix}$

分解矩陣製造**型態 I**和**型態 II**

1. $\begin{bmatrix}  a&b&0&0\\  c&d&0&0\\  0&0&a&b\\  0&0&c&d  \end{bmatrix}=\begin{bmatrix} -d&d&0&0\\  -d&d&0&0\\  0&0&a&-a\\  0&0&a&-a  \end{bmatrix}+\begin{bmatrix}  a+d&b-d&0&0\\  c+d&0&0&0\\  0&0&0&a+b\\  0&0&c-a&a+d  \end{bmatrix}$

   - 等號右邊的第一個矩陣
     - $\begin{bmatrix} -d&d&0&0\\  -d&d&0&0\\  0&0&0&0\\  0&0&0&0 \end{bmatrix}+\begin{bmatrix} 0&0&0&0\\  0&0&0&0\\  0&0&a&-a\\  0&0&a&-a  \end{bmatrix}$

2. 令 $M_1$ 表示上式等號**右邊的第二個矩陣**

   - 觀察發現 $(M_1)_{11}=(M_1)_{44}=a+d $
   - $M_1=\begin{bmatrix}  a+d&0&0&a+d\\  0&0&0&0\\  0&0&0&0\\  a+d&0&0&a+d  \end{bmatrix}+\begin{bmatrix}  0&b-d&0&-(a+d)\\  c+d&0&0&0\\  0&0&0&a+b\\  -(a+d)&0&c-a&0  \end{bmatrix}$

3. 令 $M_2$ 表示上式等號**右邊的第二個矩陣**

   - 觀察發現 $M_2$ 可分解成兩個**型態 III**

   - $M_2=\begin{bmatrix}  0&0&0&0\\  c+d&0&0&0\\  0&0&0&0\\  (c-a)-(c+d)&0&c-a&0  \end{bmatrix}+\begin{bmatrix}  0&b-d&0&(b-d)-(a+b)\\  0&0&0&0\\  0&0&0&a+b\\  0&0&0&0  \end{bmatrix}$
   - 等號右邊的第一個矩陣
     - $\begin{bmatrix}  0&0&0&0\\  c+d&0&0&0\\  0&0&0&0\\  -(c+d)&0&0&0  \end{bmatrix}+\begin{bmatrix}  0&0&0&0\\  0&0&0&0\\  0&0&0&0\\  c-a&0&c-a&0  \end{bmatrix}$
   - 等號右邊的第二個矩陣
     - $\begin{bmatrix}  0&b-d&0&b-d\\  0&0&0&0\\  0&0&0&0\\  0&0&0&0  \end{bmatrix}+\begin{bmatrix}  0&0&0&0-(a+b)\\  0&0&0&0\\  0&0&0&a+b\\  0&0&0&0  \end{bmatrix}$

4. 整理所有的分解矩陣，**按照「Strassen 演算法」給定的排序**

   - $\begin{aligned}  \begin{bmatrix}  a&b&0&0\\  c&d&0&0\\  0&0&a&b\\  0&0&c&d  \end{bmatrix}&=\begin{bmatrix}  a+d&0&0&a+d\\  0&0&0&0\\  0&0&0&0\\  a+d&0&0&a+d  \end{bmatrix}+\begin{bmatrix}  0&0&0&0\\  c+d&0&0&0\\  0&0&0&0\\  -(c+d)&0&0&0  \end{bmatrix}\\  &+\begin{bmatrix}  0&0&0&0\\  0&0&0&0\\  0&0&a&-a\\  0&0&a&-a  \end{bmatrix}+\begin{bmatrix}  -d&d&0&0\\  -d&d&0&0\\  0&0&0&0\\  0&0&0&0  \end{bmatrix}\\  &+\begin{bmatrix}  0&0&0&-(a+b)\\  0&0&0&0\\  0&0&0&a+b\\  0&0&0&0  \end{bmatrix}+\begin{bmatrix}  0&0&0&0\\  0&0&0&0\\  0&0&0&0\\  c-a&0&c-a&0  \end{bmatrix}\\  &+\begin{bmatrix}  0&b-d&0&b-d\\  0&0&0&0\\  0&0&0&0\\  0&0&0&0  \end{bmatrix}  \end{aligned}$ 
   - 可以表示為「行列展開」的矩陣乘法
   - $\displaystyle\begin{aligned}  \begin{bmatrix}  a&b&0&0\\  c&d&0&0\\  0&0&a&b\\  0&0&c&d  \end{bmatrix}&=\begin{bmatrix}  1\\  0\\  0\\  1  \end{bmatrix}(a+d)\begin{bmatrix}  1&0&0&1  \end{bmatrix}+\begin{bmatrix}  0\\  1\\  0\\  -1  \end{bmatrix}(c+d)\begin{bmatrix}  1&0&0&0  \end{bmatrix}\\  &+\begin{bmatrix}  0\\  0\\  1\\  1  \end{bmatrix}(a)\begin{bmatrix}  0&0&1&-1  \end{bmatrix}+\begin{bmatrix}  1\\  1\\  0\\  0  \end{bmatrix}(d)\begin{bmatrix}  -1&1&0&0  \end{bmatrix}\\  &+\begin{bmatrix}  -1\\  0\\  1\\  0  \end{bmatrix}(a+b)\begin{bmatrix}  0&0&0&1  \end{bmatrix}+\begin{bmatrix}  0\\  0\\  0\\  1  \end{bmatrix}(c-a)\begin{bmatrix}  1&0&1&0  \end{bmatrix}\\  &+\begin{bmatrix}  1\\  0\\  0\\  0  \end{bmatrix}(b-d)\begin{bmatrix}  0&1&0&1  \end{bmatrix}  \end{aligned}$ 

- $\begin{bmatrix}  a&b&0&0\\  c&d&0&0\\  0&0&a&b\\  0&0&c&d  \end{bmatrix}$ 乘上$\begin{bmatrix}  p\\  r\\  q\\  s  \end{bmatrix}$ 
  - $\begin{aligned}  \begin{bmatrix}  w\\  y\\  x\\  z  \end{bmatrix}&=\begin{bmatrix}  1\\  0\\  0\\  1  \end{bmatrix}(a+d)(p+s)+\begin{bmatrix}  0\\  1\\  0\\  -1  \end{bmatrix}(c+d)(p)\\  &+\begin{bmatrix}  0\\  0\\  1\\  1  \end{bmatrix}(a)(q-s)+\begin{bmatrix}  1\\  1\\  0\\  0  \end{bmatrix}(d)(-p+r)\\  &+\begin{bmatrix}  -1\\  0\\  1\\  0  \end{bmatrix}(a+b)(s)+\begin{bmatrix}  0\\  0\\  0\\  1  \end{bmatrix}(c-a)(p+q)\\  &+\begin{bmatrix}  1\\  0\\  0\\  0  \end{bmatrix}(b-d)(r+s)  \end{aligned}$
  - $\begin{bmatrix}  a& b&  c&  d  \end{bmatrix}$ 代 $\begin{bmatrix}  A_{11}&  A_{12}&  A_{21}&  A_{22}  \end{bmatrix}$
  - $\begin{bmatrix}  p& q&  r&  s  \end{bmatrix}$ 代 $\begin{bmatrix}  B_{11}&  B_{12}&  B_{21}&  B_{22}  \end{bmatrix}$
  - $\begin{bmatrix}  w& x&  y&  z  \end{bmatrix}$ 代 $\begin{bmatrix}  C_{11}&  C_{12}&  C_{21}&  C_{22}  \end{bmatrix}$
  - 即為「Strassen 演算法」



# 補充例題



Example（100 交通大學資料結構與演算法）

- Let d and k be two non-negative integers, where k > d ≧ 0.
- A dk-binary sequence is a binary sequence that satisfies the following two constraints：
  - (a) d constraint：two 1's are separated by a run of consecutive 0 of length at least d
  - (b) k constraint：any run of consecutive 0's is of length at most k
- For example, 010010001001 is a dk-binary sequence of length 12 width d=0 and k=3.
- Let n indicate the length of dk-binary sequence

(47) Given n=6, d=0, k=6, then how many dk-binary sequences are there?

- （A）8
- （B）16
- **（C）32**
- （D）64
- （E）128

由 0、1 組成長度為六的字串個數 $\Rightarrow 2^6$

(48) Given n=10, d=4, k=5, then how many dk-binary sequences are there?

- （A）8
- （B）6
- （C）12
- （D）10
- （E）11

（參考：[Re: [理工] [DS&algo] 100交大](www.ptt.cc/bbs/Grad-ProbAsk/M.1329321054.A.ECB.html)）

**在序列中有 0 個 1**

- 不可能發生

**在序列中有 1 個 1**

- 0000010000
- 0000100000

**在序列中有 2 個 1**（兩個 1 **最多**只能將序列切成 3 段）

> 轉換題目成整數分割
>
> - 字串長度為 10
> - 兩個位子置 1
> - 剩下 8 個位子置 0
>
> - 必須有連續 4 個零或 5 個零出現

- 在序列中有連續 4 個 0（必有連續 4 個 0 出現，則必有一個 4 在此整數分割）
  - 8=4+**4'**
    - 序列：0000100001
  - 8=4+3+1
    - 序列：0001000010
  - 8=4+2+2
    - 序列：0010000100
  - 8=4+1+3
    - 序列：0100001000
  - 8=**4'**+4
    - 序列：1000010000
- 有連續5個零的狀況
  - 8=5+3
    - 序列：0001000001
  - 8=5+2+1
    - 序列：0010000010
  - 8=5+1+2
    - 序列：0100000100
  - 8=3+5
    - 序列：1000001000 

(49) Given n=14, d=1, k=14, then how many dk-binary sequences are there?

- （A）81 
- （B）160 
- （C）821 
- **（D）987** 
- （E）1024  

由 0、1 組成長度為十四且 1 不得連續出現的字串個數

- $a_n = a_{n-1}+a_{n-2}$
- $a_1 = 2、a_2 = 3$
- $a_{14} = 987$

