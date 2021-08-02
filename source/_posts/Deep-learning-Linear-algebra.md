title: Deep learning - Linear algebra
author: Willy Wang (willywangkaa)
tags: []
categories:
  - Deep learning
date: 2019-04-19 21:13:00
---
# Deep learning - Linear algebra



## 矩陣的正定性（Positive definiteness）

> **Eigen decomposition** 矩陣的特徵值分解
>
> - 一個矩陣 $A$ 可作特徵值分解
>   - 若且唯若矩陣 $A$ 的「代數重根數」（特徵根）等於其「幾何重根數」（特徵向量）
> - 矩陣的特徵值分解**不一定唯一**
>   - 當有兩個或多個特徵向量屬於同一個特徵值時
>   - 取其原始「特徵向量組」之拓展空間（Span；亦為該 $\lambda$ 的特徵空間）的其他「正交向量組」亦為該 $\lambda$ 的「特徵向量組」
>
>
>
> - $A \in \mathbf R^{n\times n} \quad A = A^T$
>   - 則其可被分解成 $Q\mathrm{diag}(\lambda)Q^T$
>   - $\lambda \in \mathbf R^n$ 為 $A$ 的特徵根
>   - $Q = [v^{(1)}, \ldots, v^{(n)}]$ 為正交矩陣（Orthogonal matrix），其中每一行（Column）為對應 $\lambda$ 的特徵向量
>
> **證明對於相異的特徵根之特徵向量相互正交**
>
> 矩陣 $A$ 在可對角化的情況下
>
> 假設 $A : n\times n, A^T = A, \lambda_1\neq\lambda_2\in\lambda(A)$
>
> 1. 假設 $x_1, x_2$ 為 $A$ 相對於 $\lambda_1, \lambda_2$ 的特徵向量
>    - 則 $Ax_1 = \lambda_1 x_1, Ax_2 = \lambda_2x_2$
> 2. $\lambda_1＜x_1, x_2＞ = ＜\lambda_1x_1, x_2＞ \\  = ＜Ax_1, x_2＞ = ＜x_1, A^Hx_2＞ = ＜x_1, A^Tx_2＞ \\ = ＜x_1, Ax_2＞ = ＜x_1, \lambda_2x_2＞ = \bar{\lambda_2}＜x_1, x_2＞ = \\ \lambda_2＜x_1, x_2＞$
>    - 因為 $\lambda_1 \neq \lambda_2$ 所以 $＜x_1, x_2＞ = 0$
> 3. 所以 $x_1, x_2$ 正交
>
> 而對應於同一特徵根的特徵向量可以作「Gram-Schimidt」正交化，故可以找出一組正交基底可對矩陣 $A$ 作對角化
>
>
>
> - $A \in \mathbf R^{n\times n} \quad A = A^T \Rightarrow A = Q\mathrm{diag}(\lambda)Q^T$
>   - $[x_0', x_1'] = A [x_0,x_1]$
>   - 因為 $Q = [v^{(1)}, \ldots, v^{(n)}]$ 為正交矩陣，因為其**鋼性**，所以對向量作用時，**不改變其向量的長度**
>     - 可以將 $A$ 視為「對該特徵向量 $v^{(i)}$ 之方向伸縮 $\lambda$ 倍」
>   - 令下圖的 $v^{(1)}、v^{(2)}$ 為 $A$ 的特徵向量
>     - 對該平面以 $Q^T = [v^{(1)},  v^{(2)}]^T$ 作用，則此平面系統會以 $v^{(1)},  v^{(2)}$ 向量組重新參考
>     - 接著對此平面以舉陣 $\mathrm{diag}(\lambda)$ 作用會伸縮 $v^{(1)},  v^{(2)}$ 方向 $\lambda_1, \lambda_2$ 倍
>     - 最後在對該平面以 $Q = [v^{(1)},  v^{(2)}]$ 作用，還原平面向量參考系統
>
> ![1553931898849](\willywangkaa\images\1553931898849.png)
>
> **Theorem**
>
> Rayleigh's quotient
>
> - 給定一對稱矩陣 $A\in \mathbf R^{n\times n}$，對於所有向量 $x\in\mathbf R^{n}$
>   - $\lambda_{min} \leq \frac{x^TAx}{x^Tx}\leq\lambda_{max}$
>   - $\lambda_{min}、\lambda_{max}$ 分別為 $A$ 最小與最大的特徵向量
>
> *（直觀想法）*
>
> 以 $A$ 矩陣對上圖平面作用時，對所有在該平面向量 $x$ 最長只會伸縮 $\lambda_{max}$ 倍，最短只會伸縮 $\lambda_{min}$ 倍



