title: Deep learning - Probability and Information theories IV
author: Willy Wang (willywangkaa)
tags:
  - Probability
  - Information theories
categories:
  - Deep learning
date: 2019-04-19 21:19:00
---
# Deep learning - Probability and Information theories IV



## Common probability distribution



### Multivariate Gaussian distribution（多元常態分佈；Continuous）

> $\mathrm{X～Gaussian}(\mu, \sigma)\quad f_{\mathrm X}(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
>
> 常態分佈亦可推廣至 $\mathbb R^n$ 空間，稱作**多元常態分佈**

$$
\mathrm {X～Gaussian}(\boldsymbol \mu, \Sigma)
$$

- 機率密度函數（Probability density function）
  - $f_X(\boldsymbol x) = \sqrt{\frac{1}{(2\pi)^d\det(\Sigma)}}e^{-\frac12(\boldsymbol x-\boldsymbol \mu)^T\Sigma^{-1}(\boldsymbol x-\boldsymbol \mu)}$

> - $\boldsymbol \mu = \begin{bmatrix}\mu_1 \\ \vdots \\ \mu_d\end{bmatrix} \in \mathbb R^d$ 為平均數向量
> - $\Sigma \in \mathbb R^{n\times n}$ 共變異數矩陣（Covariance matrix）  

- 若 $\mathrm {X～Gaussian}(\boldsymbol \mu, \Sigma)$
  - 則每個隨機變數 $X_i$ 為單變量常態分佈
  - **反向不成立**
- 但是若 $X_1, \ldots, X_d$ 為 i.i.d. 且 $X_i～\mathrm{Gaussian}(\mu_i,\sigma_i)$
  - 則 $\mathrm {X～Gaussian}(\boldsymbol \mu, \Sigma)$
  - $\boldsymbol \mu = \begin{bmatrix}\mu_1 \\ \vdots \\ \mu_d\end{bmatrix}$
  - $\Sigma = \mathrm{diag}(\sigma_1^2,\ldots,\sigma_d^2)$



> **運用矩陣分解可以從另一個角度認識常態分布和共變異數矩陣**
>
> - $\Sigma$ 的正交對角化表達式為 $\Sigma=Q\Lambda Q^T=BB^T$
>   - 其中 $B=Q\Lambda^{1/2}$ 稱為極分解（見“[極分解](https://ccjou.wordpress.com/2009/09/09/%E6%A5%B5%E5%88%86%E8%A7%A3/)”）
>   - $Q$ 是一個正交矩陣表示**旋轉或鏡射**
>   - $\Lambda^{1/2}=\hbox{diag}(\sqrt{\lambda_1},\ldots,\sqrt{\lambda_n})$ 是一個正定矩陣表示**伸縮**
> - 馬氏距離可表示成
>   - $\Delta^2=(\mathbf{x}-\boldsymbol{\mu})^T(B^{-1})^TB^{-1}(\mathbf{x}-\boldsymbol{\mu}) \\=\Vert B^{-1}(\mathbf{x}-\boldsymbol{\mu})\Vert^2=\Vert\Lambda^{-1/2}Q^T(\mathbf{x}-\boldsymbol{\mu})\Vert^2$
>   - $\mathbf{z}=\Lambda^{-1/2}Q^T(\mathbf{x}-\boldsymbol{\mu})$，即有 $\Delta^2=\mathbf{z}^T\mathbf{z}$
>     - 隨機向量 $\mathbf{z}$ 的機率密度函數
>     - $p(\mathbf{z})=\frac{1}{(2\pi)^{n/2}}\exp\left\{-\frac{1}{2}\mathbf{z}^T\mathbf{z}\right\}=\prod_{i=1}^n\frac{1}{\sqrt{2\pi}}\exp\left\{-\frac{z_i^2}{2}\right\}$
>       - 稱為標準常態分布
>       - 平均數向量是 $\boldsymbol{\mu}=\mathbf{0}$
>       - 共變異數矩陣是 $\Sigma=I$
>   - 一般常態分布的隨機向量 $\mathbf{x}$其生成過程可表示為**仿射變換**
>     - $\mathbf{x}=Q\Lambda^{1/2}\mathbf{z}+\boldsymbol{\mu}$
>     - 先伸縮標準常態分布的隨機向量 $\mathbf{z}$ 各個變數（乘以 $\Lambda^{1/2}$）
>     - 再旋轉（乘以 $Q$）
>     - 最後平移（加上 $\boldsymbol{\mu}$），**如下圖**
>
> ![1555650665915](\willywangkaa\images\1555650665915.png)
>
> **共變異數矩陣** $\Sigma$ **的作用在於決定常態分布的伸縮** $\Lambda^{1/2}$ **和旋轉** $Q$



![1555650853286](\willywangkaa\images\1555650853286.png)

- 隨著 $\mathrm{Cov}[X_1,X_2]$ 增長，會將等高線沿著  45 度伸長
- 隨著 $\mathrm{Cov}[X_1,X_2]$ 減少，會將等高線沿著 -45 度伸長

![1555651404197](\willywangkaa\images\1555651404197.png)

- 上圖高度為 $f_X(\boldsymbol x) = \sqrt{\frac{1}{(2\pi)^d\det(\Sigma)}}e^{-\frac12(\boldsymbol x-\boldsymbol \mu)^T\Sigma^{-1}(\boldsymbol x-\boldsymbol \mu)}$ 的輸出
  - 高度與馬氏距離成反比
  - 若 $\Sigma=\sigma^2I$ 則該常態分佈具有**等向性（Isotropic）**



#### 特性

- 若 $\mathrm {X～Gaussian}(\boldsymbol \mu, \Sigma)$ 
  - 對於任意常數向量 $\mathbf w\in\mathbb R^d$
  - $\mathrm {w^TX～Gaussian}(\mathbf w^T\boldsymbol \mu, \mathbf w^T\Sigma\mathbf w)$
- 廣義的說給定 $\mathbf W\in \mathbb R^{d\times k}, k\leq d$
  - $\mathbf W^T\mathrm {X～Gaussian}(\mathbf W^T\boldsymbol \mu, \mathbf W^T\Sigma\mathbf W)$ 為 $k$ 變量常態分佈
  - 將隨機向量 $\mathbf x$ 投影至 $k$ 為空間**仍然為常態分佈**



#### Mahalanobis distance（馬氏距離）

**常態分佈**的機率密度函數由下列二次型決定：
$$
\Delta^2 = (\boldsymbol x-\boldsymbol\mu)^T\Sigma^{-1}(\boldsymbol x-\boldsymbol\mu)
$$

- $\Delta$ 稱為 $\boldsymbol \mu$ 與 $\boldsymbol x$ 的馬氏距離
- 若 $\Sigma = I_d$ ，則 $\Sigma^2 = \Vert \boldsymbol x -\boldsymbol \mu\Vert^2$
  - 馬氏距離退化為歐氏距離（Euclidean distance）
- 通過解析馬氏距離的二次型表達式
  - 可以瞭解常態分布的**幾何型態**

> 不失一般性原則下，假設 $\Sigma$ 為一個實對稱矩陣
>
> - 考慮特徵方程  $\Sigma\mathbf{q}_i=\lambda_i\mathbf{q}_i$
> -  $\Vert\mathbf{q}_i\Vert=1$，$i=1,\ldots,d$
> - 實對稱矩陣可正交對角化（[實對稱矩陣可正交對角化的證明](https://ccjou.wordpress.com/2011/02/09/%E5%AF%A6%E5%B0%8D%E7%A8%B1%E7%9F%A9%E9%99%A3%E5%8F%AF%E6%AD%A3%E4%BA%A4%E5%B0%8D%E8%A7%92%E5%8C%96%E7%9A%84%E8%AD%89%E6%98%8E/)）
>   - 令 $Q=\begin{bmatrix}  \mathbf{q}_1&\cdots&\mathbf{q}_d  \end{bmatrix}$ 且 $\Lambda=\mathrm{diag}(\lambda_1,\ldots,\lambda_d)$
>   - $\Sigma=Q\Lambda Q^T=\begin{bmatrix}  \mathbf{q}_1&\cdots&\mathbf{q}_d  \end{bmatrix}\begin{bmatrix}  \lambda_1&&\\  &\ddots&\\  &&\lambda_d  \end{bmatrix}\begin{bmatrix}  \mathbf{q}_1^T\\  \vdots\\  \mathbf{q}_d^T \end{bmatrix}=\displaystyle\sum_{i=1}^d\lambda_i\mathbf{q}_i\mathbf{q}_i^T$

將正交對角化分解後的式子代入馬氏距離公式
$$
\Delta^2=(\mathbf{x}-\boldsymbol{\mu})^TQ\Lambda^{-1}Q^T(\mathbf{x}-\boldsymbol{\mu})=\mathbf{y}^T\Lambda^{-1}\mathbf{y}=\displaystyle\sum_{i=1}^d\frac{y_i^2}{\lambda_i}
$$

- 令  $\mathbf{y}=Q^T(\mathbf{x}-\boldsymbol{\mu})$
- $\mathbf{x}-\boldsymbol{\mu}=Q\mathbf{y}=\begin{bmatrix}  \mathbf{q}_1&\cdots&\mathbf{q}_d  \end{bmatrix}\begin{bmatrix}  y_1\\  \vdots\\  y_d  \end{bmatrix}=y_1\mathbf{q}_1+\cdots+y_d\mathbf{q}_d$
  - $\mathbf{x}-\boldsymbol{\mu}$ 在**參考基底** $\{\mathbf{q}_1,\ldots,\mathbf{q}_d\}$ 上的座標（向量）為 $\mathbf y=\begin{bmatrix}  y_1\\  \vdots\\  y_d  \end{bmatrix}$
  -  可以解讀為 $\mathbf{y}$ 至 $\mathbf{x}$ 的仿射變換
    - $\mathbf{y}$ 經過旋轉或鏡射 $Q$，再平移 $\boldsymbol{\mu}$ 得到 $\mathbf{x}$



#### Contour line（等高線）

> 透過**等高線**可**視覺化常態分布的型態**
>
> 方便說明考慮 $d=2$ 的情形

- 若 $\Delta=1$，馬氏距離公式給出
  - $\left(\frac{y_1}{\sqrt{\lambda_1}}\right)^2+\left(\frac{y_2}{\sqrt{\lambda_2}}\right)^2=1$

- 如果 $\lambda_1\geq\lambda_2>0$，在新座標系統 $(y_1,y_2)$ 中（下圖）
  - 等高線的軌跡為一個標準橢圓
    - 長軸（即 $y_1$ 軸）半徑等於 $\sqrt{\lambda_1}$
    - 短軸（即 $y_2$ 軸）半徑等於 $\sqrt{\lambda_2}$
  - 在標準座標系統 $(x_1,x_2)$
    - 特徵向量 $\mathbf{q}_1$ 指向長軸方向
    - $\mathbf{q}_2$ 指向短軸方向
  - 橢圓上的任何一個點 $\mathbf{x}$ 至 $\boldsymbol{\mu}$ 的馬氏距離都等於 $1$

![1555580591425](\willywangkaa\images\1555580591425.png)



> 在實作上經常**限制共變異數矩陣的型態**
>
> - **（a）**一般共變異數矩陣
> - **（b）**共變異數矩陣是對角矩陣
>   -  $\Sigma=\mathrm{diag}(\sigma_1^2,\ldots,\sigma_n^2)$
>   - $\sigma_i^2$ 代表隨機變數 $X_i$ 的變異數
> - **（c）**所有隨機變數 $X_i$ 有相同的共變異數
>   -  $\Sigma=\sigma^2I$
>
> ![1555580920252](\willywangkaa\images\1555580920252.png)



#### Moment（動差）

> **考慮單變量常態分佈的動差：**
>
> - 令隨機變數 $W = X-\mu$
>   - 則 $W～\mathrm{Gaussian}(0,\sigma)$ 的機率密度函數會對稱於 $W = 0$
> - 期望值公式為 $\mathrm E[X] = \int_{-\infty}^{\infty}xf_X(x)\mathrm dx$
>   - $\text{E}[X]=\frac{1}{\sqrt{2\pi}\sigma}\int\exp\left\{-\frac{(x-\mu)^2}{2\sigma^2}\right\}xdx\\  =\frac{1}{\sqrt{2\pi}\sigma}\int\exp\left\{-\frac{w^2}{2\sigma^2}\right\}(w+\mu)dw\\=\frac{1}{\sqrt{2\pi}\sigma}(\int\exp\left\{-\frac{w^2}{2\sigma^2}\right\}wdw+\mu\int\exp\left\{-\frac{w^2}{2\sigma^2}\right\}dw.....（＊）\\  =\mu\frac{1}{\sqrt{2\pi}\sigma}\int\exp\left\{-\frac{w^2}{2\sigma^2}\right\}dw=\mu$
>     - （＊）式中的指數函數 $\exp$ 為 $w$ 的偶函數，乘上 $w$ 後變成奇函數，且積分範圍為 $(-\infty, \infty)$，根據對稱性前式為 0
> - 變異數公式為 $\mathrm{Var}(X) = \mathrm E[(X-\mu)^2] = \int_{-\infty}^{\infty}(x-\mu)^2f_X(x)\mathrm dx \\=\int_{-\infty}^{\infty}\frac{(x-\mu)^2}{\sqrt{2\pi}\sigma}\exp\left\{-\frac{(x-\mu)^2}{2\sigma^2}\right\}\mathrm dx$
>   - **已知** $\int\exp\left\{-\frac{(x-\mu)^2}{2\sigma^2}\right\}dx=\sqrt{2\pi}\sigma$
>   - 對 $\sigma$ 求導數可得 $\int\frac{(x-\mu)^2}{\sigma^3}\exp\left\{-\frac{(x-\mu)^2}{2\sigma^2}\right\}dx=\sqrt{2\pi}$
>     - 將上式兩側同乘以 $\frac{\sigma^2}{\sqrt{2\pi}}$
>     - 可得 $\int_{-\infty}^{\infty}\frac{(x-\mu)^2}{\sqrt{2\pi}\sigma}\exp\left\{-\frac{(x-\mu)^2}{2\sigma^2}\right\}\mathrm dx = \mathrm E[(X-\mu)^2] = \sigma^2$
>
> **說明單變量常態分布的「參數」** $\mu$ **是平均數，**$\sigma^2$ **是變異數**

**討論多變量常態分布的動差：**

- 令隨機變數 $W = X-\boldsymbol\mu$
- 期望值公式為 $\mathrm E[X] = \int_{-\infty}^{\infty}\boldsymbol xf_X(\boldsymbol x)\mathrm dx$
  - $\text{E}[\mathbf{X}]=\frac{1}{(2\pi)^{n/2}\vert\Sigma\vert^{1/2}}\int\exp\left\{-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T\Sigma^{-1}(\mathbf{x}-\boldsymbol{\mu})\right\}\mathbf{x}d\mathbf{x}\\  =\frac{1}{(2\pi)^{n/2}\vert\Sigma\vert^{1/2}}\int\exp\left\{-\frac{1}{2}\mathbf{w}^T\Sigma^{-1}\mathbf{w}\right\}(\mathbf{w}+\boldsymbol{\mu})d\mathbf{w}\\  =\frac{1}{(2\pi)^{n/2}\vert\Sigma\vert^{1/2}}\left(\int\exp\left\{-\frac{1}{2}\mathbf{w}^T\Sigma^{-1}\mathbf{w}\right\}\mathbf{w}d\mathbf{w}+\boldsymbol{\mu}\int\exp\left\{-\frac{1}{2}\mathbf{w}^T\Sigma^{-1}\mathbf{w}\right\}d\mathbf{w}\right)$
  - 指數函數 $\exp$ 是 $\mathbf{w}$ 的偶函數，且積分範圍是 $(-\infty,\infty)$，根據對稱性可知上式第一項等於零，故得
    - $\text{E}[\mathbf{x}]=\boldsymbol{\mu}\int\mathcal{N}(\mathbf{w}\vert\mathbf{0},\Sigma)d\mathbf{w}=\boldsymbol{\mu}$
    - 因此稱 $\boldsymbol{\mu}$ 是常態分布的平均數向量

> **考慮二階動差**
>
> - 對於單變量二階動差
>   - 由 $\text{E}[X^2]$ 給定
> - 對於多變量
>   - 共有 $n^2$ 個二階動差 $\text{E}[X_iX_j]$，$i,j=1,\ldots,n$
>   - 因為期望值是**線性運算**，所有的二階動差可合併為一個 $n\times n$ 階矩陣 $\text{E}[\mathbf{x}\mathbf{x}^T]$

$$
\displaystyle \begin{aligned}  \text{E}\left[\mathbf{x}\mathbf{x}^T\right]&=\frac{1}{(2\pi)^{n/2}\vert\Sigma\vert^{1/2}}\int\exp\left\{-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T\Sigma^{-1}(\mathbf{x}-\boldsymbol{\mu})\right\}\mathbf{x}\mathbf{x}^Td\mathbf{x}\\  &=\frac{1}{(2\pi)^{n/2}\vert\Sigma\vert^{1/2}}\int\exp\left\{-\frac{1}{2}\mathbf{w}^T\Sigma^{-1}\mathbf{w}\right\}(\mathbf{w}+\boldsymbol{\mu})(\mathbf{w}+\boldsymbol{\mu})^Td\mathbf{w}.  \end{aligned}
$$

- $(\mathbf{w}+\boldsymbol{\mu})(\mathbf{w}+\boldsymbol{\mu})^T=\mathbf{w}\mathbf{w}^T+\mathbf{w}\boldsymbol{\mu}^T+\boldsymbol{\mu}\mathbf{w}^T+\boldsymbol{\mu}\boldsymbol{\mu}^T$
  - 根據對稱性，交互項 $\mathbf{w}\boldsymbol{\mu}^T$ 和 $\boldsymbol{\mu}\mathbf{w}^T$ 的積分等於零
  -  $\boldsymbol{\mu}\boldsymbol{\mu}^T$ 可提出，剩下的機率密度函數積分等於 $1$
    - $\boldsymbol{\mu}\boldsymbol{\mu}^T\int\frac{1}{(2\pi)^{n/2}\vert\Sigma\vert^{1/2}}\exp\left\{-\frac{1}{2}\mathbf{w}^T\Sigma^{-1}\mathbf{w}\right\}d\mathbf{w}=\boldsymbol{\mu}\boldsymbol{\mu}^T$
  - 考慮包含 $\mathbf{w}\mathbf{w}^T$ 的積分
    - $\Sigma=Q\Lambda Q^T=\begin{bmatrix}  \mathbf{q}_1&\cdots&\mathbf{q}_d  \end{bmatrix}\begin{bmatrix}  \lambda_1&&\\  &\ddots&\\  &&\lambda_d  \end{bmatrix}\begin{bmatrix}  \mathbf{q}_1^T\\  \vdots\\  \mathbf{q}_d^T \end{bmatrix}=\displaystyle\sum_{i=1}^d\lambda_i\mathbf{q}_i\mathbf{q}_i^T$
    - 令 $\mathbf{v}=Q^T\mathbf{w}$， $\mathbf{w}=Q\mathbf{v}=\sum_{i=1}^nv_i\mathbf{q}_i$
      - 則 $\mathbf{w}\mathbf{w}^T=Q\mathbf{v}\mathbf{v}^TQ^T=\sum_{i=1}^n\sum_{j=1}^nv_iv_j\mathbf{q}_i\mathbf{q}_j^T$
      - $\mathbf{w}^T\Sigma^{-1}\mathbf{w}=\mathbf{v}^TQ^T\Sigma^{-1} Q\mathbf{v}=\mathbf{v}^T\Lambda^{-1}\mathbf{v}=\sum_{k=1}^nv_k^2/\lambda_k$
    - $\frac{1}{(2\pi)^{n/2}\vert\Sigma\vert^{1/2}}\int\exp\left\{-\frac{1}{2}\mathbf{w}^T\Sigma^{-1}\mathbf{w}\right\}\mathbf{w}\mathbf{w}^Td\mathbf{w}\\  =\sum_{i=1}^n\sum_{j=1}^n\mathbf{q}_i\mathbf{q}_j^T\frac{1}{(2\pi)^{n/2}(\lambda_1\cdots\lambda_n)^{1/2}}\int\exp\left\{-\sum_{k=1}^n\frac{v_k^2}{2\lambda_k}\right\}v_iv_jd\mathbf{v}\\  =\sum_{i=1}^n\mathbf{q}_i\mathbf{q}_i^T\left(\prod_{k=1\atop k\neq i}^n\frac{1}{(2\pi\lambda_k)^{1/2}}\int\exp\left\{-\frac{v_k^2}{2\lambda_k}\right\}dv_k\cdot\frac{1}{(2\pi\lambda_i)^{1/2}}\int\exp\left\{-\frac{v_i^2}{2\lambda_i}\right\}v_i^2dv_i\right)\\  =\sum_{i=1}^n\mathbf{q}_i\mathbf{q}_i^T\lambda_i=\Sigma$
      - 當 $i\neq j$，根據對稱性可知積分為零，並使用單變量變異數 $\hbox{E}\left[v_i^2\right]=\lambda_i$
    - $\hbox{E}\left[\mathbf{x}\mathbf{x}^T\right]=\boldsymbol{\mu}\boldsymbol{\mu}^T+\Sigma$
  - $\hbox{cov}[\mathbf{x}]=  \hbox{E}\left[\mathbf{x}\mathbf{x}^T-\mathbf{x}\boldsymbol{\mu}^T-\boldsymbol{\mu}\mathbf{x}^T+\boldsymbol{\mu}\boldsymbol{\mu}^T\right]\\  =\hbox{E}\left[\mathbf{x}\mathbf{x}^T\right]-\text{E}[\mathbf{x}]\boldsymbol{\mu}^T-\boldsymbol{\mu}\text{E}\left[\mathbf{x}\right]^T+\boldsymbol{\mu}\boldsymbol{\mu}^T\\  =\hbox{E}\left[\mathbf{x}\mathbf{x}^T\right]-\boldsymbol{\mu}\boldsymbol{\mu}^T=\Sigma$



