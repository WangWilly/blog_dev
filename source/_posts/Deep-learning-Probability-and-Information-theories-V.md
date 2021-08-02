title: Deep learning - Probability and Information theories V
author: Willy Wang (willywangkaa)
tags:
  - Probability
  - Information theories
categories:
  - Deep learning
date: 2019-04-24 12:38:00
---
# Deep learning - Probability and Information theories V



## Common probability distribution



### Dirac distribution（Continuous）

$$
\mathrm{X～Dirac}(\mu)
$$



- 在一些情況下，希望機率分佈中的所有質量集中在一點 $\mu$ 上
- 透過 Dirac delta 函數 $\delta(x)$ 定義機率密度函數
  - $f_X(x) = \delta(x-\mu)$
  - Dirac delta 函數被定義成**除了在** $X=0$ **以外的所有點的值都為 0，但是整體積分為 1**
- 下圖為 Dirac delta 函數以 $X = 0$ 為中心的**常態分佈**
  - $\delta _{a}(x)=\frac {1}{a\sqrt{\pi}}e^{-x^{2}/a^{2}}$ 隨著 $a\to0$ 的極限（**分佈意義上的**）

![1555656295794](\willywangkaa\images\Dirac_function_approximation.gif)

> Dirac delta 函數不像普通函數依樣對 $x$ 的每個值皆有一個實數值的輸出，此為一中不同類型的數學對象，稱作**廣義函數（Generalized function）**
>
> - 廣義函數依據積分性質定義的數學對象
>   - Dirac delta 函數可以想成一系列函數的**極限點**
>   - 此系列函數將除 0 以外所有點的機率密度**越變越小**
>
> 透過將機率密度函數 $f_X(x)$ 定義成 delta 函數左移 $-\mu$ 個單位，得到一個在 $X = \mu$ **具有無限窄也無限高的峰值機率質量**



#### Empirical Distribution（經驗分佈；Continuous）

> Dirac 分佈經常做為經驗分佈的一個組成部分

$$
\mathrm{X～Empirical}(\mathbb X)
$$

- 給定一個資料集 $\mathbb X = \{x^{(i)}\}_{i = 1}^N$
  - $x^{(i)}$ 相互為 $X$ 的 i.i.d. 樣本
- 在訓練集上訓練模型時，可以從這個**訓練集上得到的經驗分佈指明了採樣來源的分佈**
  - 經驗分佈為訓練數據相似度最大的**機率密度函數**
- 怎樣的機率分佈 $\mathrm P(\theta)$ 可以將 $\mathrm P(\theta\vert\mathbb X)$ 相似度最大化
  - 找到此資料集的**機率分佈**
- 若 $X$ 為**離散型**，經驗分佈可以被「Multinoulli」分佈所組成
  - $f_X(x) = \frac 1N\sum_{i = 1}^N 1(x = x^{(i)})$
- 若 $X$ 為**連續型**，經驗分佈需要藉由 Dirac delta 函數定義
  - $f_X(x) = \frac 1N \sum_{i = 1}^N\delta(x-x^{(i)})$



### Mixtures of distribution（機率分佈的混合）

> - 透過組合一些簡單的機率分佈以定義新的機率分佈
>   - 機率分佈集合 $\{\mathrm P^{(i)}(\theta^{(i)})\}$
>   - $\theta^{(i)}$ 為第 $(i)$ 個機率分佈函數的參數

$$
\mathrm {X～Mixture}(\boldsymbol p, \{\theta^{(i)}\}_i)
$$

**一種機率分佈的混和**

- 取決於從一個 **Categorical 分佈**中取樣的結果

$$
P_X(x)= \sum_i \mathrm P^{(i)}_X(x)\mathrm P_C(i)
$$

- $\mathrm P_C(\cdot)$ 為對各「子分佈」的 Categorical 機率質量函數
  - 其每個子分佈機率由 $\mathrm {X～Mixture}(\boldsymbol p, \{\theta^{(i)}\}_i)$ 的 $p^{(i)}$ 決定
  - $C～\mathrm{Categorical}(\boldsymbol p)$

- $\mathrm P^{(i)}_X(\cdot)$ 為第 $(i)$ 個「子機率分佈函數」
  - 其參數為 $\theta^{(i)}$ 

