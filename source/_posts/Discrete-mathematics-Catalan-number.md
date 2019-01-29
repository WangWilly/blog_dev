title: Discrete mathematics - Catalan number
author: Willy Wang (willywangkaa)
tags:
  - Catalan Number
categories:
  - Discrete Mathematics
date: 2019-01-24 20:29:00
---
# Discrete mathematics - Catalan number



> 給定兩個生成函數：
>
> - $A(x) = \sum_{n = 0}^\infty a_nx^n$
> - $B(x) = \sum_{n = 0}^\infty b_nx^n$
>
> 則：
>
> - $A(x)\pm B(x) = \sum_{n = 0}^\infty(a_n+b_n)x^n$
> - $\begin{aligned} A(x) \times B(x) = (a_0+a_1x+a_2x^2+\ldots)\times(b_0+b_1x+b_2x^2+\ldots) 
>   \\ \Rightarrow (a_0b_0) + (a_0b_1+a_1b_0)x + (a_2b_0+a_1b_1+a_0b_2)x^2+\ldots \end{aligned}$



定義

- $c_n = a_n \otimes b_n = a_0b_n+a_1b_{n-1}+\ldots+a_nb_0$
  - $a_n、b_n$ 為兩個遞迴函數
- 稱 $c_n$ 為 $a_n、b_n$ 的「Convalution」
  - $c_n = a_n \otimes b_n \Rightarrow C(x) = A(x)\times B(x)$
  - $c_n = a_n \otimes a_n \Rightarrow C(x) = A(x)\times A(x) = A(x)^2$



## 「生成函數」推導



