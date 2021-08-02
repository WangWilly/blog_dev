title: Deep learning - Probability and Information theories II
author: Willy Wang (willywangkaa)
tags:
  - Probability
  - Information theories
categories:
  - Deep learning
date: 2019-04-19 21:17:00
---
# Deep learning - Probability and Information theories II



## Principal components analysis（PCA；主成份分析）

> PCA 視覺化：[Principal Component Analysis](http://setosa.io/ev/principal-component-analysis/?fbclid=IwAR0cSmfJGjg-IFsKNuU1h9ECUmOcE81J2naJaIjObHnHXlj-4mqWTNVaJwA)

- 給定一組資料點 $\mathbf X = \left\{x^{(i)}\right\}_{i = 1}^{N}$
  - 每個資料點 $x^{(i)} \in \mathbf R^D$ ，意旨每個資料點都含有 $D$ 個特性（維度）
- 欲對資料 $\mathbf X$ 的維度作**有損壓縮（Lossily compression）**
  - 找到一個函數使得 $f(x^{(i)}) = z^{(i)} \in \mathbf R^K, K<D$
  - 在有損壓縮之下，如何減少原始資料 $\mathbf X$ 的**特徵損失最少化**

> - 假設有一組資料變數包含身高與體重，欲求一個向量來投影這些資料點
>
> ![1554452077532](\willywangkaa\images\1554452077532.png)
>
> 先將這些資料作「正規化」（Normalization）
>
> ![1554452186787](\willywangkaa\images\1554452186787.png)
>
> 先撇開中間橢圓與其兩軸，正規化的資料將原本絕對關係的資料轉換成相對關係的資料以避免資料的極端情形，但是仍無法對此資料進行更多著墨，所以欲求另外一組基底以標示這些資料點來更直覺的分析資料
>
> - 取得一組基底來標示這些正規化的資料
>   - 保留更多原始資料的信息或特徵，在減少某些特定的維度時，資料的特性還得以保留
> - 對於基底中某一向量
>   - 資料點投影在此向量上的分量可視為轉換後「變異量」的重要參考因素
>
> 在一組六個點的正規資料下，有兩個候選的基底向量 $v, v'$ 分別如下
>
> ![1554453026001](\willywangkaa\images\1554453026001.png)
>
> 比較後可以發現以左側為基底的情形下，其投影分量的差距會比以右側為基底的狀況還大（其變異數的值亦會比較大）
>
> ![1554453264290](\willywangkaa\images\1554453264290.png)
>
> - 上述何者比較適合當作新的基底
>   - 每個點對於新基底的距離最短
>     - $\mathrm{minimize_v} \sum_{n = 1}^6 ∥\mathrm{x_n-(v^Tx_n)v}∥^2$
>     - 使上式最小化，則要最大化 $\mathrm{(v^Tx_n)}$，整體來說需要最大化 $\sum_{n = 1}^6 \mathrm{(v^Tx_n)}$
>   - 避免拉扯原始資料的間距
>     - $\left \| \mathrm v \right \| = 1$
> - 綜合上述，可將問題轉換成
>   - $\mathrm{maximize_v \frac 1n\sum_{n = 1}^6(v^Tx_n)^2}, \left\|v \right\|=1 \\ \equiv \mathrm{maximize_v \frac 1n\sum_{n = 1}^6(v^Tx_n)(x_n^Tv)}, \left\|v \right\|=1\\ \equiv \mathrm{maximize_v v^T \frac 1n\sum_{n = 1}^6x_nx_n^Tv}, \left\|v \right\|=1 \\ \equiv \mathrm{maximize_v v^T Cv}, \left\|v \right\|=1$
>   - 最大化 $\mathrm{(v^Tx_n)}$ 意旨「最大化沿著基底向量 $v$ 的變異數」
>
> ![1554462474803](\willywangkaa\images\1554462474803.png)
>
>

- 令 $x^{(i)}$ 為 i.i.d. 隨機變數 $\mathrm x$ 的樣本
- 讓有損壓縮函數 $f$ 為線性函數
  - $f(\mathrm {x) = W^Tx, W\in}\mathbf R^{D\times K}$
- 主成份分析的目的就是找到 $K$ 個正範向量（Orthonormal） $W = [w^{(1)},\ldots,w^{(K)}]$
  - 使轉換後的樣本 $\mathrm z = W^T \mathrm x$ 不會損失太多原本的性質（亦可視為座標軸 $\mathbf R^D \to \mathbf R^K$ 轉換）
    - 讓轉換後的**衍隨機變數** $\mathrm z_j = w^{(j)T}\mathrm x$ 的變異數 $\mathrm {Var(z_j)}$ 最大化（原始資料沿著基底向量 $w^{(j)}$ 方向的隨機變數）
  - $w^{(1)}, \ldots,w^{(K)}$ 稱作「主成份軸」（Principle components）
- 為何主成份軸 $w^{(1)},\ldots,w^{(K)}$ 需要相互正交？
  - 避免新的衍隨機變數所保留的資訊與其他衍隨機變數所保留的資訊重疊，降低效率
- 為何主成份軸 $w^{(1)},\ldots,w^{(K)}$ 長度需要為 1？
  - 避免該向量的長度影響原始資料的間距讓新的衍隨機變數對應的變異數有變動



### Solving W

**Example**

- 令壓縮後的維度簡化為 1（$K = 1$）
- 如何求出新的衍隨機變數其變異數 $\mathrm{Var(z_1)}$？
  - 新的衍隨機變數是經由 $\mathrm z_1 = w^{(1)T}\mathrm x$  求取
    - 其隨機變數 $\sigma_{\mathrm z_1}^2 = w^{(1)T}\Sigma_{\mathrm x}w^{(1)}$
  -  $\Sigma_{\mathrm x}$ 可經由已知的樣本（**已正規化**）去猜想
    - $\hat\Sigma_{\mathrm x} = \frac 1nX^TX$
- 欲解的最佳化問題
  - $\arg \underset{w^{(1)}\in\mathbf R^D}{\max} w^{(1)T}X^TXw^{(1)}, \mathrm{subject \; to }\left\|w^{(1)}\right\| = 1$
  - 在 $\left\|w^{(1)}\right\| = 1$ 的狀況下找到一個參數（$w^{(1)}\in\mathbf R^D$）最大化函數（$w^{(1)T}X^TXw^{(1)}$）
- Designed matrix $X$
  - $X^TX$ 為對稱矩陣，因此可被**正交對角化**
  - 由於「Rayleigh's quotient」，對於每個對稱矩陣
    - $\forall v, \lambda_{\min}\leq \frac{v^TX^TXv}{v^Tv}\leq\lambda_{\max}$
  - 當 $\frac{v^TX^TXv}{v^Tv}=\lambda_{\max}$ 時，$v$ 為對應 $\lambda_{\max}$ 的特徵向量

**Example（Cont.）**

- 令壓縮後的維度簡化為 2（$K = 2$）
- 繼續找到最佳第二個主成份軸 $w^{(2)}$
  - $\arg \underset{w^{(2)}\in\mathbf R^D}{\max} w^{(2)T}X^TXw^{(2)}, \mathrm{subject \; to }\left\|w^{(2)}\right\| = 1$
  - $w^{(2)T}w^{(1)} = 0$
- 經由「Rayleigh's quotient」可知，次大的特徵值所對應的特徵向量就是欲求取的 $w^{(2)}$

> 由此可知，$K>1, w^{(1)}, \ldots, w^{(K)}$ 為 $X^TX$ 前 $K$ 大的特徵值所對應的特徵向量（由歸納法可知）



## Technical details of continuous random variables

> 連續型隨機變數與機率密度函數的深度理解需要用到「測度論」（Measure theory）的相關內容來擴展**機率論**
>
> - 連續型隨機向量 $\boldsymbol X$ 落在一個集合 $\mathbb S$ 中的機率
>   - 透過 $f_{\boldsymbol X}(\boldsymbol x)$ 對集合 $\mathbb S$ 積分得到
> - 在一些選擇之下可能引起悖論
>   - 給兩集合 $\mathbb S_1、\mathbb S_2 \ni f_{\boldsymbol X}(\boldsymbol x\in \mathbb S_1)+f_{\boldsymbol X}(\boldsymbol x\in \mathbb S_2)>1$
>   - 且 $\mathbb S_1 \cap \mathbb S_2 = \O$
>   - 這些集合通常是大量使用實數的無限精度來構造
>     - [碎形集合](https://zh.wikipedia.org/wiki/%E5%88%86%E5%BD%A2)
>     - 透過有理數相關集合的變換定義集合
> - 測度論提供一些集合的特徵，使計算該機率時不會遇到悖論
>
> - 測度論提供一種嚴格的方式**描述那些非常微小的點集**
>   - 稱為「零測度」（Measure zero）
>   - 可以認為零測度集合在「度量空間」中不佔有任何**體積**
>   - 在 $\mathbb R^2$ 空間中，一直線的測度為零，一多面體具有真正的測度
>   - 一個單獨的點其測度為零；「可數多個零測度集合的聯集」依然為零測度
> - **幾乎處處（Almost everywhere）**
>   - 某個性質是幾乎處處都成立
>     - **其性質除了一個測度為零的集合以外皆成立**
>   - 因為不成立的例外在該空間中只佔有極其微小的量
>   - 機率論中一些重要結果對於離散值成立但對於連續值只能是「幾乎處處」成立



### Sure and almost sure events

> **機率公理（Axioms of probability）**
>
> 給定任何**非空集合** $\Omega$ 為**樣本空間（Sample sapce）**，接著我們定義一個**函數** $\Pr$ 在上述樣本空間 $\Omega$ 的子集合 $\cal F$ 上
>
> 則我們稱此函數 $\Pr$ 為一個**機率測度（Probability measure）**，若該函數能同時滿足下面四條公理
>
> 1. 空集合 $\O$ 稱為**「不可能發生事件」（Imposisible event）**，此不可能發生的事件在樣本空間上的子集合中機率為 0，亦即 $\Pr(\O) = 0$
> 2. **（非負性質）**機率 $\Pr$ 為非負值，亦對任意事件 $\mathbb A$ 而言，$\Pr(\mathbb A)\geq0$
> 3. **（可數加法性質）**若 $A_1, A_2,\ldots$為兩兩互斥事件（Pairwise disjoint or mutuallly exclusive），$\forall n\neq m, A_n\cap A_m = \O$
>    - $\Pr(\bigcup_{n = 1}^\infty A_n) = \sum_{n = 1}^\infty \Pr(A_n)$
>    - 若事件本身互斥（如：丟一枚硬幣不可能**同時**出現正面與反面，稱「出現正面」與「出現反面」事件為互斥事件），則所有可能發生的事件機率為各自相加
> 4. 整個樣本空間的機率被稱作**確定事件（Sure event）**，此事件發生的機率為 1，$\Pr(\Omega) = 1$
>    - 若一個事件 $A\neq\Omega$ 但滿足 $\Pr(A) = 1$，則此事件 $A$ 為幾乎確定事件（Almost-sure event）
>
> 機率測度本質上為一個「函數」，考慮機率空間為 $(\Omega,\cal F, \Pr)$ 其機率測度為 $\Pr:\cal {F}\to [0,1]$，$\cal F$ 為 $\Omega$ 的子集合（$\cal F$ 稱為 $\sigma$-algebra，其中元素稱為事件）



-  一個連續型的隨機變數 $\mathrm x$，當 $\mathrm x = x$ 為任意值時 $\Pr(\mathrm x = x) = 0$
   -  那麼 $\mathrm x = x$ 事件會不會發生？
     -  **會**（機率等於 0 不代表其事件不會發生）
   -  $\Pr(\mathrm x \neq x) = 1$ 因為 $\mathrm x \neq x$ 事件不等於整個樣本空間 $\Omega$，所以此事件為**「幾近確定事件」（Almost-sure event）**

> **Example**（[原文](http://mathcentral.uregina.ca/QQ/database/QQ.09.06/h/ben1.html)）
>
> 在實驗的過程，所有可能的實驗結果 $\omega$ 組成之集合稱為「樣本空間」（Sample space）$\Omega$
>
> - 擲**一次**六面骰子
>   - 其樣本空間為｛1, 2, 3, 4, 5, 6｝
>
> 「事件」（Event）為樣本空間的**子集合**
>
> - 擲**一次**六面骰子
>   - 其中一個事件「大於 4 點」的集合為｛5, 6｝
>   - 上述事件的機率為一分數（Fraction）
>     - 分母為樣本空間的元素個數 6
>     - 分子為其事件的元素個數 2
>     - $機率(大於 \;4 \;點) = \frac 26 = \frac 13$
> - **對於所有事件**
>   - **0 ≦ 事件對應的機率 ≦ 1**
>
> 如果樣本空間從上述的有限集合轉換為**無限集合**，考慮下述問題
>
> 1. 在正整數 $\mathbb Z$ 中隨機挑一數，其大於 1 的機率？
> 2. 在正整數 $\mathbb Z$ 中隨機挑一數，其小於等於 1 的機率？
>
> 假設第二小題的答案為 $p$ ，則第一小題的答案為 $1-p$
>
> 然而第二小題無法求解，因為其樣本空間有無限多個元素，但是可以解下述簡化的第二小題
>
> - 在正整數｛1, 2, …, 100｝中隨機取一數，其小於等於 1 的機率？
>   - 機率為 $\frac1{100} = 10^{-2}$
>   - 因為第二小題的樣本空間遠比此樣本空間大，所以 $p$ 亦比 $10^{-2}$ 小
> - 在正整數｛1, 2, …, 1000｝中隨機取一數，其小於等於 1 的機率？
>   - 機率為 $\frac1{1000} = 10^{-3}$
>   - 因為第二小題的樣本空間遠比此樣本空間大，所以 $p$ 亦比 $10^{-3}$ 小
>
> 可知當樣本隨之增大，機率 $p$ 會隨之越來越小，由於 $p$ 為**機率**所以 $0 \leq p$；所以 $p$ 介於 $[0,10^{-\infty}]$ 中，其最好的可能取 $p = 0$ 所以第一小題的答案為 $1$
>
> 在正整數 $\mathbb Z$ 中隨機挑一數，**其幾近確認（Almost surely）大於 1，但卻不必然確認（Absolutely certainly）大於 1**





### Equality of random variables

> **Definition（Equality）**
>
> Two random variables $\mathrm x$ and $\mathrm y$ are **equal** iff thay maps the same events on unique sample space $\Omega$ to same values.
> $$
> \mathrm x(\omega) = \mathrm y(\omega), \forall \omega\in\Omega
> $$
> 隨機變數 $\mathrm x$ 與 $\mathrm y$ 為相等若且唯若在同一個樣本空間中 $\Omega$，其函數將每個事件 $\omega$ 映射的值相同
>
> **Example**
>
> - 擲一次兩個相互**量子糾纏**的六面骰子，一藍一紅
>   - 藍骰子無論擲出幾點紅骰子亦擲出幾點
> - 其擲出的結果標記為 $(藍骰子, 紅骰子)$
>   - 樣本空間為 $｛(1,1),(2,2),\ldots,(6,6)｝$
> - 令隨機變數 $\mathrm x$ 為藍骰子之點數，隨機變數 $\mathrm y$ 為紅骰子之點數
>   - $\mathrm x(i,j) = i$
>   - $\mathrm y(i,j) = j$
> - $\mathrm{x,y}$ 等價
>   - $\mathrm x(\omega) = \mathrm y(\omega), \forall \omega\in\Omega$



> **Definition（Almost sure equality）**
>
> Two random variables $\mathrm x$ and $\mathrm y$ are equal almost surely iff $\Pr(\mathrm x = \mathrm y) = 1$
>
> Two random variables *X* and *Y* are *equal almost surely* (denoted $\mathrm x\;\stackrel {a.s.}{=}\;\mathrm y$） if, and only if, the probability that they are different is zero：
>
> $\Pr (X\neq Y)=0$
>
> [Example of random variables XX and YY where X≠YX≠Y, but X=YX=Y almost surely](https://math.stackexchange.com/questions/980060/example-of-random-variables-x-and-y-where-x-neq-y-but-x-y-almost-sur)
>
> [Do two almost surely equal random variables necessarily have the same probability?](https://math.stackexchange.com/questions/1362292/do-two-almost-surely-equal-random-variables-necessarily-have-the-same-probabilit)
>
> [Almost surely equal random variables and expectation](https://math.stackexchange.com/questions/508212/almost-surely-equal-random-variables-and-expectation)



> **Definition（Equality in distribution）**
>
> Two random variables $\mathrm x$ and $\mathrm y$ are **equal in distribution** iff $\Pr(\mathrm x\leq a) = \Pr(\mathrm y\leq a)$ for all $a$.
>
> 給任意常數 $a$ 使得 $\Pr(\mathrm x\leq a) = \Pr(\mathrm y\leq a)$（擁有一樣的分佈函數 Distribution function），稱其為「Equality in distribution」，記作 $\mathrm x\overset{d}{=}\mathrm y$
>
> To be equal in distribution, random variables **need not be defined on the same probability space**. Two random variables having equal [moment generating functions](https://en.wikipedia.org/wiki/Moment_generating_function) have the same distribution. This provides, for example, a useful method of checking equality of certain functions of [independent, identically distributed (IID) random variables](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables). However, the moment generating function exists only for distributions that have a defined [Laplace transform](https://en.wikipedia.org/wiki/Laplace_transform).
>
> **Example**
>
> - 擲一次兩個六面骰子，一藍一紅
> - 其擲出的結果標記為 $(藍骰子, 紅骰子)$
>   - 樣本空間為 $｛(1,1),(1,2),\ldots,(6,6)｝$
> - 令隨機變數 $\mathrm x$ 為藍骰子之點數，隨機變數 $\mathrm y$ 為紅骰子之點數
>   - $\mathrm x(i,j) = i$
>   - $\mathrm y(i,j) = j$
> - $\mathrm{x,y}$ 有相同的分佈（Equal in ditribution）
>   - 其「機率質量函數」（p.m.f.）皆為 $\mathrm P(i) = \frac 16, i = 1,2,\ldots,6$
>   - 但是 $\mathrm x \neq \mathrm y$



- 「Equality in distribution」與「Almost sure equality」的**差異性**為何？
- 「Equality in distribution」代表其分佈相同，但是其分佈相同時不代表其為「Almost sure equality」

**Example**

- 使 $\mathrm x$ 與 $\mathrm y$ 為二元隨機變數（Binary random variables）
- $P_{\mathrm x}(0) = P_{\mathrm x}(1) = P_{\mathrm y}(0) = P_{\mathrm y}(1) = 0.5$
  - $\mathrm x$ 與 $\mathrm y$ 為「Equal in distribution」
  - 但是 $\Pr(\mathrm {x = y}) = 0.5 \neq 1$



### Convergence of random variables

> **實數序列之極限**
>
> - 給定實數序列｛$b_1, \ldots, b_n$｝
>   - 對於任一 $\varepsilon$ 大於 0
>   - 存在一實數 $b$ 以及一整數 $\mathrm N(\varepsilon)$ 使得
>   - $\vert b_n-b \vert<\varepsilon, \forall n >\mathrm N(\varepsilon)$
> - 稱 $b_n$ 為實數序列｛$b_n$｝的極限（Limit），記作
>   - $\lim_{n\to\infty} b_n = b$

> **Definition（Convergence in distribution）**
>
> A sequence of random variables $｛\mathrm {X^{(1)}, X^{(2)},\ldots}｝$ to $\mathrm X$ iff $\lim_{n\to\infty}\mathrm P(\mathrm X^{(n)} = x) = \mathrm P(\mathrm X = x)$



> **Definition（Convergence in probability）**
>
> A sequence of random variables $｛\mathrm {X^{(1)}, X^{(2)},\ldots}｝$ to $\mathrm X$ iff for any $\varepsilon > 0, \lim_{n\to\infty}\Pr(\vert\mathrm{X^{(n)}-X}\vert<\varepsilon) = 1$



> **Definition（Almost sure convergence）**
>
> A sequence of random variables $｛\mathrm {X^{(1)}, X^{(2)},\ldots}｝$ to $\mathrm X$ iff $\Pr(\lim_{n\to\infty}\mathrm X^{(n)} = \mathrm X) = 1$



### Distribution of derived variables

- 如果隨機變數 $\mathrm Y = f(\mathrm X)$ 且 $f^{-1}$ 存在，則 $\mathrm P(\mathrm Y = y) = \mathrm P(\mathrm X = f^{-1}(y))$ 會永遠成立嗎？
  - **否**，當隨機變數 $\mathrm {X,Y}$ 為連續時
- 令連續隨機變數 $\mathrm {X～Uniform}(0,1)$ 且 $P_{\mathrm X}(x) = c, x\in(0,1)$
- 令 $\mathrm Y = \mathrm X/2～\mathrm{Uniform}(0,1/2)$
- 若 $F_{\mathrm Y}(y) = F_{\mathrm X}(f^{-1}(y)) =  F_{\mathrm X}(2y)$
  - $\int_{y = 0}^{\frac 12} F_{\mathrm Y}(y)\mathrm dy = \int_{y = 0}^{\frac 12}c\cdot\mathrm dy = \frac 12 \neq 1$
  - **違反機率論的公理**



- 若 $\mathrm{X,Y}$ 為連續隨機變數
  - 則 $\Pr(\mathrm Y = y) = F_{\mathrm Y}(y)\mathrm dy \\ \Pr(\mathrm X = x) = F_{\mathrm X}(x)\mathrm dx$
- 由於 $f$ 可能會拉扯空間軸，必須要保證
  - $\vert F_{\mathrm Y}(f(x))\mathrm dy \vert= \vert F_{\mathrm X}(x)\mathrm dx\vert$
  - 若 $f^{-1}$ 存在則 $x = f^{-1}(y)$
  - 且 $\Rightarrow F_{\mathrm Y}(f(x)) = F_{\mathrm Y}(y) =  \frac{F_{\mathrm X}(x)\mathrm dx}{\mathrm dy}$
- 可以得到
  - $F_{\mathrm Y}(y) =  \frac{F_{\mathrm X}(x)\mathrm dx}{\mathrm dy} = F_{\mathrm X}(f^{-1}(y))\vert \frac{\partial f^{-1}(y)}{\partial y}\vert$ （或 $F_{\mathrm X}(x) =  F_{\mathrm Y}(f(x))\vert\frac{\partial f(x)}{\partial x}\vert$）
  - 在之前的範例中：$F_{\mathrm Y}(y) = 2\cdot F_{\mathrm X}(2y)$

- 在「多元隨機變數」中，則
  - $F_{\mathrm Y}(y) = F_{\mathrm X}(f^{-1}(y))\vert \det(J(f^{-1})(y))\vert$
  - 而 $J(f^{-1})(y)$ 為**「雅可比矩陣」（Jacobian matrix）**
    - $J(f^{-1})(y)_{i,j} = \partial f^{-1}_i(y)/\partial y_i$



## Common probability distribution



### Random experiments

- 隨機變數 $\mathrm X$ 的值可以視為一個隨機事件的結果
  - 藉此定義其機率分佈（Probability distribution）函數 $\mathrm {P(X)}$



### Bernoulli distribution（Discrete）

> 一次實驗兩種結果，「一個機率」在意某結果發生與否

$$
\mathrm {X ～Bernoulli}(p)
$$



- 機率質量函數（Probability mass function）
  - 給定成功發生的機率 $\mathrm P(success) = p$
  - 只作一次試驗，$\mathrm X = 成功發生的次數$
  - $P_{\mathrm X}(x)= \left\{\begin{matrix} p, &if \;x = 1\\ 1-p, & if \; x = 0 \\ 0, &otherwise\end{matrix}\right. or \;p^x(1-p)^{1-x}$

![1554962546955](\willywangkaa\images\1554962546955.png)

- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \left\{\begin{matrix}0,& x<0 \\ 1-p, &0\leq x < 1 \\ 1, & x\geq 1 \end{matrix}\right.$

![1554963118110](\willywangkaa\images\1554963118110.png)



Example

- 亂停車被拖吊的機率為 0.8
  - 定義隨機變數 $\mathrm X(被拖吊) = 1 \\ \mathrm X(未拖吊) = 0$
  - 則 $\mathrm {X ～Bernoulli}(0.8)$



### Binominal distribution（Discrete）

> 共 $n$ 次實驗，一個機率，在意 $n$ 次實驗中出現 $k$ 次某結果有之機率

$$
\mathrm {X～BIN}(p)
$$



- 機率質量函數（Probability mass function）
  - 給定成功發生的機率 $\mathrm P(success) = p$
  - 作 $n$ 次試驗，$\mathrm X = 成功發生的次數$
  - $P_{\mathrm X}(x) = \mathrm P(\mathrm X = x) = \left\{\begin{matrix}\binom nx p^x(1-p)^{n-x}, & x = 0,1, \ldots,n \\ 0,& otherwise \end{matrix}\right.$
    - #success = x
    - #failure = n-x
- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \sum_{m = -\infty}^{\lfloor x\rfloor}P_{\mathrm X}(m) = \sum_{m = -\infty}^{\lfloor x\rfloor} \binom nmp^m(1-p)^{n-m}$



**Example**

- 在三次亂停車之中，若每次被拖吊的機率為 0.8 ，則三次之中被拖吊兩次的機率為？
  - 定義隨機變數 $\mathrm X$ 為亂停車三次中被拖吊的次數
  - 則 $\mathrm {X～BIN}(3, 0.8)$

$P_{\mathrm X}(2) = \mathrm P(\mathrm X = 2) = \binom 32 0.8^20.2^{3-2}$



### Uniform distribution（Discrete）

> 一次實驗，$n$ 種結果，機會均等，在意某解果發生與否

$$
\mathrm {X～UNIF}(a,b)
$$



- 機率質量函數（Probability mass function）
  - 給定隨機變數 $\mathrm X$ 等於 $a, a+1,\ldots, or \; b$ 的**機率均等**
  - $P_{\mathrm X}(x) = \left\{\begin{matrix}\frac{1}{b-a+1}, &x = a, a+1,\ldots,b \\ 0, &otherwise \end{matrix}\right.$

![1554964948225](\willywangkaa\images\1554964948225.png)

- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \sum_{n = -\infty}^{\lfloor x\rfloor} P_{\mathrm X}(x) = \sum_{n = a}^{\lfloor x\rfloor} \frac{1}{b-a+1} = \frac{\lfloor x\rfloor-a+1}{b-a+1}$
  - $F_{\mathrm X}(x) = \left\{\begin{matrix}0, & x<a \\ \frac{\lfloor x\rfloor-a+1}{b-a+1}, & a\leq x <b \\ 1, & x\geq b \end{matrix}\right.$



Example

- 擲一六面公平骰子
  - 定義隨機變數 $\mathrm X$ 為出現的點數
  - 則 $\mathrm {X～UNIF}(1,6)$
    - $P_{\mathrm X} = \left\{\begin{matrix}\frac16, &x = 1, 2,\ldots,6 \\ 0, &otherwise\end{matrix}\right.$



### Geometric distribution（Discrete）

> 實驗中某結果出現機率已知，重複操作實驗至某結果首次出現為止
>
> 在意某結果是在第幾次實驗才首次出現

$$
\mathrm {X～GEO}(p)
$$



![1554966200838](\willywangkaa\images\1554966200838.png)

- 機率質量函數（Probability distribution function）
  - 給定成功的機率 $\mathrm P(success) = p$
  - 隨機變數 $\mathrm X$ 為作了「幾次試驗」後得到第一次成功
  - $P_{\mathrm X}(x) = \mathrm P(\mathrm X = x) = \left\{\begin{matrix}(1-p)^{x-1}p, &x = 1,2,\ldots \\ 0, & otherwise\end{matrix}\right.$
    - 成功必從第一次以後才有可能發生（$x>0$）

![1554966387154](\willywangkaa\images\1554966387154.png)

- 累進分佈函數（Cumulative distribution  function）
  - $F_{\mathrm X}(x) = \sum_{n = -\infty}^{\lfloor x\rfloor} P_{\mathrm X}(x) = \left\{\begin{matrix}\sum_{n = 1}^{\lfloor x\rfloor}(1-p)^{n-1}p, &x\geq1 \\ 0, &x<1\end{matrix}\right. \\ =\left\{\begin{matrix} p\frac{1-(1-p)^{\lfloor x\rfloor}}{1-(1-p)}, &x\geq1 \\ 0, &x<1\end{matrix}\right. \\ =\left\{\begin{matrix} 1-(1-p)^{\lfloor x\rfloor}, &x\geq1 \\ 0, &x<1\end{matrix}\right.$



Example

- 一投手被上壘的機率為 0.01，第 27 位打者才第一次上壘的機率？
  - 定義隨機變數 $\mathrm X$ 為第幾位打者首次上壘
  - 則 $\mathrm {X～GEO}(0.01)$
    - $P_{\mathrm X}(x) = \left\{\begin{matrix} 0.99^{x-1}0.01, &x = 1,2,\ldots \\ 0, & otherwise\end{matrix}\right.$
    - $P_{\mathrm X}(27) = 0.99^{26}0.01$



### Pascal distribution（Discrete）

> 實驗鍾某結果出現機率已知，重複操作實驗至某結果出現 k 次為止
>
> 在意是在第幾次實驗才看到某實驗結果第 k 次出現

$$
\mathrm {X～PASCAC}(k, p)
$$

![1554968485171](\willywangkaa\images\1554968485171.png)

- 機率質量函數（Probability mass function）
  - 給定成功的機率 $\mathrm P(success) = p$
  - 在意第 $k$ 次成功發生在第幾次實驗 $\max\left\{＃success\right\} = k$
  - 隨機變數 $\mathrm X$ 為第幾次時間中可以看到第 k 次成功（$\max\left\{＃success\right\}$
  - $P_{\mathrm X}(x) = \mathrm P(\mathrm X = x) = \left\{\begin{matrix}\binom{x-1}{k-1}p^{k-1}(1-p)^{x-k}p, & x = k, k+1,\ldots \\ 0, & otherwise\end{matrix}\right.$
    - $k-1$ successes，$x-k$ failures



![1554969621742](\willywangkaa\images\1554969621742.png)

- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \mathrm P(\mathrm X\leq x) \\ = \mathrm P(在 \;x\; 之前有發生過\; k－th \;成功)\\=\mathrm P(在 \;x\; 次實驗中有大於等於 \;k\;次成功) \\ \overset{\mathrm {Y～BIN}(x,p)}{=} \mathrm P(\mathrm Y\geq k)$
    - Passcal distribution 又稱作 Negative binomial 



Example

- 一投手被上壘的機率為 0.01，被上壘 5 次便會換投，第 12 位打者時被換投的機率？
  - 定義隨機變數 $\mathrm X$ 為第 5 次上壘發生於**第幾個打者**
  - 則 $\mathrm {X～PASCAC}(5, 0.01)$
    - $P_{\mathrm X}(12) = \binom{11}{4}0.01^40.99^70.01$



### Poisson distribution（Discrete）

> 某結果出現之平均速率（Rate：次數/時間）已知
>
> **問持續觀察某時間長度後**，看到該結果出現 k 次之機率？

$$
\mathrm {X～POI}(\lambda T) \\ 令\;\mu = \lambda T, \mathrm {X～POI}(\mu)
$$

> **帕松小數定律**
>
> 在[二項分佈](https://zh.wikipedia.org/wiki/%E4%BA%8C%E9%A1%B9%E5%88%86%E5%B8%83)的[伯努利試驗](https://zh.wikipedia.org/wiki/%E4%BC%AF%E5%8A%AA%E5%88%A9%E8%A9%A6%E9%A9%97)中，如果試驗次數 $n$ 很大，二項分佈的機率 $p$ 很小，且乘積 λ= *np* 比較適中，則事件出現的次數的機率可以用帕松分佈來逼近。事實上，二項分佈可以看作帕松分佈在離散時間上的對應物。
>
> 二項分佈定義
> $$
> P_{\mathrm X}(x) = \mathrm P(\mathrm X = x) = \left\{\begin{matrix}\binom nx p^x(1-p)^{n-x}, & x = 0,1, \ldots,n \\ 0,& otherwise \end{matrix}\right.
> $$
> 如果令 $p=\lambda /n$ , $n$ 趨於無窮時 $P$ 的極限：
>
> $\lim_{n\to\infty} P(X=k)=\lim_{n\to\infty}{n \choose k} p^k (1-p)^{n-k} \\  =\lim_{n\to\infty}{n! \over (n-k)!k!} \left({\lambda \over n}\right)^k \left(1-{\lambda\over n}\right)^{n-k}\\ =\lim_{n\to\infty} \underbrace{\left[\frac{n!}{n^k\left(n-k\right)!}\right]}F \left(\frac{\lambda^k}{k!}\right) \underbrace{\left(1-\frac{\lambda}{n}\right)^n}{\to\exp\left(-\lambda\right)} \underbrace{\left(1-\frac{\lambda}{n}\right)^{-k}}{\to 1} \\ = \lim_{n\to\infty} \underbrace{\left[ \left(1-\frac{1}{n}\right)\left(1-\frac{2}{n}\right) \ldots \left(1-\frac{k-1}{n}\right)  \right]}{\to 1} \left(\frac{\lambda^k}{k!}\right) \underbrace{\left(1-\frac{\lambda}{n}\right)^n}{\to\exp\left(-\lambda\right)} \underbrace{\left(1-\frac{\lambda}{n}\right)^{-k}}_{\to 1}      \\ = \left(\frac{\lambda^k}{k!}\right)\exp\left(-\lambda\right)$ 



- 機率質量函數（Probability mass function）
  - 給定**平均出現的速率**（Occurence rate = $\lambda$）、觀察持續時間長度（Observation period = $T$）
  - 隨機變數 $\mathrm X$ 為在觀察時間長度 $T$ 下所看到**事件發生的數量**
  - $P_{\mathrm X}(x) = \mathrm P(\mathrm X = x) = e^{-\lambda T}\frac{(\lambda T)^x}{x!}  \overset{\mu = \lambda T}{=} e^{-\mu}\frac{(\mu)^x}{x!}$

- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm x}(x) = \sum_{n = -\infty}^{\lfloor x\rfloor} P_{\mathrm X}(x) = \left\{\begin{matrix}\sum_{n = -\infty}^{\lfloor x\rfloor} e^{-\mu}\frac{\mu^n}{n!}, & x = 0,1,2,\ldots \\ 0, &otherwise \end{matrix}\right.​$



Example

- *費雯*在發文後，平均每分鐘會有 5 人噓之，問發文後二十分鐘被噓 100 次的機率？
  - $\lambda = 5 \quad\frac{噓}{分}$、$T = 20 \quad 分$
  - 定義隨機變數 $\mathrm X$ 為二十分鐘內的噓的數量
  - 則 $\mathrm {X～POI}(5\times20) = \mathrm {POI}(100)$
    - $P_{\mathrm X}(100) = e^{-\lambda T}\frac{(\lambda T)^{100}}{100!} = e^{-100}\frac{(100)^{100}}{100!}$