> 實數隨機變數的「經驗分佈」（Empirical distribution）對於每個**訓練樣本**，是以 *Dirac 分佈*為子分佈的**混合分佈**
>
> - 經驗分佈：$f_X(x) = \frac 1N \sum_{i = 1}^N\delta(x-x^{(i)})$
>   - $\Rightarrow \sum_{i = 1}^N \delta(x-x^{(i)})\frac 1N =\sum_{i = 1}^N \delta(x-x^{(i)}) P_C(i)$
>   - $C～\mathrm{Categorical}(\boldsymbol p)$，$\forall i, p_i = \frac 1N$
> - 子分佈的編號隨機變數 $C$ 為一個**「潛變量」（Latent variable）**
>   - 是一個不能直接觀測到的隨機變數
>   - 在聯合分佈（Joint distribution）下可能與 $X$ 有關
>     - 則 $P_{X,C}(x,c)= \mathrm P_{X,C}(x\vert c)\mathrm P_C(c)$
>     - $\mathrm P_C(\cdot)$ 為潛變量的分佈
>     - $\mathrm P_{X,C}(x\vert c)$ 為關聯潛變量與觀測變量的條件分佈



#### Gaussian mixture model（高斯混合模型）

- 其每個子分佈皆為高斯分佈
  - $\mathrm {X～Mixture}(\boldsymbol p, \{\mu^{(i)}, \Sigma^{(i)}\}_i)$
  - $P^{(i)}_X(x)$：$X～\mathrm{Gaussian}^{(i)}(\mu^{(i)}, \Sigma^{(i)})$
- 混合模型可擁有更多限制
  - 共變數矩陣在各個子分佈間共享 $\Sigma^{(i)} = \Sigma,\forall i$
  - 限制共變數矩陣為對角陣 $\Sigma^{(i)} = \mathrm{diag}(\boldsymbol \sigma)$
  - 限制每個高斯分佈皆有「等向性」（Isotropic） $\Sigma^{(i)} = \boldsymbol \sigma I$
- 任何平滑的**機率密度函數**皆可以被高斯混合模型逼近

![1555918141695](\willywangkaa\images\1555918141695.png)



## Common parameterizing function

> - 一個機率的分佈函數 $\mathrm P_X$
>   - $X～\mathrm{Distribution}(\theta)$
>   - 其參數由 $\theta$ 決定
> - 在機器學習的領域中，$\theta$ 的控制會由一個給定函數來決定
>   - 稱為「Parameterizing function」



### Logistic function

- 「Logistic sigmoid function」為其中一個特別的函數
  - $\sigma(x) = \frac{\exp(x)}{\exp(x)+1} = \frac{1}{1+\exp(-x)} = \frac{1}{1+e^{-x}}$
- 值域
  -  $(0,1)$，實數域
- 常用來決定 Bernoulli 分佈中的參數 $p$ 
  - $p\in(0,1)$

![1555919224741](\willywangkaa\images\1555919224741.png)



### Softplus function

$$
\zeta(x) = \log(1+\exp(x)) = \log(1+e^x)
$$



- Softplus 函數為 $x^+$ 函數（正部函數）的平滑版本
  - $x^+ = \max(0,x)$
- 值域
  - $(0,\infty)$
- 可用來產生常態分佈的
  - $\beta$ 精度（變異數倒數）
  - $\sigma$ 標準差



### Properties

- $1-\sigma(x) = \sigma(-x)$

$$
1-\frac{1}{1+e^{-x}} = \frac{e^{-x}}{1+e^{-x}} = \frac{1}{1+e^{-(-x)}} = \sigma(-x)
$$

- $\log \sigma(x) = -\zeta(-x)$

$$
\log(\frac{1}{1+e^{-x}}) = -\log(1+e^{-x}) = -\zeta(-x)
$$

- $\frac{\mathrm d}{\mathrm dx}\sigma(x) = \sigma(x)(1-\sigma(x))$

$$
\frac{\mathrm d}{\mathrm dx}\frac{1}{1+e^{-x}} = \frac{\mathrm d}{\mathrm dx}(1+e^{-x})^{-1} = -(1+e^{-x})^{-2}\cdot-(e^{-x}) \\ = \frac{1}{1+2e^{-x}+(e^{-x})^2}\cdot e^{-x} = \frac{1}{e^x+2+e^{-x}} = \frac{1}{1+e^{-x}}\frac{1}{1+e^x}\\= \sigma(x)\sigma(-x) = \sigma(x)(1-\sigma(x))
$$

- $\frac{\mathrm d}{\mathrm dx} \zeta(x) = \sigma(x)$

$$
\frac{\mathrm d}{\mathrm dx} \log(1+e^x) = \frac{e^x}{1+e^x} = \frac{1}{1+e^{-x}} = \sigma(x)
$$

- $\forall x\in(0,1), \sigma^{-1}(x) = \log(\frac{x}{1-x})$
  - 稱作**分對數（Legit）**

