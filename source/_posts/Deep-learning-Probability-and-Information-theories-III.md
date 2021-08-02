title: Deep learning - Probability and Information theories III
author: Willy Wang (willywangkaa)
tags:
  - Probability
  - Information theories
categories:
  - Deep learning
date: 2019-04-19 21:18:00
---
# Deep learning - Probability and Information theories III



## Common probability distribution

> - 離散的隨機變數以「Probability mass function」透漏某個數字發生的機率
> - 對連續的隨機變數，欲知某數字發生的機會多大，可以使用 P.M.F.？
>   - 無法使用，因為每個數字恰發生的機率都為 0
> - 要在連續上討論，要以密度的方式討論，對其隨機變數 $\mathrm X$ 其機率密度為：
>   - 「Probability density function」：$f_{\mathrm X}(x) = \lim_{\Delta x\to 0}\frac{P(x\leq\mathrm X\leq x+\Delta x)}{\Delta x} = \lim_{\Delta x\to 0}\frac{F_{\mathrm X}(x+\Delta x)-F_{\mathrm X}(x)}{\Delta x} = F'_{\mathrm X}(x)$
>
> - 「Probability density function」如何與機率連結？
>   - $f_{\mathrm X}(x) = \lim_{\Delta x\to 0}\frac{P(x\leq\mathrm X\leq x+\Delta x)}{\Delta x}$
>   - 當 $\Delta x$ 非常小時：$P(x\leq\mathrm X\leq x+\Delta x)\approx f_{\mathrm X}(x)\Delta x$
> - 「Probability density function」性質
>   - $f_{\mathrm X}(x) = F'_{\mathrm X}(x)$
>     - $f_{\mathrm X}(x)\geq0$
>   - $F_{\mathrm X}(x) = \int_{-\infty}^xf_{\mathrm X}(u)\mathrm du$
>   - $\int_{-\infty}^{\infty}f_{\mathrm X}(x)\mathrm dx = 1 = F_{\mathrm X}(\infty)$
>   - $P(x_1\leq\mathrm X\leq x_2) = \int_{x_1}^{x_2}f_{\mathrm X}(x)\mathrm dx$
>     - $P(x_1\leq\mathrm X\leq x_2) = F_{\mathrm X}(x_2)-F_{\mathrm x}(x_1) \\ = \int_{-\infty}^{x_2}f_{\mathrm X}(x)\mathrm dx-\int_{-\infty}^{x_1}f_{\mathrm X}(x)\mathrm dx\\= \int_{x_1}^{x_2}f_{\mathrm X}(x)\mathrm dx$
>
> ![1554983657822](\willywangkaa\images\1554983657822.png)

> 與「Probability density function」有關的名詞
>
> - pth percentile
>   - 如：30th percentile of $\mathrm X$，表示為 $\mathrm X_{0.3}$
>   - $\mathrm P(-\infty\leq\mathrm X\leq \mathrm X_{0.3}) = 0.3$
>
> ![1554983848235](\willywangkaa\images\1554983848235.png)
>
> - Median（中位數）：50th percentile of $\mathrm X$，即 $\mathrm X_{0.5}$
>
> ![1554984331534](\willywangkaa\images\1554984331534.png)
>
> - Mode：使 $f_{\mathrm X}(x)$ 發生最大值的 $x$ 值
>
> ![1554984389081](\willywangkaa\images\1554984389081.png)



### Uniform distribution（Continuous）

$$
\mathrm {X～UNIF}(a,b)
$$

![1554984634190](\willywangkaa\images\1554984634190.png)