- $A$ 有「半正定性」（Positive semidifinite；標記為 $A \succeq O$）
  - 若且唯若其特徵值皆非負
  - $x^TAx \geq 0 , \forall x$
- $A$ 有「正定性」（Positive definite；標記為 $A \succ O$）
  - 若且唯若其特徵直皆大於 0
  - 任意非零向量 $x$ ，$x^TAx \neq 0$



## Quadretic function

- 一個二次函數（Qusdretic function）
  - 若且唯若可寫成 $f(x) = \frac12x^TAx-b^Tx +c$
  - 上述 $A$ 為**對稱矩陣**

**Example**

$f(x)= x^2+x+1 = \frac12[x]^T[2][x]-[-1]^Tx+c$



- $x^TAx$ 稱作**「二次式」（Quadertic form）**
  - 觀察其「正定性質」以判斷其函數的「凹性」
- 令 $x = \begin{bmatrix} x_1\\ x_2\end{bmatrix}$ 則 $f(x) = x^TAx$ 可作成下圖
  - $x_1、x_2$ 之平面為該函數的輸入（Input）
  - $f(x)$ 為該函數的輸出（Output）
- （a）當 $A$ 為「正定矩陣」
  - 如果此時 $x \in \mathbf R$，代表 $A$ 為 x 的係數（$xAx = Ax^2$；為一元二次方程式）
  - 若 $A > 0$ 則此二次式的圖會上凹（碗型）
- （b）當 $A$ 為「負定矩陣」
  -  如果此時 $x \in \mathbf R$，代表 $A$ 為 x 的係數（$xAx = Ax^2$；為一元二次方程式）
  - 若 $A < 0$ 則此二次式的圖會下凹
- （c）當 $A$ 為「半正定矩陣」（奇異矩陣；Sigular matrix）
  - 代表在某些特徵向量的**方向**之下其特徵值為 0 ，則**在該方向的輸入值輸出必為 0**（上凹峽谷型）
- （d）當 $A$ 有「未定矩陣」（Indefinite matrix）
  - 其特徵值包含大於 0 與小於 0 之值
  - 某些特徵向量方向上凹，某些特徵向量下凹

![1553933848943](\willywangkaa\images\1553933848943.png)



## Singular value decomposition



- 所有 m×n 矩陣 $A \in \mathbf R^{m\times n}$ 可作「奇異值分解」（Singular value decomposition）
  - $A = UDV^T$
  - $U \in \mathbf R^{m\times m}, D \in \mathbf R^{m\times n}, V \in \mathbf R^{n\times n}$
  - $U, V$ 為正交矩陣，各自的行向量分別稱為**「左奇異向量」（Left-singular vector）與「右奇異向量」（Right-singular vector）**
  - 在 $D$ 矩陣上對角線元素稱為「奇異值」（Singular value）

- $A$ 的「左奇異向量」為 $AA^T$ 的「特徵向量」
- $A$ 的「左奇異向量」為 $A^TA$ 的「特徵向量」
- $A$ 的所有非零「奇異值」為 $AA^T$ （或 $A^TA$）的**「非零特徵值」**



### Moore-Penrose pseudoinverse（虛反矩陣）

> - 反矩陣只定義在方陣上

- 欲對矩陣 $A \in \mathbf R^{m\times n}$ 找到一個「左反矩陣」$B \in \mathbf R^{n\times m}$，對線性方程組 $Ax = y$ 兩側左乘 $B$ 以解出 $x = By$
  - 若 m > n
    - 則可能因為 $y \notin \mathrm{col}(A)$，所以 $B$ 矩陣不存在
  - 若 m < n
    - 則可能會有多個符合的 $B$ 矩陣