$$
x = \frac{1}{1+e^{-y}} \\ {\Rightarrow} x+xe^{-y} = 1 \\ \Rightarrow e^{-y} = \frac{1-x}{x} \\ \Rightarrow e^y = \frac{x}{1-x} \\ \Rightarrow y = \sigma^{-1}(x) = \ln(\frac{x}{1-x})
$$

- $\forall x>0, \zeta^{-1}(x) = \log(e^x-1)$

$$
x = \ln(1+e^y) \\ \Rightarrow e^x = 1+e^y \\ \Rightarrow e^y = e^x-1 \\ \Rightarrow y = \zeta^{-1}(x) = \ln(e^x-1)
$$

- $\zeta(x) = \int_{-\infty}^x \sigma(y)\mathrm dy$
  - $\mathrm de^y = e^{y}\mathrm dy$

$$
\int_{-\infty}^x \frac{e^y}{1+e^{y}} \mathrm dy = \int_{-\infty}^x \frac{1}{1+e^y}(e^y\mathrm dy) = \int_{-\infty}^x \frac{1}{1+e^y} \mathrm de^y \\ = \log(1+e^y)\vert_{-\infty}^x = \log(1+e^x)-\log(1+e^{-\infty})\\ = \log(1+e^x)-\log(1) = \log(1+e^x)
$$

- $\zeta(x)-\zeta(-x) = x$
  - 正部函數 $x^+ = \max\{0,x\}$
    - 其平滑函數為「softplus 函數」$\zeta(x)$
  - 負部函數 $x^- = \max\{0,-x\}$
    - 其平滑函數為 $\zeta(-x)$
  - $x = x^+-x^-$
    - $x = \zeta(x)-\zeta(-x)$



## Information theory（資訊論）

> **基本概念**
>
> - **一個不太可能發生的事件**比**一個非常可能發生的事件**所攜帶的資訊還多
>   - 「早上太陽升起」的資訊量少到沒有必要發送
>   - 「早上發生日蝕」的資訊量非常豐富
> - 欲透過此基本想法來**量化（Quantify）**資訊
>   - 機率論可以確立一個事件發生的不確定性的百分比
>   - 資訊論可以**量化該不確定性**
>
>
>
> 1. **非常可能發生的事件資訊量要比較少**
>    - 在極端狀況下，確保能夠發生的事件應該沒有資訊量
>
> 2. **較不可能發生的事件具有更高的資訊量**
>
> 3. **獨立事件應具有增量的資訊**
>    - 投擲硬幣兩次皆為正面攜帶的資訊量，應為擲幣一次為正面所攜帶的訊量還高



### Self-information（自資訊）

定義一個事件 $X = x$ 的**自資訊**
$$
I_X(x) = -\log P_X(x)
$$

> 通常 $\log$ 沒有指定時，在此會是底數為 $e$ 的自然對數

- 定義 $I_X(x)$ 的單位為**「奈特」（nat）**
  - 一奈特為「以 $\frac 1e$ 的機率觀測到一個事件獲得的資訊量」
  - $-\log P_X(x) = \log_{\frac 1e}P_X(x)$
  - $\frac 1e^{奈特} = 事件\;x\;的機率$

![1556025616392](\willywangkaa\images\1556025616392.png)

- 當對數底數為 2 時，單位稱作**「比特」（bit）**
  - 或稱作**夏農（Shannons）**
  - $\frac12^{比特} = 事件\;x\;的機率$

![1556025648289](\willywangkaa\images\1556025648289.png)

> **連續型隨機變數**
>
> - 可能喪失原為離散型隨機變數的性質
> - 一個具有大機率的事件其**資訊量為 0**
>   - 但不保證其一定發生，或一定不發生



### Entropy（熵 $^ㄕ_ㄤ$）

對**整個機率分佈其不確定性**的量化稱為「熵」（Entropy）
$$
\mathrm H(X～\mathrm{Distribution})\\ = \mathrm E_{X～\mathrm{Distribution}}[I_X(x)] = -\mathrm E_{X～\mathrm {Distribution}}[\log P_X(x)]
$$

- 亦記作 $\mathrm {H(Distribution)}$
- 當 $X$ 為離散型隨機變數
  - $\mathrm {H(Distribution)} = -\sum_x P_X\log P_X(x)$ 
  - 稱作**「夏農熵」（Shannon entropy）**
- 當 $X$ 為連續型隨機變數
  - $\mathrm {H(Distribution)} = -\int f_X\log f_X(x)\mathrm dx$
  - 稱作**「微分熵」（Differential entropy）**