- 機率密度函數（Probability density function）
  - $f_{\mathrm X}(x) = \left\{\begin{matrix}\frac{1}{b-a}, & a\leq x\leq b \\ 0, & otherwise\end{matrix}\right.$

![1554984835136](\willywangkaa\images\1554984835136.png)

- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \int_{-\infty}^x f_{\mathrm X}(u)\mathrm du = \left\{\begin{matrix}0, &x\leq a \\ \frac{x-a}{b-a}, & a<x\leq b \\ 1, & x>b\end{matrix}\right.$



Example



- 已知公車每十分鐘一班，小美隨意出發至公車站，小美需等候公車的時間為 $\mathrm X$
  - $\mathrm {X～UNIF}(0,10)$

![1554984994261](\willywangkaa\images\1554984994261.png)



### Exponential distribution（Continuous）

$$
\mathrm {X～Exponential}(\lambda)
$$

![1554985301953](\willywangkaa\images\1554985301953.png)

- 機率密度函數（Probability density function）
  - $f_{\mathrm X}(x) = \left\{\begin{matrix}\lambda e^{-\lambda x}, & x\geq 0 \\ 0, & otherwise\end{matrix}\right.$



- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \int_{-\infty}^x f_{\mathrm X}(u)\mathrm du = \int_{-\infty}^x \lambda e^{-\lambda u}\mathrm du \\ = -\int_{-\infty}^x \lambda e^{-\lambda u}\mathrm d(-\lambda u)\\ = -[e^{-\lambda u}]_{-\infty}^x = 1-e^{-\lambda x}$
  - $F_{\mathrm X}(x) = \left\{\begin{matrix}1-e^{-\lambda x}, & x\geq0 \\ 0, &x<0\end{matrix}\right.$



- Exponential distribution 有失憶的性質（Memoryless），常被用來 model 有這種性質的事情
  - 如：小美出門化妝所需的時間



> - 在深度學習中經常會需要一個在 $X = 0$ 處取得邊界點（Sharp point） 的分佈
>   - 在實現中使用了**指數分佈**
>   - $f_{\mathrm X}(x) = \left\{\begin{matrix}\lambda e^{-\lambda x}, & x\geq 0 \\ 0, & otherwise\end{matrix}\right.$ 亦可寫成 $f_{\mathrm X}(x) = \lambda\cdot 1(x\geq 0)\cdot e^{-\lambda x}$
>     - 使用「Indicator function」來使得 $X$ 取負值時函數輸出為 0
>
> ![1555654177483](\willywangkaa\images\1555654177483.png)
>
> - 關係緊密的機率分佈為 **Laplace 分佈（Laplace distribution）**
>   - 允許再任意一點 $\mu$ 處**設置機率密度的最高點**
>   - 可想像成一個「兩面」的指數分佈
>     - $\mathrm {X～Laplace}(\mu,\gamma) = \frac{1}{2\gamma}e^{-\frac{\vert x-\mu\vert}{\gamma}}$
>
> ![1555654189924](\willywangkaa\images\1555654189924.png)



### Erlang distribution（Continuous）

$$
\mathrm {X～Erlang}(n,\lambda)
$$



![1554986080345](\willywangkaa\images\1554986080345.png)

- 機率密度函數（Probability density function）

  - $f_{\mathrm X}(x) = \left\{\begin{matrix} \frac{1}{(n-1)!}\lambda^nx^{n-1}e^{-\lambda x}, & x\geq 0 \\ 0, & otherwise \end{matrix}\right.$



- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \left\{\begin{matrix} 1-\sum_{k=0}^{n-1}\frac{(\lambda x)^k}{k!}e^{-\lambda x}, &x\geq 0 \\ 0, & otherwise \end{matrix}\right.$



- $\mathrm {Erlang}(n,\lambda)$ 常被用來 model 一件有多個關卡而每個關卡所花的時間都為隨機的總時間
  - 關卡數：$n$
  - 每關卡所需時間之機率分佈：$\mathrm {Exponential}(\lambda)$
  - 如：打電動過三關所需時間 $\mathrm{Erlang}(3,\lambda)$



### Normal distrubution（Continuous）

$$
\mathrm{X～Gaussian}(\mu, \sigma)
$$



- 常態分佈，亦常被稱作 Gaussian distribution

![1554986768092](\willywangkaa\images\1554986768092.png)

- 機率密度函數（Probability density function）
  - $f_{\mathrm X}(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$



> **定理（中央極限定理；Central limit theorem）**
>
> - 隨機變數 $\mathrm X$ 為許多相互獨立隨機變數之加總
>   - 獨立隨機變數之加總的同時，其機率密度函數作**卷積（Convolution）**
>   - 近似於常態分佈
>   - $\mathrm{X～Gaussian}(\mu, \sigma)$
> - 在每個相互獨立的隨機變數之數量非常大的情況下
>   - 其各自的機率分佈不會改變此性質
> - $\mu_{\mathrm X} = \mu \\ \sigma_{\mathrm X} = \sigma$



> 在取機率密度函數值時，需要對 $\sigma$ 取平方與倒數
>
> 在經常需要對不同餐數下的機率密度函數求值時
>
> - 定義 $\beta = \sigma^{-2} \in (0,\infty)$ 為**精度（Percision）**
>   - $P_{\mathrm X}(x) = \sqrt{\frac{\beta}{2\pi}}e^{-\frac12\beta(x-\mu)^2}$

- 常態分佈在自然界很常出現
  - 如：人口身高分佈、體重分佈
- 亦常被用來 model **很多隨機量的總和行為**
  - 如：100 人吃飯時間的總和
  - 來自**「中央極限定理」**





#### Standard normal distribution

$$
\mathrm{X～Gaussian}(\underset \mu0, \underset \sigma1)
$$



- 亦稱作 Unit Gaussian distribution

![1555324513637](\willywangkaa\images\1555324513637.png)

- 機率密度函數（Probability density function）
  - $f_{\mathrm Z}(z) = \frac{1}{\sqrt{2\pi}}e^{-\frac{z^2}{2}}$

![1555324589928](\willywangkaa\images\1555324589928.png)

- 累進分佈函數（Cumulative distribution function）
  - $\Phi(z) = \int_{-\infty}^z\frac{1}{\sqrt{2\pi}}e^{-\frac{u^2}{2}}\mathrm du$
  - 無法取其積分函數，只能以數值方法求出建表供人查詢



![1555324891173](\willywangkaa\images\1555324891173.png)



![1555325058757](\willywangkaa\images\1555325058757.png)

- $\Phi(z)$ 的性質
  - $\Phi(-z) = 1-\Phi(z)$（上圖一）
  - Complementary of standard normal distribution（上圖二）
    - $Q(z) = \mathrm P(\mathrm Z\geq z) =  1-\Phi(z)$



##### 一般 Normal random variable 的 CDF？



- 對任何 $\mathrm{X～Gaussian}(\mu, \sigma)$，$\mathrm{\frac{X-\mu}{\sigma}～Gaussian}(0, 1)$

**證明**（$\mathrm{\frac{X-\mu}{\sigma}}$ 與 $\mathrm{Gaussian}(0, 1)$ 的 CDF 一致）

1. $\mathrm P(\frac{\mathrm X-\mu}{\sigma}\leq z) = \mathrm P(\mathrm X\leq \mu+\sigma z)$
   - $f_{\mathrm X}(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
2. $\int_{-\infty}^{\mu+\sigma z} \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}} \mathrm dx$
   - 令 $w = \frac{x-\mu}{\sigma}$
   - $\mathrm P(\frac{\mathrm X-\mu}{\sigma}\leq z) = \int_{-\infty}^{z} \frac{1}{\sqrt{2\pi}}e^{-\frac{w^2}{2}} \mathrm dw = \Phi(z)$



- 對任何 $\mathrm{X～Gaussian}(\mu, \sigma)$，$F_{\mathrm X}(x) = \Phi(\frac{x-\mu}{\sigma})$

**證明**

$F_{\mathrm X}(x) = \mathrm P(\mathrm X\leq x) \\ = \mathrm P(\underset{～\mathrm{Gaussian(0,1)}}{\frac{X-\mu}{\sigma}}\leq \frac{x-\mu}{\sigma}) \\ = \Phi(\frac{x-\mu}{\sigma})$



- 若 $\left\{\begin{matrix}\mathrm{X～Gaussian}(\mu^{(X)}, \sigma^{(X)}) \\ \mathrm{Y～Gaussian}(\mu^{(Y)}, \sigma^{(Y)}) \end{matrix}\right.$ **相互獨立**
  - 則 $\mathrm{X+Y～Gaussian}(\underset{Mean}{\mu^{(X)}+\mu^{(Y)}}, \underset{Variance}{\sigma^{2(X)}+\sigma^{2(Y)}})$
  - 若 $\mathrm X、\mathrm Y$ **互不獨立則此式不成立**

**證明**

新的隨機變數為 $\mathrm {Z = X+Y}$ 

其對應的機率密度函數 $f_{\mathrm Z}$ 為 $f_{\mathrm X}$ 與 $f_{\mathrm Y}$ 的卷積
$$
f_Z(z)=\int _{-\infty }^{\infty }f_Y(z-x)f_X(x)\mathrm dx
$$

1. $f_{X}(x)=\frac {1}{\sqrt{2\pi}\sigma _{X}}e^{-(x-\mu _X)^2/(2\sigma _X^2)}\\f_{Y}(y)=\frac {1}{\sqrt{2\pi}\sigma _{Y}}e^{-(y-\mu _Y)^2/(2\sigma _Y^2)}$ 
2. $f_Z(z)=\int _{-\infty }^{\infty }f_Y(z-x)f_X(x)\mathrm dx$
   - $f_Z(z)=\int _{-\infty}^{\infty}\frac {1}{\sqrt {2\pi}\sigma _Y}\exp \left[-\frac{(z-x-\mu _Y)^2}{2\sigma _Y^2}\right]\frac {1}{\sqrt{2\pi}\sigma _X}\exp\left[-\frac{(x-\mu_X)^2}{2\sigma_X^2}\right]\mathrm dx \\ =\int _{-\infty}^{\infty}\frac {1}{\sqrt{2\pi}\sqrt{2\pi}\sigma _X\sigma _Y} \exp\left[-\frac{\sigma _X^2(z-x-\mu _Y)^2+\sigma_Y^2(x-\mu _X)^2}{2\sigma _X^2\sigma _Y^2}\right]\mathrm dx \\ =\int _{-\infty }^{\infty }\frac {1}{\sqrt {2\pi}\sqrt {2\pi}\sigma _X\sigma _Y}\exp \left[-\frac {\sigma _X^2(z^2+x^2+\mu _Y^2-2xz-2z\mu _Y+2x\mu _Y)+\sigma _Y^2(x^2+\mu _X^2-2x\mu _X)}{2\sigma _Y^2\sigma _X^2}\right]\mathrm dx \\ =\int _{-\infty}^{\infty }\frac {1}{\sqrt {2\pi}\sqrt {2\pi}\sigma _X\sigma _Y} \exp \left[-\frac {x^2(\sigma _X^2+\sigma _Y^2)-2x(\sigma _X^2(z-\mu _Y)+\sigma _Y^2\mu _X)+\sigma _X^2(z^2+\mu _Y^2-2z\mu _Y)+\sigma _Y^2\mu _X^2}{2\sigma _Y^2\sigma _X^2}\right]\mathrm dx$
3. 令 $\sigma _Z = \sqrt{\sigma _X^2 + \sigma _Y^2}$
   - $f _Z(z) =\int _{-\infty}^{\infty}\frac {1}{\sqrt {2\pi}\sigma _Z}\frac {1}{\sqrt {2\pi}\frac {\sigma _X\sigma _Y}{\sigma _Z}} \exp\left[-{\frac {x^2-2x \frac{\sigma _X^2(z-\mu _Y)+\sigma _Y^2\mu _X}{\sigma _Z^2}+\frac {\sigma _X^2(z^2+\mu _Y^2-2z\mu _Y)+\sigma _Y^2\mu _X^2}{\sigma _Z^2}} { 2\left(\frac {\sigma _X\sigma _Y}{\sigma _Z}\right)^2} }\right]\mathrm dx \\ = \int _{-\infty }^{\infty }\frac {1}{\sqrt{2\pi}\sigma _Z}\frac {1}{\sqrt {2\pi} \frac{\sigma _X\sigma _Y}{\sigma _Z}} \exp\left[-\frac{\left(x-\frac {\sigma _X^2(z-\mu _Y)+\sigma _Y^2\mu _X}{\sigma _Z^2}\right)^2-\left(\frac {\sigma _X^2(z-\mu _Y)+\sigma _Y^2\mu _X}{\sigma _Z^2}\right)^2+\frac {\sigma _X^2(z-\mu _Y)^2+\sigma _Y^2\mu _X^2}{\sigma _Z^2}} {2\left(\frac {\sigma _X\sigma _Y}{\sigma _Z}\right)^2}\right]\mathrm dx \\ = \int _{-\infty}^{\infty}\frac {1}{\sqrt {2\pi}\sigma _Z} \exp\left[-\frac {\sigma _Z^2\left(\sigma _X^2(z-\mu _Y)^2+\sigma _Y^2\mu _X^2\right)-\left(\sigma _X^2(z-\mu _Y)+\sigma _Y^2\mu _X\right)^2}{2\sigma _Z^2\left(\sigma _X\sigma _Y\right)^2}\right]\frac {1}{\sqrt {2\pi}{\frac{\sigma _X\sigma _Y}{\sigma _Z}}} \exp\left[-\frac{\left(x-\frac {\sigma _X^2(z-\mu _Y)+\sigma _Y^2\mu _X}{\sigma _Z^2}\right)^2}{2\left(\frac {\sigma _X\sigma _Y}{\sigma _Z}\right)^2}\right]\mathrm dx \\ = \frac {1}{\sqrt{2\pi}\sigma _Z} \exp \left[-\frac{(z-(\mu _X+\mu _Y))^2 }{ 2\sigma _Z^2}\right] \underline{\int _{-\infty }^{\infty }\frac {1}{\sqrt {2\pi}\frac {\sigma _X\sigma _Y}{\sigma _Z}}\exp \left[-\frac {\left(x- \frac {\sigma _X^2(z-\mu _Y)+\sigma _Y^2\mu _X}{\sigma _Z^2}\right)^2}{2\left(\frac {\sigma _X\sigma _Y}{\sigma _Z}\right)^2}\right]\mathrm dx}$ 
4. 上述底線項為對隨機變數 $X$ 的機率密度函數作積分，所以其等於 1
   - $f _Z(z) = \frac {1}{\sqrt{2\pi}\sigma _Z} \exp \left[-\frac{(z-(\mu _X+\mu _Y))^2 }{ 2\sigma _Z^2}\right]$





**Example**

- 已知 50 位同學拉贊助總金額 $\mathrm X$ 之機率分佈為 $\mathrm {Gaussian(50000,1000)}$
- 問本月業績少於 25000 之機率為？

$F_{\mathrm X}(25000) = \mathrm P(\mathrm X\leq 25000) \\ = \mathrm P(\frac{X-\mu}{\sigma}\leq \frac{25000-\mu}{\sigma}) = \mathrm P(\frac{X-\mu}{\sigma}\leq \frac{25000-50000}{1000}) \\ = \Phi(-25)$



#### 信賴區間



![1555329512233](\willywangkaa\images\1555329512233.png)

-  在抽樣的情況下，有 **95% 信心水準**其母體的**期望值**會落在**信賴區間** $[\mu-2\sigma, \mu+2\sigma]$
  - [信賴區間與信心水準的解讀](https://www.learnmode.net/upload/flip/book/e9/e9694f1cae726b07/926608d6e409.pdf)

 

#### 為何高斯分佈會如此受歡迎？



1. 可 model 複雜的系統

   - 根據中央極限定理

   - 其隨機變數代表系統許多隨機變數加總而成
   - [白雜訊- 維基百科](https://zh.wikipedia.org/zh-tw/%E7%99%BD%E9%9B%9C%E8%A8%8A)（Gaussian white noise）
     - 訊號在各個頻段上的功率一致

2. 在相同變異數的所有**機率分佈**中

   - 常態分佈具有最大的**「不確定性」（Uncertainty）**
   - 是**對模型加入最少的前驗知識（Priori knowledge）之分佈**
     - 之後解說

3. 計算友善

   - 連續、可微分…等



### Multinoulli distribution（Discrete）

> 一次實驗 $k$ 種結果，「$k-1$ 個機率」在意 $i–th$ 結果發生與否
>
> $k–th$ 結果發生的機率：$1-\sum_{i = 1}^{k-1} p_i$ （這個結果換言之就是前 $k-1$ 個結果都不發生；相較於「Bernoulli distribution」就是失敗事件）

$$
\mathrm {X～CAT}(p_1, p_2,\ldots,p_{k-1})
$$



> 又稱為**「範疇分佈」（Categorical distribution）**

- 機率質量函數（Probability mass distribution）
  - 一個隨機變數 $\mathrm X$ 具有 $k$ 個不同狀態
  - 給定每個狀態成功發生的機率 $P(success \;of\; state_i) = p_i$
  - 只作一次試驗，$\mathrm X = 成功發生的狀態編號$
  - $P_{\mathrm X}(x) = \left\{\begin{matrix}p_i, & x = i \\ 1-\sum_{i = 1}^{k-1} p_i, & x = 0 \\ 0, &otherwise\end{matrix}\right.$

> 有時候每個狀態成功發生的機率可以一個向量 $\boldsymbol p \in [0,1]^{k-1}$ 參數化
> $$
> \mathrm {X～CAT}(\boldsymbol p)
> $$
>
>
> - 機率質量函數（Probability mass distribution）
>   - $P_{\mathrm X}(x) = \left(\prod_{i = 1}^{k-1}p_i^{1(x = i)}\right)\cdot\left(1-\sum_{i = 1}^{k-1} p_i\right)^{1(x = 0)}$
>   - 其中 $1(x = i)$ 為指示函數（Indecator function），當 $x$ 等於 $i$ 時等於 1，反之等於 0
>   - 令 $\boldsymbol 1 = \begin{bmatrix}1 \\ 1 \\ \vdots \\ 1\end{bmatrix} \in \mathbb R^{k-1}$
>     - 則 $1^T\boldsymbol p \leq 1$



- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(x) = \left\{\begin{matrix} 0,  &x<0 \\ 1-\sum_{i = 1}^{k-1} p_i , & 0\leq x <1\\ 1-\sum_{i = q+1}^{k-1} p_i, & q\leq x<q+1 \\ 1, & x\geq k\end{matrix}\right.$

> 另外，「Categorical distribution」也可如此表示
>
> - 一次實驗 $k$ 種結果，「$k$ **個機率**」在意 $i–th$ 結果發生與否
>
> - 機率質量函數（機率向量 $\boldsymbol p \in [0,1]^k$）
>   - $P_{\mathrm X}(x) = \prod_{i = 1}^kp^{1(x = i)}$
>   - 隨機變數 $\mathrm X$ 為「成功發生狀態編號」
>   - $1^T\boldsymbol p = 1$ 



### Multinomial distribution（Discrete）

> 共 $n$ 次實驗，$k–1$ 個機率，在意 $n$ 次實驗中 $狀態_i$ 有出現 $r_i$ 次之機率

$$
\mathrm {X～Multi}(n,\;p_1,p_2,\ldots,p_{k-1}) \\ or \; \mathrm {X～Multi}(n,\; \boldsymbol p)
$$

- 機率質量函數（Probability mass distribution）
  - 一個隨機變數 $\mathrm X$ 具有 $k$ 個不同狀態
  - 給定每個狀態成功發生的機率，並向量 $\boldsymbol p \in [0,1]^{k-1}$ 表示之
  - 作 $n$ 次實驗，$\mathrm X = \boldsymbol x \in \mathbb R^{k-1}$
    - 向量中每個元素 $x_i = r_i = 狀態_i\;成功發生的次數$
  - $P_{\mathrm X}(\boldsymbol x) = \left\{\begin{matrix} \binom{n}{r_1,\ldots,r_{k-1}}\left(\prod_{i = 1}^{k-1} p_i^{r_i}\right)\cdot\left(1-\sum_{i = 1}^{k-1} p_i\right)^{n-\sum_{j = 1}^{k-1}r_j}, & \sum_{i = 1}^{k-1}r_i \leq n \\ 0, &otherwise\end{matrix}\right.$
    - $＃｛success \;of \;state_i｝ = r_i = x_i$
    - $＃｛failure \;of \;all\;state｝ = n-\sum_{j = 1}^{k-1}r_j$



- 累進分佈函數（Cumulative distribution function）
  - $F_{\mathrm X}(\boldsymbol x) = \sum_{\boldsymbol m = -\infty}^{\lfloor \boldsymbol x\rfloor}P_{\mathrm X}(\boldsymbol m) \\= \sum_{\boldsymbol m = -\infty}^{\lfloor \boldsymbol x\rfloor} \binom{n}{r_1,\ldots,r_{k-1}}\left(\prod_{i = 1}^{k-1} p_i^{r_i}\right)\cdot\left(1-\sum_{i = 1}^{k-1} p_i\right)^{n-\sum_{j = 1}^{k-1}r_j}$ 
    -  $\boldsymbol m = -\infty = \begin{bmatrix}r_1\\\vdots\\ r_{k-1}\end{bmatrix} =\begin{bmatrix}0\\\vdots\\ 0\end{bmatrix}$
    -  $\lfloor \boldsymbol x\rfloor = \begin{bmatrix}\lfloor x_1\rfloor\\\vdots\\ \lfloor x_{k-1}\rfloor\end{bmatrix}$



> 另外，「Multinomial distribution」也可這樣表示
>
> - 共 $n$ 次實驗，$k$ **個機率**，在意 $n$ 次實驗中 $狀態_i$ 有出現 $r_i$ 次之機率
>
> - 機率質量函數（機率向量 $\boldsymbol p \in [0,1]^k$）
>   - $P_{\mathrm X}(\boldsymbol x)\binom{n}{r_1,\ldots,r_{k}}\left(\prod_{i = 1}^k p_i^{r_i}\right)$
>   - 隨機變數 $\mathrm X = \boldsymbol x \in \mathbb R^k$
>     - 向量中每個元素 $x_i = r_i = 狀態_i\;成功發生的次數$
>   - $\sum_{i = 1}^k r_i = n$
>   - $1^T\boldsymbol p = 1$
>
>
>
> **性質**
>
> - **期望值（Mean）**
>   - $\mathrm E(X) = n\boldsymbol p$
>
>
>
> 1. 令隨機變數 $\mathrm X = \boldsymbol x \in \mathbb R^k$
>    - 向量中每個元素 $x_i = r_i = 狀態_i\;成功發生的次數$
>    - 則 $\boldsymbol m$ 為一種狀態出現的可能
>    -  $\boldsymbol X = \begin{bmatrix} m_1 \\ \vdots \\ m_{k} \end{bmatrix} = \boldsymbol m$ 出現的機率定義為
>      - $P_{\mathrm X}(\boldsymbol m) = \binom n{\boldsymbol m}p^{\boldsymbol m}$
>      - $\binom n{\boldsymbol m} = \frac {n}{m_1!\ldots m_k!}$
>      - $p^{\boldsymbol m} = p_1^{m_1}\ldots p_k^{m_k}$
> 2. 展開 $k$ 次多項式
>    - $1 = 1^n = (p_1+\ldots+p_k)^n = \sum_{\boldsymbol m}\binom n{\boldsymbol m}p^{\boldsymbol m}$
>
> 3. 先求 $\mathrm E(X_1) = \sum_{x_1 = 0}^n x_1\mathrm P(X_1 = x_1)$
>    - 當 $x_1 = 0$ 時，$x_1\mathrm P(X_1 = x_1)$ 等於 0
>      - $\Rightarrow \sum_{x_1 = 0}^n x_1\mathrm P(X_1 = x_1) = \sum_{x_1 = 1}^n x_1\mathrm P(X_1 = x_1)$
>      - $\Rightarrow \sum_{x_1 = 1}^n x_1\sum_{x_2,\ldots,x_k} \mathrm P(X_2 = x_2,\ldots,X_k = x_k)\\, \forall x_2+\ldots+x_k = n-x_1$
>      - $\Rightarrow \sum_{x_1 = 1}^n x_1\sum_{x_2,\ldots,x_k} \frac{n!}{x_1!\ldots x_k!}p_1^{x_1}\ldots p_k^{x_k}$
>    - 將 $x_1$ 乘進 $\sum_{x_2,\ldots,x_k} \frac{n!}{x_1!\ldots x_k!}p_1^{x_1}\ldots p_k^{x_k}$
>      - $\Rightarrow \sum_{x_2,\ldots,x_k} \frac{n\cdot (n-1)!}{(x_1-1)!\ldots x_k!}p_1\cdot p_1^{x_1-1}\ldots p_k^{x_k}$
>      - 已知 $x_1+x_2+\ldots+x_k = n$ 令 $x_1' = x_1-1$
>      - $x_1'+x_2+\ldots+x_k = n-1$
>      - $\Rightarrow np_1\sum_{x_1' = 0}^n \sum_{x_2,\ldots,x_k} \frac{(n-1)!}{x_1'!\ldots x_k!}p_1^{x_1'}\ldots p_k^{x_k} \\ ,\forall x_1'+\ldots+x_k = n-1$
>      - $\Rightarrow np_1\sum_{x_1',x_2,\ldots,x_k} \frac{(n-1)!}{x_1'!\ldots x_k!}p_1^{x_1'}\ldots p_k^{x_k} \\ , \forall x_1'+\ldots+x_k = n-1$
>      - $\Rightarrow np_1\cdot1 = np_1$
> 4. $\mathrm E(\boldsymbol X) = n\begin{bmatrix}p_1 \\ p_2 \\ \vdots \\ p_k \end{bmatrix} = n\boldsymbol p$
>
> - **變異數（Variance）**
>   - $\mathrm {Var}(\boldsymbol X) = n(\mathrm{diag}(\boldsymbol p)-\boldsymbol {pp}^T)$
>   - $\mathrm{Var}(X_i) = np_i(1-p_i)$
>   - $\mathrm{Cov}(x_i,x_j) = -np_ip_j$
>
>
>
> 1. $\mathrm{Var}(X_i) = \mathrm E((X_i-\mu_{X_i})^2) = \mathrm E(X_i^2)-\underset{n^2p_i^2}{[\mathrm E(X_i)]^2}$
>    - 先從 $\mathrm{Var}(X_1) = \mathrm E(X_1^2)-\underset{n^2p_1^2}{[\mathrm E(X_1)]^2}$ 討論
> 2. $\mathrm E(X_1^2) = \sum_{x_1 = 0}^n x_1^2 \mathrm P(X_1 = x_1)$
>    - $\Rightarrow \sum_{x_1 = 1}^n x_1^2 \mathrm P(X_1 = x_1) = \sum_{x_1 = 1}^n x_1^2\sum_{x_2,\ldots,x_k}\frac{n!}{x_1!\ldots x_k!}p_1^{x_1}\ldots p_k^{x_k}$
>      - $x_2+\ldots+x_k = n-x_1$
>    - $\Rightarrow \sum_{x_1 = 1}^n x_1 \sum_{x_2,\ldots,x_k}\frac{n!}{(x_1-1)!\ldots x_k!}p_1^{x_1}\ldots p_k^{x_k}$
>    - $\Rightarrow \sum_{x_1' = 0}^{n-1} (x_1'+1) \sum_{x_2,\ldots,x_k}\frac{n!}{x_1'!\ldots x_k!}p_1^{x_1'+1}p_2^{x_2}\ldots p_k^{x_k}$
>      - $x_1' = x_1-1 \\ x_1 = x_1'+1\\x_2+\ldots+x_k  = n - x_1'-1$
>    - $\Rightarrow \sum_{x_1' = 0}^{n-1} (x_1') \sum_{x_2,\ldots,x_k}\frac{n!}{x_1'!\ldots x_k!}p_1^{x_1'+1}p_2^{x_2}\ldots p_k^{x_k} \\\quad + \sum_{x_1' = 0}^{n-1} \sum_{x_2,\ldots,x_k}\frac{n!}{x_1'!\ldots x_k!}p_1^{x_1'+1}p_2^{x_2}\ldots p_k^{x_k}$
>    - $\Rightarrow np_1 \sum_{x_1',x_2,\ldots,x_k}x_1'\frac{(n-1)!}{x_1'!\ldots x_k!}p_1^{x_1'}p_2^{x_2}\ldots p_k^{x_k} \\\quad + np_1 \sum_{x_1',x_2,\ldots,x_k}\frac{(n-1)!}{x_1'!\ldots x_k!}p_1^{x_1'}p_2^{x_2}\ldots p_k^{x_k}$
>      - $x_1'+x_2+\ldots+x_k  = n-1$
>      - 此式等價於 $np_1(n-1次實驗下的期望值)+np_1(n-1次實驗下的全部機率和) \\ = np_1(n-1)p_1+np_1\cdot1$
> 3. $\mathrm{Var}(X_1) = \mathrm E(X_1^2)- n^2p_1^2 \\= np_1(n-1)p_1+np_1 - n^2p_1^2 = np_1(1-p_1)$
>
>
>
> 1. $\mathrm{Cov}(X_i, X_j) = \mathrm E(X_iX_j)-\mathrm E(X_i)\mathrm E(X_j) \\ = \mathrm E(X_iX_j)-n^2p_ip_j$
>    - 先從 $\mathrm E(X_1X_2)-n^2p_1p_2$ 討論
> 2. $\mathrm E(X_1X_2) = \sum_{x_1,x_2}x_1x_2\mathrm P(X_1 = x_1,X_2 = x_2)$
>    - $\Rightarrow \sum_{x_1,x_2}x_1x_2\frac{n!}{x_1!x_2!\ldots x_r!}p_1^{x_1}\ldots p_k^{x_k}$
>      - $x_1+x_2+\ldots+x_k = n$
>    - $\Rightarrow p_1p_2\sum_{x_1,x_2}\frac{n!}{(x_1-1)!(x_2-1)!\ldots x_r!}p_1^{x_1-1}p_2^{x_2-1}\ldots p_k^{x_k}$
>    - $\Rightarrow p_1p_2\sum_{x_1',x_2',\ldots,x_k} \frac{n!}{x_1'!x_2'!\ldots x_r!}p_1^{x_1'}p_2^{x_2'}\ldots p_k^{x_k}$
>      - $x_1' = x_1-1 \\ x_2' = x_2-1\\ x_1'+x_2'+\ldots+x_k = n-2$
>    - $\Rightarrow n(n-1)p_1p_2\sum_{x_1',x_2',\ldots,x_k} \frac{(n-2)!}{x_1'!x_2'!\ldots x_r!}p_1^{x_1'}p_2^{x_2'}\ldots p_k^{x_k} \\= n(n-1)p_1p_2$
> 3. $\mathrm{Cov}(X_i, X_j)=n(n-1)p_1p_2-n^2p_ip_j = -np_1p_2$