- 若 $B$ 為**「Moore-Penrose pseudoinverse」**$A^+$
  - 當 m = n 且 $A^{-1}$ 存在時，$A^+$ 退化成 $A^{-1}$
  - 當 m > n 且 $\hat x = A^+y$，則 $\forall x, ∥Ax-y∥_2 \geq ∥A\hat x-y∥_2$
    - 因為 $A$ 為 n 維映射至 m 維所對應的矩陣，則有可能向量 $y \notin Ax, \forall x$，目標找到與其最相似的向量 $A\hat x$
    - $A\hat x$ 為 $\mathrm{col}(A)$ 中距離 $y$  最短的向量
  - 當 m < n 且 $\hat x = A^+y$，則 $\forall x \in ｛x｜Ax = y｝, ∥x∥ \geq ∥\hat x∥$
    - $\hat x$ 為 $Ax = y$ 的最小長度解（Minimal Eudlidean norm solution）

- 「Moore-Penrose psedoinverse」的定義
  - $A^+ = \lim_{a \to 0}(A^TA+\alpha I_n)^{-1}+A^T$
- 在實作上是以 $A^+ = VD^+U^T$ 計算，其 $UDV^T = A$
  - $D^+ \in \mathbf R^{n\times m}$ 為將 $D$ 矩陣中的非零對角元素**取其乘法反元素**，再對其**作轉置**



## Trace（矩陣的跡）

- $\mathrm{tr}(A) = \Sigma_i A_{i, i}$
- $\mathrm{tr}(A) = \mathrm{tr}(A^T)$
- $∥A∥^2_F = \mathrm{tr}(AA^T) = \mathrm{tr}(A^TA)$
  - （矩陣的長度）
- $\mathrm{tr}(ABC) = \mathrm{tr}(BCA) = \mathrm{tr}(CAB)$
  - 因為 $\mathrm{tr}(AB) = \mathrm{tr}(BA)$ （可暴力正明其正確性）則 $\mathrm{tr}(A(BC)) = \mathrm{tr}((BC)A)$



## Determinant（矩陣的行列式）

- 行列式 $\det(\cdot)$ 為一函數將方陣 $A \in \mathbf R^{n \times n}$ 映射至**實數域**
  - $\det(A) = \Sigma_i(-1)^{i+1}A_{1,i}\det(A_{-1,-i})$**（此亦為降階求行列式的定義）**
    - $A_{-1,-i}$ 為減去**第 1 列**與**第 i 行**的 (n-1)×(n-1) **矩陣**
    - $A_{1,i}$ 為第 1 列第 i 行的**元素**
- $\det(A^T) = \det(A)$
- $\det(A^{-1}) = \frac 1{\det(A)}$
- $\det(AB) = \det(A)\det(B)$
- $\det(A) = \prod_i \lambda_i$
  - 當一個矩陣可作「特徵值分解」時，可藉由特徵值 $\lambda$ 發現矩陣 $A$ 所對應的函數 $T$ 作用後，在其特徵向量 $x$ 的「方向」上伸縮的倍率
  -  由此可見 $A$ 的行列式為這些倍率的「總乘」
  - 其幾何意義可視為：當被作用的輸入向量可形成一個「超單位矩體」（Image of the unit square）時，則 $T$ 的行列值為「$T$ 作用之後而形成『超矩體』之**體積**」

**Example**

- 令 $A = \begin{bmatrix} a & b \\ c & d\end{bmatrix}$ ，有兩單位向量 $e_1 = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, e_2 = \begin{bmatrix} 0 \\ 1 \end{bmatrix}$
  - 經過矩陣 $A$ 作用後 $Ae_1 = \begin{bmatrix} a \\ c \end{bmatrix} \\ Ae_2 = \begin{bmatrix} b \\ d \end{bmatrix}$
  - 下圖中的平行四邊形為標準基底向量經過 $A$ 矩陣後而成，行列式值為其面積

![1553944007423](\willywangkaa\images\1553944007423.png)

- 一個函數 $T$ 的行列式值比「特徵值分解」更能簡潔表達其函數的**伸縮性**
- 若 $\det(A) = 0$，代表經過此函數作用後會比原本維度至少**少一個維度以上**
  - 由高維度的空間映射至低維度的空間
    - 該函數不能取其「反函數」
  - 在特徵值上，也就是說有一特徵值等於 0，其對應的特徵向量的方向伸縮為 0，其「超體」亦退化成「超面」導致『超矩體』之**體積**退化成 0
- 若 $\det(A) = 0$，代表經過此函數 $T$ 的轉換後，其『超矩體』之**體積**不變