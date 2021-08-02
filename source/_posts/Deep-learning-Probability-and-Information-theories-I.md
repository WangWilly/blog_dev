title: Deep learning - Probability and Information theories I
author: Willy Wang (willywangkaa)
tags:
  - Probability
  - Information theories
categories:
  - Deep learning
date: 2019-04-19 21:16:00
---
# Deep learning - Probability and Information theories I



## Random variables and probability distribution



### Random variables（隨機變數）

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



> **Definition**（隨機變數 $\mathrm x$）
>
> 為一個**函數**將樣本空間 $\Omega$ 的元素映射至實數空間，換言之 $\mathrm X$ 為一個自訂的法則，將每個實驗結果 $\omega \in \Omega$ 賦予其實數值 $\mathrm X(\omega)$
>
> Example
>
> - 對應於樣本空間 $\Omega = ｛(1,1),(1,2), \ldots,(6, 6)｝$，自訂一個隨機變數 $\mathrm X$   其等於有序對（Order pair）的元素相加
>   - $\mathrm X(1, 1) = 2, \mathrm X(1, 2) = 3$
>   - 推廣：$\mathrm X(i, j) = i+j$
>
> **Notation**
>
> - $\mathrm X = x$
>   - 為事件集合 $｛\omega\in\Omega:\mathrm X(\omega) = x｝$



- 一個**隨機變數** $\mathrm X$ 可被隨機指定指定值的變數
  - （以正體標示，斜體標示為一個常數變數）
  - 會對應一個機率值當指定 $\mathrm x$ 一個值時
    - 如：$\Pr(\mathrm X = x_1) = 0.1, \Pr(\mathrm X = x_2) = 0.3\ldots$
  - 正規來說，$\mathrm X$ 為一個函數將一個「機率事件」映射至「實數域」
- 必須伴隨一個可以說明當 $\mathrm X$ 為該實數值的**機率分佈** $\mathrm P$
  - $\mathrm X～\mathrm P(\theta)$ 意旨「$\mathrm X$ 擁有一個以 $\theta$ 為**參數**的機率分佈 $\mathrm P$」