令 Catalan number 等於
$$
\left\{\begin{matrix} a_n = a_0a_{n-1} + a_1a_{n-2} + \ldots + a_{n-1}a_0 , n\geq 1 \\ a_0 = 1, a_1 = a_0a_0  = 1 \end{matrix}\right.
$$
以「生成函數」求此遞迴函數的顯式

1. 令 $A(x) = \sum_{n = 0}^\infty a_n x^n$

   - 因為由上式可以知道此遞迴**從 n ≧ 1開始有意義**

   - $\sum_{n = 0}^\infty a_n x^n - a_0 = \sum_{n = 1}^\infty(a_0a_{n-1}+a_1a_{n-2}+\ldots +a_{n-1}a_0)x^n$
     - $\sum_{n = 1}^\infty(a_0a_{n-1}+a_1a_{n-2}+\ldots +a_{n-1}a_0)x^n \\ \Rightarrow x\cdot \sum_{n = 1}^\infty(a_0a_{n-1}+a_1a_{n-2}+\ldots +a_{n-1}a_0)x^{n-1} = x\cdot A(x)^2$
   - $A(x) - a_0 = x\cdot A(x)^2 \\ \Rightarrow xA(x)^2- A(x) +1 = 0$
2. $A(x) = \frac{1\pm \sqrt{1-4x}}{2x}$

   - $(1-4x)^\frac12 = \sum_{r = 0}^\infty \binom{\frac12}{r}(-4x)^r$
     - $\sum_{r = 0}^\infty \binom{\frac12}{r}(-4x)^r = \sum_{n = 0}^\infty \frac{\frac12(\frac12 -1)\ldots (\frac12-r+1)}{r!}(-1)^r4^rx^r$
     - $\Rightarrow \sum_{r = 0}^\infty \frac{\frac12(-\frac12)(-\frac32)\ldots (-\frac{2r-3}{2})}{r!}(-1)^r4^rx^r$
     - $\Rightarrow \sum_{r = 0}^\infty (-1)^{r-1}(\frac12)^r\frac{1(1)(3)\ldots (2r-3)}{r!}(-1)^r4^rx^r$
     - $\Rightarrow -\sum_{r = 0}^\infty \frac{(1)(3)\ldots (2r-3)}{r!}2^rx^r$
     - $\Rightarrow \sum_{r = 0}^\infty \frac{(1)(2)(3)\ldots (2r-3)(2r-2)(2r-1)(2r)}{r!\cdot2^r\cdot r!\cdot(2r-1)}2^rx^r$
     - $\Rightarrow -\sum_{r = 0}^\infty \frac{(2r)!}{r!\cdot r! (2r-1)}x^r$
     - $\Rightarrow -\sum_{r = 0}^\infty \frac{1}{2r-1}\binom{2r}{r} x^r$
3. 因為 A(x) 會有兩個解，但是因為欲求之解不可能有負數情形，所以**取其正解**
  - $A(x) = \frac{1-\sqrt{1-4x}}{2x} = \frac{1+\sum_{r = 0}^\infty \frac{1}{2r-1}\binom{2r}{r} x^r }{2x}$
    - $\Rightarrow \frac12 \sum_{r = 1}^\infty \frac1{2r-1}\binom{2r}{r}x^{r-1}$，取 $x^n$ 的係數
    - 則 r = n+1，$\Rightarrow \frac12 \cdot \frac1{2(n+1)-1} \binom{2(n+1)}{(n+1)}x^n$
    - $\Rightarrow a_n = \frac{1}{2(2n+1)}\cdot\frac{(2n+2)\times(2n+1)}{(n+1)\times(n+1)}\cdot\binom{2n}{n} = \frac1{n+1}\binom{2n}{n}$
4. Catalan number 為 $\frac1{n+1}\binom{2n}{n}$ 



Example

求取 n 個節點的「Binary ordered tree」個數？

![1548332494667](\willywangkaa\images\1548332494667.png)

，k ≧ 0

$\Rightarrow\left\{\begin{matrix} a_n = a_0a_{n-1} + a_1a_{n-2} + \ldots + a_{n-1}a_0 , n\geq 1 \\ a_0 = 1 \end{matrix}\right. \\ \Rightarrow a_n = c_n = \frac{1}{n+1}\binom{2n}{n}$

> 變形
>
> - $\left\{\begin{matrix} a_n = a_0a_{n-1} + a_1a_{n-2} + \ldots + a_{n-1}a_0 , n\geq 1 \\ a_0 = 1 \end{matrix}\right.​$
>   - $a_n = c_n = \frac{1}{n+1}\binom{2n}{n}$
> - $\left\{\begin{matrix} a_n = a_1a_{n-1} + a_2a_{n-2} + \ldots + a_{n-1}a_1 , n\geq 1 \\ a_1 = 1 \end{matrix}\right.$
>   - $a_n = c_{n-1} = \frac{1}{n}\binom{2(n-1)}{n-1}$
> - $\left\{\begin{matrix} a_n = a_2a_{n-1} + a_3a_{n-2} + \ldots + a_{n-1}a_2 , n\geq 1 \\ a_2 = 1 \end{matrix}\right.$
>   - $a_n = c_{n-2} = \frac{1}{n+1}\binom{2(n-2)}{n-2}$
>
> Example
>
> n 個變數 $x_1, x_2, \ldots, x_n$ 之合法的括號數有幾個？
>
> ![1548332797656](\willywangkaa\images\1548332797656.png)
>
> ，k ≧ 1
>
> $\left\{\begin{matrix} a_n = a_1a_{n-1} + a_2a_{n-2} + \ldots + a_{n-1}a_1 , n\geq 1 \\ a_1 = 1 \end{matrix}\right. \Rightarrow c_{n-1} = \frac{1}{n}\binom{2(n-1)}{n-1}$



## 「組合證明」推導



### 前導



令 Catalan number 等於
$$
\left\{\begin{matrix} a_n = a_0a_{n-1} + a_1a_{n-2} + \ldots + a_{n-1}a_0 , n\geq 1 \\ a_0 = 1, a_1 = a_0a_0  = 1 \end{matrix}\right.
$$
則我們可以將此遞迴視為一個**「合法路徑問題」**

- 在一個二維空間下，從原點 (0, 0) 只能走下面兩種步伐至 (m, n)
  - 步伐 R：(x, y) → (x+1, y)
  - 步伐 U：(x, y) → (x, y+1)
- 並且在路徑中，不能發生 U 大於 R 的情形
  - 違法：(0,0)→R→R→U→U**→U**→R→...→(m,n)



- 首先我們先將問題簡化成「Catalan number」可以解釋的問題，再來推廣
  - 在一個二維空間下，從原點 (0, 0) 只能走下面兩種步伐至 **(n, n)**
    - 步伐 R：(x, y) → (x+1, y)
    - 步伐 U：(x, y) → (x, y+1)
  - 並且在路徑中，不能發生 U 大於 R 的情形

> 討論
>
> - 若問題再進一步簡化成「自點(0, 0) 只能以規定的兩種步伐至 **(0, 0)**」
>   - 令該方法數為 $a_0$，並且其方法數視為 1
> - 而當問題為「自點(0, 0) 只能以規定的兩種步伐至 **(1, 1)**」時
>   - 只有唯一種方法，就是 (0,0)→R→U→(1,1)
>   - 而我們更可以進一步將此看成
>     -  $a_1$ = 「自(0,0)至(0,0)的方法數」×（唯一的走法：...→R→U→...）×「自(1,1)至(1,1)的方法數」
>     - $a_1 = a_0\times1\times a_0 = a_0a_0$

所以，在一個二維空間下，從原點 (0, 0) 只能以規定兩種步伐至 **(n, n)**的方法數為

- $a_n = a_0a_{n-1} + a_1a_{n-2} + \ldots + a_{n-1}a_0$，為以下方法數組成
  - (「自(0,0)至(0,0)的方法數」×（唯一的走法：...→R→U→...）×「自(1,1)至(n,n)的方法數」) +
  - (「自(0,0)至(1,1)的方法數」×（唯一的走法：...→R→U→...）×「自(2,2)至(n,n)的方法數」) +
  - ....
  - (「自(0,0)至(n-1,n-1)的方法數」×（唯一的走法：...→R→U→...）×「自(n,n)至(n,n)的方法數」)

> 由上方討論可以得知**簡化的「合法路徑問題」**與 **n 個節點的「Binary ordered tree」個數問題**等價



### 組合證明



一樣是上述的**簡化的「合法路徑問題」**，由下圖來討論

![1548319123283](\willywangkaa\images\1548319123283.png)

自 (0, 0) 至 (16, 16) 且不得超過**中間雙線的部分（可觸及）**

合法示意圖：

![1548319440435](\willywangkaa\images\1548319440435.png)

非法示意圖：

![1548319549796](\willywangkaa\images\1548319549796.png)

> 每個違法的路徑，必定會和下方紅色雙斜線接觸
>
> ![1548320935256](\willywangkaa\images\1548320935256.png)
>
> 將**「開始違法的座標」之後的路徑**對紅色雙斜線做對稱化，則最後到達的座標必定在 (15,17)，而這種轉換有幾個特性：
>
> - 1-1 對應
> - 可逆
>   - 若再一次對稱回來，必定會對應到一個**違法的路徑**自(0,0)至(16,16)
>
> 所以自(0,0)至(15,17)的每種路徑，皆會對應到一個違法路徑
>
> - 合法路徑方法數
>   - $\binom{32}{16} - \binom{32}{15} = \frac{1}{16+1}\binom{2\cdot16}{16}$ 為「Catalan number」

- 所以從(0, 0)至(n, n)的「合法路徑為」
  - $\binom{2n}{n}-\binom{2n}{n-1}$ 



Example 

有一場選舉，兩個候選人 A、B，假設知道 A 的票數一定比 B 多，假設 A 的總得票數為 p ，B 的總得票數為 q，請問有幾種開票的方法可保證每個當下 A 的得票數會大於 B 當下的得票數？

> 分析
>
> 有別於上面的題目，此處 A 每個當下的得票數絕對會比 B 每個當下的得票數還多
>
> - **不合法**的開票方法：A→A→B**→B**→...

![1548330759722](\willywangkaa\images\1548330759722.png)

假設 A 得到 15 票，B 得到 11 票，則上圖中綠色為合法的開票方法，紅色虛線為不合法的開票方法，但是這樣討論之下，綠色的開票方法在尚未開票時其實也觸碰了紅色雙斜線，然而為何還是正確的開票方法，因為我們在尚未開票時不討論誰贏的比較多，所以我們可以將起點平移到 (1, 0) 處，代表第一章開出來的票一定是 A 得到，且亦不牴觸我們方才的假設

![1548331206829](\willywangkaa\images\1548331206829.png)

則**每個非法路徑**會唯一對應到「自 (1,0) 至 (11,15) 的開票方法」

![1548332141506](\willywangkaa\images\1548332141506.png)

- 合法的開票方法為
  - $\binom{(15-1)+11}{15-1} - \binom{(11-1)+15}{11-1} = \binom{25}{14}-\binom{25}{10} = 1188640$

- 原本的問題之**合法開票數**為
  - $\binom{(p-1)+q}{p-1} - \binom{(q-1)+p}{q-1}$