- **接近確定性的機率分佈（輸出幾乎肯定）具有較低的熵**
  - 反之，接近均勻的機率分佈具有較高的熵
- （下圖）Bernoulli distribution 的夏農熵
  - 水平軸為 $\mathrm{X～Bernoulli}(p)$ 中的 $p$
    - $\mathrm {H(Bernoulli)} = -(1-p)\log(1-p)-p\log p$
  - 更接近確定性的分佈有較低的夏農熵
    - 當 $p$ 接近 $0$（或接近 $1$）時，分佈幾乎確定，因為隨機便量總是為 $0$（為 $1$）
  - 接近均勻的分佈有高的夏農滴
    - 當 $p = 0.5$，因為分佈在兩個結果之上是均勻的，所以熵最大

![1555929886554](\willywangkaa\images\1555929886554.png)

![1556023800324](\willywangkaa\images\1556023800324.png)



> ![1556026885048](\willywangkaa\images\1556026885048.png)
>
>
>
> ![1556026848421](\willywangkaa\images\1556026848421.png)



### Average code length（期望編碼長度）

> 資訊論為應用數學的一個分支，主要研究一個信號包含的資訊多寡進行量化
>
> 最初被發明用來研究一個含有雜訊的信道上用離散的字母表來發送消息，例如無線電通信
>
> 資訊論可以對消息進行最優編碼、計算消息的期望長度
>
> 夏農熵（Shannon entropy）原始就是要解決此通訊問題

- 從機率分佈 $P_X$ 中的值（$X = x$）
  - 夏農熵可以計算「將每個 $x$ 進行編碼」後平均最低的「比特」（Bit）數量
- 一個隨機變數 $X～\mathrm{Uniform}$
  - 有八個機率相等的狀態
    - $P_X(x) = \frac 18,\forall x$
  - 欲將 $x$ 傳送，勢必要將其編碼
    - 由**夏農熵**可知，平均最低的比特數為 $3$
    - $\mathrm {H(Uniform)} = 8\cdot(-\frac18\log_2\frac 18) = 3$
    - 其最佳編碼為 000, 001, 010, 011, 100, 101, 110, 111
  - 每個狀態出現的機率相等，當一個未知的狀態出現時，**很難猜測**是哪個狀態
    - **亂度大**
- 若八個狀態的機率為 $(\frac12,\frac14,\frac18,\frac1{16}, \frac1{64},\frac1{64},\frac1{64},\frac1{64})$
  - $\mathrm {H(X)} = 2$
  - 其最佳的編碼為 0, 10, 110, 1110, 111100, 111101, 111110, 111111
    - 平均編碼長度為 $2$ 比特
  - 每個狀態出現的機率不等，當一個未知的狀態出現時，**較易猜測**可能是前面幾個狀態會發生
    - **亂度小**
- 此編碼的演算法為**「霍夫曼編碼」（Huffman Coding）**
  - 將每個狀態表示為一個**二元決策樹的葉節點**
  - 每個**決策節點（內節點）**皆有兩個決策，令其各個決策選到的機率為 $\frac12$



- 熵的另一種解釋是**「亂度」（Impurity）**，由此可知如果亂度越大，代表其**平均資訊量（期望資訊量）**會越大
  - **資訊量：Bit、Nat**
  - 因為比特與奈特兩個資訊量的度量只差一個倍數所以在機器學習的範疇中，**通常使用奈特作為資訊量的度量**
    - $-\ln P_X(x) = -\log_2 P_X(x)\times\frac{1}{\log_2(e)}$
    - $\log_{\frac 1e}P_X(x) = \log_{\frac12}P_X(x)\times\frac{1}{\log_2(e)}$



### Kullback-Leibler divergence（KL散度；Relative entropy）

> 對於**一個隨機變數** $X$，有兩個單獨的機率分佈 $P_X(x)、Q_X(x)$

從機率分佈 $Q$ 至 $P$ 的 KL 散度（KL divergence from distribution $Q$ to $P$）
$$
\mathrm {D_{KL}(P\Vert Q)} = \mathrm E_{X～P_X}\left[\log\frac{P_X(x)}{Q_X(x)}\right] = \mathrm E_{X～P_X}[\log P_X(x)-\log Q_X(x)] \\= \underset{\mathrm{entropy}}{\mathrm E_{X～P_X}[\log P_X(x)]}-\mathrm E_{X～P_X}[\log Q_X(x)] \\ = -\mathrm H_X(P_X)-\mathrm E_{X～P_X}[\log Q_X(x)] \\ = 
\underset{\mathrm{cross \; entropy}}{\left(-\mathrm E_{X～P_X}[\log Q_X(x)]\right)}-\mathrm H_X(P_X)
$$