> - 若 $\mathrm X$ 為**離散值**，$\mathrm P(\mathrm X = x)$ 為一個**「機率質量函數」（Probability mass function**；$P_{\mathrm X}(x) = \mathrm P(\mathrm X = x) = \Pr(\mathrm X =x )$）
>   - 如：「一個公正的骰子能擲出的點數」為「離散均勻分佈」並且 $P_{\mathrm X}(x) = \frac 16$
> - 若 $\mathrm X$ 為**連續值**，$\mathrm P(\mathrm X = x)$ 為一個**「機率密度函數」（Probability density function**；$F_{\mathrm x}(x)$）
>   - $F_{\mathrm X}(x)$ 的輸出值不為**機率**而是**機率密度**（在 $x$ 點之**累進機率成長的幅度**）
>   - 為「累進分佈函數」（Cumulative distribution function）的**導函數**
>     - 機率求取：$\Pr(a\leq\mathrm x\leq b) = \int_{[a,b]}p(x)\mathrm dx$
>     - $F_{\mathrm X}(x)$ 的輸出可能大於 1
>         - 在 [a, b] **平均分佈（Uniform distribution）**的狀態下
>         - 其**機率密度函數** $p(x) = \left\{\begin{matrix}\frac {1}{b-a} &,if\; x\in [a, b] \\ 0 &,otherwise  \end{matrix}\right.$
>         - 當 $b-a$ 小於 1 時，$p(x)>1$



### Marginal probability（邊際機率）

- 多個變數存在於一個機率分佈
  - $P_{\mathrm{X,Y}}(x, y)$ （**聯合機率分佈；Joint probability distribution**）
- **邊際機率分佈**
  - 在一個聯合機率分佈之下，取其多個隨機變數中只對其子集所關心的機率分佈
    - 離散型：$\mathrm P(\mathrm X = x) = \sum_y P_{\mathrm{X,Y}}(x, y)$
    - 連續型：$\int F_{\mathrm{X,Y}}(x,y)\mathrm dy$
  - 亦稱作「Sum rule of probability」



### Conditional probability（條件機率）

- 在機率質量函數或機率密度函數中，其條件機率函數為
  - $\mathrm P(\mathrm X = x\vert\mathrm X = y) = \frac {\mathrm P(\mathrm X = x,\mathrm Y = y)}{\mathrm P(\mathrm Y = y)}$
  - 必須符合 $\mathrm P(\mathrm y = y) > 0$ 的情況下
- **Product rule of probability**
  - $\mathrm P(\mathrm X^{(1)}, \ldots, \mathrm X^{(n)}) = \mathrm P(\mathrm X^{(1)})\prod_{i = 2}^n\mathrm P(\mathrm X^{(i)}｜\mathrm X^{(1)}, \ldots, \mathrm X^{(i-1)})$
  - 如：$\mathrm P(\mathrm{a, b, c}) = \mathrm P(\mathrm{c｜a, b})\mathrm P(\mathrm{b｜a})\mathrm P(\mathrm{a})$



### Independence and conditional independence（獨立與條件獨立）

- 隨機變數 $\mathrm X$ **獨立**於（Independent）$\mathrm Y$
  - 若且唯若 $\mathrm P(\mathrm X\vert\mathrm Y) = \mathrm {P(Y)}$
  - 可推論至 $\mathrm {P(X,Y) = P(X)P(Y)}$
  - 標記為 $\mathrm X \perp \mathrm Y$
- 隨機變數 $\mathrm X$ 在 $\mathrm Z$ 之下**條件獨立**於（Conditionally independent）$\mathrm Y$
  - 若且唯若 $\mathrm {P(X\vert Y, Z) = P(X\vert Z)}$
  - 可推論至 $\mathrm {P(X,Y\vert Z) = P(X\vert Z)P(Y\vert Z)}$
  - 標記為 $\mathrm X \perp \mathrm{Y\vert Z}$



### Expectation（期望值）

- 可稱為「Expectation value」或「Mean」（平均值）
- 當一個"平均值"定義在對應於 $\mathrm X$ 的函數 $f$ 中
  - 將 $f$ 視為一個事件模型（分佈模型），當輸入 $\mathrm X$ 值改變時，其輸出值的加權（其出現機率）平均 
  - 離散型：$\mathrm E_{\mathrm {X～P}}[f(x)] = \sum_xP_{\mathrm X}(x)f(x)$
  - 連續型：$\mathrm E_{\mathrm {X～P}}[f(x)] = \int F_{\mathrm X}(x)f(x) \mathrm dx = \mu_{f(x)}$
- 對於與 $\mathrm X$ 無關的變數 $a, b$，**期望值函數為線性函數**
  - $\mathrm E[af(\mathrm X)+b] = a\mathrm E[f(\mathrm X)]+b$

**證明**（離散型）

$\mathrm E[af(\mathrm X)+b] = \sum_xP_{\mathrm X}(x)(af(x)+b) \\ = \sum_x aP_{\mathrm X}(x)f(x)+ bP_{\mathrm X}(x) = a\sum_x P_{\mathrm X}(x)f(x)+b\sum_xP_{\mathrm X}(x)= \\ =a\sum_x P_{\mathrm X}(x)f(x)+b\cdot1 = a\mathrm E[f(\mathrm x)]+b$



- 因為 $\mathrm E[f(x)]$ 為一個**決定性的定值**
  - 則 $\mathrm E[\mathrm E[f(x)]] = \mathrm E[f(x)]$
- 在**聯合機率分佈之下定義期望值**
  - 離散型：$\mathrm {E[f(X,Y)]} = \sum_{x,y}P_{\mathrm {X,Y}}(x,y)f(x,y)$
  - 連續型：$\mathrm {E[f(X,Y)]} = \int_{x,y}F_{\mathrm {X,Y}}(x,y)f(x,y)\mathrm dx\mathrm dy$ 
- 條件期望值
  - 離散型：$\mathrm E[f(\mathrm X)｜\mathrm Y = y] = \sum_x P_{\mathrm{X,Y}}(x｜y)f(x)$
  - 連續型：$\mathrm E[f(\mathrm X)｜\mathrm Y = y] = \int F_{\mathrm{X,Y}}(x｜y)f(x)\mathrm dx$
  - 若隨機變數 $\mathrm {X,Y}$ **相互獨立**
    - $\mathrm {E[f(X)g(Y)] = E[f(X)]E[g(Y)]}$



### Variance（變異數）

- 描述「一個對應於隨機變數 $\mathrm X$ 的事件函數 $f$」其**輸出之於該期望值的差異量**
  - $\mathrm {Var[f(X)] = E[(f(X)-E[f(X)])^2] = \sigma_{f(X)}^2}$
    -  = $(「每個 \;\mathrm {f(X)} \;的輸出」- 「\;\mathrm {f(X)} \;的平均數」)^2 的平均數$
    - 離散型：$\sigma^2 = \frac 1n\sum_{i = 1}^n(x_i-\mu)^2$
  - $\sigma_{\mathrm f(x)}$ 稱作「Standard deviation」（標準差）
- 對於與 $\mathrm x$ 無關的變數 $a, b$ 時
  - $\mathrm {Var}[af(\mathrm X)+b] = a^2\mathrm{Var}[f(\mathrm X)]$
  - 因為取變異數其可視為一個**二次式**
  - 又因為取變異數是與其平均數的**相對值**，所以 $b$ 不影響其輸出

**證明**

$\mathrm {Var}[af(\mathrm X)+b] = \mathrm{E[(af(X)+b-E[af(X)+b])^2]} \\ = \mathrm{E[(af(X)+b-aE[f(X)]-b)^2]} = \mathrm{E[(af(X)-aE[f(X)])^2]}\\= \mathrm{a^2E[(f(X)-E[f(X)])^2]} = a^2\mathrm{Var}[f(\mathrm X)]$



### Covariance（共變異數）

> 在給定兩個變量（兩個隨機變數 $\mathrm {X, Y}$）之下
>
> 1. 探討兩個變量之間是否有關聯，其關聯程度是多少，在統計學上稱為「**相關**」
> 2. 藉由「共變異數」可以決定兩個變量的「線性相關程度」
>    - 正相關：當 $\mathrm X$ 值增加時 $\mathrm Y$ 值隨之增加，此時稱兩變數為**線性正相關**
>    - 負相關：當 $\mathrm X$ 值增加時 $\mathrm Y$ 值隨之遞減，此時稱兩變數為**線性負相關**
>    - 零相關：當 $\mathrm X$ 值增加時 $\mathrm Y$ 值不隨之增加遞減或是成**非線性相關**，此時稱兩變數為**線性零相關**
> 3. 引申出「相關係數」$r$，$\mathrm {X, Y}$ 有 $n$ 筆數據
>    - $-1\leq\frac{\sum_{i = 1}^n(x_i-\mathrm E[f(\mathrm X)])(y_i-\mathrm E[g(\mathrm Y)])}{n\cdot\sigma_{\mathrm X}\cdot \sigma_{\mathrm Y}}\leq1$

- 共變異數描述 $f(\mathrm X)$ 與 $g(\mathrm Y)$ 的變動的差異性
  - $\mathrm{Cov[f(X), g(Y)] = E[(f(X)-E[f(X)])(g(Y)-E[g(Y)])]}$
  - 此數為正時，當 $\mathrm X$ 值增加時 $\mathrm Y$ 值隨之增加
  - 此數為負時，當 $\mathrm X$ 值增加時 $\mathrm Y$ 值隨之遞減，反之亦然
- 若 $\mathrm{X, Y}$ 相互獨立，則 $\mathrm{Cov(X,Y)} =0$
  - **反之不成立**，當 $\mathrm{X,Y}$ 有可能相互有「非線性關係」（Nonlinear）
  - 如：$\mathrm Y = \sin(\mathrm X), \mathrm Y ～\mathrm{Uniform(-\pi, \pi)}$ 如下圖



![1554273024744](\willywangkaa\images\1554273024744.png)



- $$\mathrm{Var}(af(\mathrm X)+bg(\mathrm Y)) \equiv \mathrm{Var}(a\mathrm X+b\mathrm Y) \\ = a^2\mathrm{Var(X)}+b^2\mathrm{Var(Y)}+2ab\mathrm{Cov(X,Y)}$$
  - 如果 $\mathrm{X,Y}$ 相互獨立則 $\mathrm{Var}(\mathrm X+\mathrm Y) = \mathrm{Var(X)}+\mathrm{Var(Y)}$ 

**證明**

$\mathrm{Var}(a\mathrm X+b\mathrm Y) = \mathrm{E[(ax+by-E[ax+by])^2]} \\ = \mathrm{E[(ax+by- aE[x]-bE[y])^2]} = \mathrm{E[(a(x-E[x])+b(y-E[y]))^2]} \\ = \mathrm{E[a^2(x-E[x])^2+b^2(y-E[y])^2+ab(x-E[x])(y-E[y])]} \\ = \mathrm{a^2E[(x-E[x])^2]+b^2E[(y-E[y])^2]+abE[(x-E[x])(y-E[y])]} \\ = a^2\mathrm{Var(x)}+b^2\mathrm{Var(y)}+2ab\mathrm{Cov(x,y)}$ 



- $\mathrm{Cov}(a\mathrm x+b, c\mathrm y+d) = ac\mathrm{Cov(x,y)}$

**證明**

$\mathrm{Cov}(a\mathrm x+b, c\mathrm y+d) = \mathrm E[(a\mathrm x+b-\mathrm E[a\mathrm x+b])(c\mathrm y+d-\mathrm E[c\mathrm y+d])] \\ =  \mathrm E[(a\mathrm x+b-a\mathrm E[\mathrm x]-b)(c\mathrm y+d-c\mathrm E[\mathrm y]-d)] \\ = \mathrm E[ac(\mathrm x-\mathrm E[\mathrm x])(\mathrm y-\mathrm E[\mathrm y])] = ac\mathrm E[(\mathrm x-\mathrm E[\mathrm x])(\mathrm y-\mathrm E[\mathrm y])] = ac\mathrm{Cov(x,y)}$ 



- $\mathrm{Cov}(a\mathrm x+b\mathrm y, c\mathrm w +d\mathrm v) = \\ ac\mathrm{Cov(x,w)}+ad\mathrm{Cov(x,v)}+bc\mathrm{Cov(y,w)}+bd\mathrm{Cov(y,v)}$

**證明**

$\mathrm{Cov}(a\mathrm x+b\mathrm y, c\mathrm w +d\mathrm v) \\= \mathrm E[(a\mathrm x+b\mathrm y-\mathrm E[a\mathrm x+b\mathrm y])(c\mathrm w+d\mathrm v-\mathrm E[c\mathrm w+d\mathrm v])] \\ = \mathrm E[(a\mathrm x+b\mathrm y-a\mathrm E[\mathrm x]-b\mathrm E[\mathrm y])(c\mathrm w+d\mathrm v-c\mathrm E[\mathrm w]-d\mathrm E[\mathrm v])] \\ = \mathrm E[(a(\mathrm x-\mathrm E[\mathrm x])+b(\mathrm y-\mathrm E[\mathrm y]))(c(\mathrm w-\mathrm E[\mathrm w])+d(\mathrm v-\mathrm E[\mathrm v]))] \\= \mathrm E[ac(\mathrm x-\mathrm E[\mathrm x])(\mathrm w-\mathrm E[\mathrm w])+ad(\mathrm x-\mathrm E[\mathrm x])(\mathrm v-\mathrm E[\mathrm v])\\+bc(\mathrm y-\mathrm E[\mathrm y])(\mathrm w-\mathrm E[\mathrm w])+bd(\mathrm y-\mathrm E[\mathrm y])(\mathrm v-\mathrm E[\mathrm v])] \\ = ac\mathrm{Cov(x,w)}+ad\mathrm{Cov(x,v)}+bc\mathrm{Cov(y,w)}+bd\mathrm{Cov(y,v)}$ 



## Multivariate and Derived variables



### Multivariate random variables（多元隨機變數）

- 多元隨機變數標記為 $\mathrm x = \begin{bmatrix} \mathrm x_1\\ \vdots \\ \mathrm x_d \end{bmatrix}$
  - 可視為一個向量（稱作「Random vector」隨機向量），裡面每一個分量則為一個特性（Attributes；Variables；Features），其彼此通常為相互依賴的（Dependent）否則拆開討論即可
  - 多元隨機變數對應的機率分佈 $\mathrm {P(x)}$ 即為 $\mathrm{x_1,\ldots,x_d}$ 對應的**聯合機率分佈**
- $\mathrm x$ 的**期望值**定義為 $\mu_{\mathrm x} = \mathrm{E(x) = \begin{bmatrix} \mathrm \mu_{x_1}\\ \vdots \\ \mathrm \mu_{x_d} \end{bmatrix}}$
- $\mathrm x$ 的**共變異數矩陣（Covariance matrix）**
  - $\Sigma_x = \begin{bmatrix} \sigma^2_{\mathrm x_1} & \sigma_{\mathrm {x_1,x_2}} & \ldots & \sigma_{\mathrm {x_1,x_d}}\\ \sigma_{\mathrm {x_2,x_1}} & \sigma^2_{\mathrm x_2} & \ldots & \sigma_{\mathrm {x_2,x_d}} \\ \vdots & \vdots & \ddots & \vdots \\ \sigma_{\mathrm {x_d,x_1}} & \sigma_{\mathrm {x_d,x_2}} & \ldots &\sigma^2_{\mathrm x_d} \end{bmatrix}$
  - $\sigma_{\mathrm {x_i,x_j}} = \mathrm{Cov(x_i,x_j) = E(x_i-\mu_{x_i})(x_j-\mu_{x_j})} \\ = \mathrm{E(x_ix_j)-\mu_{x_i}\mu_{x_j}}$
  - $\Sigma_x = \mathrm{Cov(x) = E[(x-\mu_x)(x-\mu_x)^T] = E(xx^T)-\mu_x\mu_x^T}$ 
  - **重要性質**
    - 必為**對稱矩陣**
    - 必為**半正定**
      - 特徵向量必為**實數**且 ≧ 0
    - 此矩陣為**非奇異**若且唯若此舉陣為**正定**
    - 此矩陣為**奇異**代表隨機向量 $\mathrm x$ 可能
      - 包含**確定性（Deterministic）**的隨機變數，此值與其他隨機變數的共變異數為 0（相互獨立）
      - 包含一對以上隨機變數**相互獨立**
      - 包含一對以上隨機變數為**非線性相依**
      - 包含重複的隨機變數導致矩陣內兩列（Row）相依

**證明共變異數矩陣必為半正定**

1. 令 $C$ 為一個共變異數矩陣
   - 存在一個隨機向量 $\mathrm x$ 使得 $C = E[(x-\mu_x)(x-\mu_x)^T]$
2. $\forall u, u^TCu = u^TE[(x-\mu_x)(x-\mu_x)^T]u \\ = E[u^T(x-\mu_x)(x-\mu_x)^Tu] \\ = E[＜(x-\mu_x), u＞^2] = ＜(x-\mu_x), u＞^2$
   - 因為 $＜(x-\mu_x), u＞^2$ 必為**實數**，則對其求期望值不會改變其值
3. 因為 $＜(x-\mu_x), u＞ \in \mathbf R$
   - $u^TCu = ＜(x-\mu_x), u＞^2 \geq 0$
   - 共變異數矩陣為半正定



### Derived random variables（衍隨機變數）

- 由其他**隨機向量**（$\mathrm x$）衍生定義出的**隨機變數**（$\mathrm y$）
  - 給定一個參數向量 $w$，$\mathrm x$ 可衍生隨機變數 $\mathrm y$
  - $\mathrm y = f(\mathrm x;w) = w^Tx$
- 由 $\mathrm x$ 向量以描述向量 $\mathrm y$ 的性質
  - 期望值向量：$\mu_{\mathrm y} = \mathrm E(w^T\mathrm x) = w^T\mathrm{E(x)} = w^T\mu_x$
- $\sigma^2_{\mathrm y} = w^T\Sigma_{\mathrm x}w$
  - $\sigma^2_{\mathrm y}  = \mathrm E[(\mathrm y-\mu_{\mathrm y})^2] \\ = \mathrm E[(w^T\mathrm x-w^T\mu_{\mathrm x})^2] \\ = \mathrm E[(w^T(\mathrm x-\mu_{\mathrm x}))^2] \\ = \mathrm E[(w^T(\mathrm x-\mu_{\mathrm x}))((\mathrm x-\mu_{\mathrm x})^Tw)] \\ = w^T \mathrm E[((\mathrm x-\mu_{\mathrm x}))((\mathrm x-\mu_{\mathrm x})^T)] w = w^T\Sigma_{\mathrm x}w$



## Bayes' rule and statistics

> $\Pr(\mathrm x = x)$ **的意義？**
>
> 1. **Bayesian probability（貝氏機率）**
>    - It's a degree of belief or qualitative levels of certainty
>    - 「相信 $\mathrm x = x$ 事件發生的程度（確定性）」
> 2. **Frequentist probability（頻率）**
>    - If we can draw samples of $\mathrm x$, them the proportion of frequency of samples having the value $\mathrm x$ is equal to $\Pr(\mathrm x = x)$
>    - 如果可以對隨機變數 $\mathrm x$ 進行抽樣，$\Pr(\mathrm x = x)$ 則為 $x$ 在抽樣中的出現比率
>
> **上述兩個意義應為一致**



### Bayes' rule（貝氏定理）


$$
\mathrm {P(y\vert x)} = \frac{\mathrm{P(x\vert y)P(y)}}{\mathrm{P(x)}} = \frac{\mathrm{P(x\vert y)P(y)}}{\mathrm{\sum_yP(x\vert y} = y)\mathrm{P(y}=y)}
$$

- 貝氏定理在機器學習上是非常重要的概念，所以上述的每一項皆有各自的名稱
  - $\mathrm{posterior = \frac{likeihood \times prior}{evidence}} \\ \mathrm{後驗機率 = \frac{相似性\times 前驗機率}{事證}}$
- **為何重要？**
  - 一個醫生在診斷病人的疾病時，內心中其實令看到的「病徵」（Symptom）為 $\mathrm x$、令「病種」（Disease）為 $\mathrm y$，而醫生的目標就是要經由「病徵」確診病人的「病種」（使 $\mathrm{P(y\vert x)}$ 最大化）
  - 醫生藉由過往統計的 $\mathrm{P(x \vert y), P(y)}$ （在某個「病種」下其「病徵」發作的機率、該「病種」發生的機率）更輕鬆的判斷



### Point estimation（點估計）

- 「點估計」：試著**藉由樣本（單一的性質；單點）**以估計**母體未知的參數**（$\theta$；性質，有可能是平均值、標準差…等性質）
  - 如：為了瞭解台北市民的平均月收入（假設有 260 萬人），挑選其中 1000 人計算月收入的算術平均數，假設為 60000，若使用點估計則會推論台北市民的月平均收入為 60000
- 假設一個獨立同分佈（Independent and identically distributed；i.i.d.）隨機變數 $\mathrm x$ 有 $n$ 個的樣本記作 $\left\{ x^{(1)},\ldots,x^{(n)} \right\}$  
  - 這些資料的點估計（Pointer estimator；Statistic）函數為
    - $\hat\theta_n = g(x^{(1)},\ldots,x^{(n)})$
  - $\hat \theta_n$ 稱作 $\theta$ 性質的**估計**
    - 在機器學習中目標就是希望能找出一個好的函數 $g$ 使得 $\hat \theta_n$ 性質與 $\theta$ 性質越相近越好



#### Sample mean and covariance

- 給定 $X = \begin{bmatrix} x^{(1)} \\ \vdots \\ x^{(n)} \end{bmatrix} \in \mathbf R^{n\times d}$ 為一個 i.i.d. 樣本（Design matrix），則 $\mathrm x$ 的「猜想平均數向量」與「猜想共變異矩陣」為何？
  - 樣本平均數向量
    - $\hat\mu_x = \frac 1n\sum_{i = 1}^nx^{(i)}$
  - 樣本共變異矩陣
    - $\hat\Sigma_{\mathrm x} = \frac 1n\sum_{i = 1}^n(x^{(i)}-\hat\mu_{\mathrm x})(x^{(i)}-\hat\mu_{\mathrm x})^T$
      - 第 $(i)$ 個樣本
      - $(x^{(i)}-\hat\mu_{\mathrm x})(x^{(i)}-\hat\mu_{\mathrm x})^T$ 為第 $(i)$ 個樣本的共變異矩陣
    - $\mathrm x_i, \mathrm x_j$ 兩個**隨機變數**的「共變異數」
      - $\hat\sigma_{\mathrm x_i,\mathrm x_j}^2 = \frac1n\sum_{s = 1}^n(x^{(s)}_i-\hat\mu_{\mathrm x_i})(x^{(s)}_j-\hat\mu_{\mathrm x_j})$
        - 第 $(s)$ 個樣本
        - $(x^{(s)}_i-\hat\mu_{\mathrm x_i})(x^{(s)}_j-\hat\mu_{\mathrm x_j})$ 為第 $(s)$ 個樣本對於 $\mathrm x_i, \mathrm x_j$ 兩個**隨機變數**的「共變異數」
  - 若將每個樣本 $x^{(i)}$ 歸位化（先將每個樣本減去 $\hat\mu_{\mathrm x}$，成為零均值 zero-mean）
    - 則 $\hat\Sigma_{\mathrm x} = \frac {\sum_{i = 1}^nx^{(i)^T}x^{i}}{n} = \frac1n X^TX$ 

> - 假設只有一個歸位化的隨機變數 $\mathrm Y$
>   - 其變異數 $\sigma^2 = \mathrm {E(Y^2)}$
>   - 在有 $n$ 個樣本資料 $\mathrm {Y_1,Y_2,\ldots, Y_n}$ 的情況下，其「猜想變異數」會等價於「猜想平均數」
>     - $\hat\sigma^2_{\mathrm Y} = \hat\mu_{\mathrm Y} = \frac{\sum_{k = 1}^n Y_k^2}{n}$ 
> -  假設一個隨機向量 $\mathrm X = \begin{bmatrix}\mathrm{X_1,\ldots,X_d} \end{bmatrix}$，每個元素皆為歸位化的隨機變數
>   - 其共變異數矩陣 $\Sigma_{\mathrm X }\mathrm {= E(X^TX)}$