- 在**離散型隨機變數**下
  - 編碼集合 $\mathbb Q$ 為一種能夠使「機率分佈 $Q_X$ 產生的消息」平均*長度* $k$ 是最小的
  - 使用 $\mathbb Q$ 來對「機率分佈 $P_X$ 產生的消息」編碼時，「KL 散度」用來衡量此平均編碼的*長度* $l$ 與*長度* $k$ 的差異（使用 $\mathbb Q$ 來對「機率分佈 $P_X$ 產生的消息」編碼時，所需的額外平均編碼長度）



- 當使用的對數底數為 $2$ 時
  - 資訊*長度*的單位為**比特**
- 機器學習中，使用的對數底數通常為 $e$
  - 資訊*長度*的單位為**奈特**



- **交叉熵（Cross entropy）**
  - $\mathrm {H(P,Q) = H(P)+D_{KL}(P\Vert Q)} = -\mathrm E_{X～P_X}[\log Q_X(x)]$ 
  - 當 $P_X$ 與 $Q_X$ 相互獨立
    - $\arg \underset{\mathrm Q}{\min}\mathrm {D_{KL}(P\Vert Q)}$ 可以由 $\arg \underset{\mathrm Q}{\min}-\mathrm E_{X～P_X}[\log Q_X(x)]$ 解出
    - 因為 $Q_X$ 不參與被省略的那一項



> - $0 \log 0 = 0$
>   - 在資訊論中，將上式表達為 $\lim_{x\to 0} x\log x = 0$



#### Properties

KL 散度的性質

- $\mathrm {D_{KL}(P\Vert Q)} \geq 0 , \forall \mathrm {P,Q}$
  - 對於「機率分佈 $Q_X$ 產生的消息」所進行的最好編碼，拿來對「機率分佈 $P_X$ 產生的消息」編碼，其平均長度必比較長



- $\mathrm {D_{KL}(P\Vert Q)} = 0$
  - $\mathrm{P、Q}$ 在離散型隨機變數下為**相同的機率分佈**
  - $\mathrm{P、Q}$ 在連續型隨機變數下為**幾乎處處相同**



- $\mathrm {D_{KL}(P\Vert Q)}\neq\mathrm {D_{KL}(Q\Vert P)}$
  - 經常被視為分佈之間的*某種距離*，但**非真正的距離**

![1555937595977](\willywangkaa\images\1555937595977.png)



#### Minimizer of KL divergence

在給定一個分佈 $P_X$ 希望用另一個分佈 $Q^＊_X$ 近似之，要選擇 $\mathrm {D_{KL}(P\Vert Q)}$ 或 $\mathrm {D_{KL}(Q\Vert P)}$？

- 令 $P_X$ 為高斯混合模型（兩個高斯分佈）、$Q_X$ 為單一高斯分佈
- $\mathrm{Q^{＊(from)}} = \arg \underset{\mathrm Q}{\min}\mathrm {D_{KL}(P\Vert Q)}$ 或 $\mathrm{Q^{＊(to)}} = \arg \underset{\mathrm Q}{\min}\mathrm {D_{KL}(Q\Vert P)}$ ？
  - 在不同的情況下，會選擇不同的近似手段



- $\mathrm{Q^{＊(from)}}$ 會選擇一個 $Q^＊_X$ **在「**$P_X$ **有高機率密度」的地方使其具有高機率密度**
  - 當 $P_X$ 的機率密度函數同時有多個峰值時
  - $Q^＊_X$ 會選擇將這些峰值模糊到一起，以便將高機率值量同時放到所有峰上
- $\mathrm{Q^{＊(to)}}$ 會選擇一個 $Q^＊_X$ **在「**$P_X$ **有低機率密度」的地方使其具有低機率密度**
  - 當 $P_X$ 的機率密度函數同時有多個峰值且峰值之間隔很寬時
  - $Q^＊_X$ 會選擇其中一個峰以避免將機率質量放置於 $P_X$ 低機率質量的區域中
    - 下圖 $Q^＊_X$ 是選擇強調左峰的結果，當然也可以選擇右峰以達到 $\mathrm{Q^{＊(to)}}$
    - 如果 $P_X$ 的多峰無法被足夠強的低機率區域分離，那 $\mathrm{Q^{＊(to)}}$ 仍可能**選擇模糊這些峰**

![1555982692715](\willywangkaa\images\1555982692715.png)