title: KNN and Radial Basis Function - K近鄰算法 與 放射狀基底函數網路
author: Willy Wang
tags:
  - KNN
  - Radial Basis Function
  - ''
categories:
  - Machine Learning
date: 2018-05-13 20:59:00
---
# 前言<br>機器學習分兩大類

---



## Eager alogorithm(積極)



### 性質



- 將歷史資料做了很多分析與篩選( Munge )等預處理。
- 因為訓練出的假說很明確( Explicit )，所以自然判斷也比起 Lazy 方法更迅速。



### $Ex.$



- Artifitial neural network
- Decision tree



## Lazy algorithm(被動)



### 性質



- 簡單的處理訓練資料，不會將資料轉成其他方式表達，所以資料不會因為處理過後而喪失一些屬性。
- 這種訓練方法在「判斷」未知案例時，因為要做的比對處理比較多，比起利用歷史資料先處理的 Eager算法需時更長。
- 因為這種方法是將資料做區域化的處理( Localized )，所以每次判斷全域( Generalization )時都要重新花一次時間。



### $Ex.$



- KNN




# KNN and Radial Basis Function

---



## Instance-based Learning



以先前的案例( 未做處理或修飾的 )建立假說空間。



## 重點



- 概念
  - 只有將先前的案例完整的儲存起來，不要有任何的修飾或是處理。
  - $$<x_i, f(x_i)>$$
- 最近鄰法 ( Nearest neighbor )
  - 給定一個**要判斷**的案例 $x_q$，接者我們在該案例空間中找到**一個**最靠近的先前案例以猜測之。
  - $\hat{f}(x_q) \leftarrow f(x_{Nearest \; neighbor})$
- 近 $K$ 鄰法 ( *KNN* )
  - 給定一個**要判斷**的案例 $x_q$，接著我們在近 $K$ 個鄰居之中做**多數決選擇法**。 ( 當目標結果 Target function 是離散值時 )
  - 給定一個**要判斷**的案例 $x_q$，接著我們在近 $K$ 個鄰居之中取其**平均數**決定。 ( 當目標結果是在實數值時 )
  - $i.e., \quad \hat{f}(x_q) = \frac{\sum_{i = 1}^k f(x_i)}{k}$



## Voronoi Diagram - [沃羅諾伊圖](https://zh.wikipedia.org/wiki/%E6%B2%83%E7%BD%97%E8%AF%BA%E4%BC%8A%E5%9B%BE)



- K-Nearest Neighbor

![knearestneighbor](\blog\images\knearestneighbor.png)

- Voronoi Diagram (K= 1)

![voronoidiagram](\blog\images\voronoidiagram.png)



## Nearest neighbor - 最近鄰法



- 重點性質
  - 將所有的案例( Instance )儲存在一個 $R^n$ 案例空間之中。( $n$ 的多寡代表這些資料的 **Attribute - 環境引響因素**有多少 )
  - 盡量將 *環境引響因素* 去蕪存菁，最好少於 *20 個* 。 <br>**＜Note＞：** 引響的環境因素最好是**具有意義實數**且不可以太多。<br>舉例：假設今天有一筆資料同時可以被兩個與三個的**環境因素**所表達，但在有三個**環境因素**案例空間的資料間隔會更分散。

- Pros
  - 訓練的時間很快。
  - 可以學習很複雜的「目標**函數 ( 概念 )**」
  - 不會捨去訓練資料的資訊。
- Cons
  - 判斷新進的案例需時很長。
  - 容易被不重要的**環境因素**所誤導。



## K-Nearest Neighbor Learning - 近 $K$ 個鄰法



若今天給定資料案例 $x$ 是由 $n$ 個 **attributes - 環境因素**所構成，可以表達為：$x = ＜a_1(x), \ldots, a_n(x)＞$。

接者，使用平常對於歐基里德空間最熟悉的兩點求距**( Euclidean distance )** $d(i, j) = \sqrt{\sum_{r = 1}^n (a_r(x_i) - a_r(x_j))^2}$



###Algorithm - 演算法



- 給定一個要判斷的案例 $x_q$ 。
  - 欲找到 $k$ 個與 $x_q$ 靠最近的 $x_i$ 。( 利用 $d(x_i, x_q)$ 判斷距離 ) 
  - 選出 $k$ 個案例之中出現比較多次的結果作為 $x_q$ 的結果。( 當這份資料的結果是由離散的資料組成採用此方法 )
    - 以下圖舉例 <br> 
![knearestneighbor2](\blog\images\knearestneighbor2.png)<br>$k = 1$ ，判定 $x_q$ 為正向輸出。<br>$k = 5$ ，判定 $x_q$ 為負向輸出。
  - 當資料的結果為連續的時數值時，我們判定 $x_q$ 的輸出為 $k$ 個鄰居的平均值。



### 建立的假說空間



- 使用 KNN 時我們建立的假說空間 $H$ 不是明確的。



#### 隱式的假說空間 $H$



- 將所有的訓練資料( 案例 )都完整地保留在我們建立的假說之中。
- 要檢驗新的 $x_q$ 時，需要將所有的案例都檢查過一遍。
- 1-NN：$H$ = Voronoi Diagram

![voronoidiagram2](\blog\images\voronoidiagram2.png)



## 距離權重近鄰法 Distance - Weighted $K$NN



- 欲考慮比較近的鄰居佔比越重。( 所以距離與權重呈現倒數的關係 )
  - $\hat{f} \leftarrow \frac{\sum_{i = 1}^k w_if(x_i)}{\sum_{i = 1}^k w_i}$
  - $w_i \equiv \frac{1}{d(x_q, x_i)^2}$
  - $d(x_q, x_i)$ 是 $x_q$ 與 $x_i$ 的距離。
- 問題來了，那我們要選擇幾個鄰居作為參考值呢？
  - Shepard's Method ：將整個案例空間所有的 $x_i$ 都納入考量。



## Curse of Dimensionality - 維度災難 ([閱讀更多](https://blog.csdn.net/ztf312/article/details/50894224))



想像一個案例可以用20個環境變因( Attribute )所解釋，但是只有其中兩個變因是實際有影響的，若變因的維度太高可能會讓不重要的因素導致整個空間裡的案例之間的距離變得更稀疏，進而干擾我們最終呈現的「想法」。



### 解決方法：計算兩個案例之間的距離時對每個變因進行加權



這樣的方法相當於按比例縮放歐式空間中的坐標軸，先決定哪些環境邊因對我們的訓練比較重要(Try and error)，縮短對應到相關不大之變因的坐標軸，拉長對應於相關較大之變因之座標軸。每個座標軸的伸縮量可以透過交叉驗證的方法自動決定。



## 一些專有名詞



- Regression - 回歸
  - 逼近一個實數函數 $f$ ( 最終「想法」)
- Residual - 殘差
  - 「某樣本的均值」與「所有樣本集均值」的均值之偏離，代表取樣的合理性即該樣本是否具代表意義。残差大，表明樣本不具代表性，也有可能由特徵值引起。 <br> **＜Note＞：**誤差: 所有「不同樣本集的均值」之均值與真實總體均值的偏離量。由於真實總體均值通常無法獲取或觀測，因此通常是假设總體為某一分部類型，則有 $N$ 個估算的均值；代表的是觀測/測量的精確度。誤差大，由變異數引起。表明數據可能有嚴重的測量錯誤，或者所選模型不合適。
  - 要看一個模型是否合適，看誤差；要看所取樣本是否合適，看残差。
  - $\hat{f}(x) - f(x)$
- Kernel function $K$ - 核心函數 $K$
  - 決定距離的函數，用於決定權重影響的比例。
  - $w_i = K(d(x_i, x_q))$
  - $\Rightarrow w_i =  K(d(x_i, x_q)) = \frac{1}{d(x_i, x_q)^2}$



# Locally Weighted Regrassion - 區域加權回歸

---



> **＜Note＞：**全域法 v.s 區域法：在估計 $f(x_q)$ 時，
>
> - 全域法
>   - 將所有的案例 $＜x, f(x)＞$ 納入參考。
> - 區域法
>   - 只將區域的( knn )鄰居納入參考。



- 區域法
- 加權：由對應的案例 $x_i$ 與 $x_q$ 的距離產生的權重。
- 回歸：逼近一個實數的目標函式。



## 直觀



- $K$ - NN 對於目標函式 $f(x)$ ，有一個 $x_q$ 的需求時，產生一個區域型的逼近結果。
- 區域加權回歸其實就是泛化的 $K$ - NN 。
- 藉由 $x_q$ 劃出來的範圍 $K$ 直觀( explicit )的逼近目標函式 $f(x)$。
  - 舉例來說，可以一個「線性方程」表達 $K$ 個鄰居的加權影響值。


$$
\hat{f}(x) = w_0 + w_1a_1(x) + \ldots +w_na_n(x)
$$
$a_i$ 代表 $x$ 對應環境變因的值。

- 特別的是，有可能可以用「非線性方程」來表達。
  - K 個鄰居以二次多項式( quadratic function )呈現權重值。
- 以「分段近似法( piecewise approximation )」求取 $f$。 ( 類似cubic spline的手法 )



## 以各種不同的「殘差函式」將殘差降到最低求取最終的 $f$ 



- 在「 $K$ 個鄰居」之間可以找到 Sum Square Error (SSE)
  - $E_1(x_q) \equiv \frac{1}{2} \sum_{x \in k \; nearest \; nbr \; of \; x_q} (f(x) - \hat{f}(x))^2$
- 在「所有鄰居」之間可以找到**距權** Distance-Weighted SSE
  - $E_2(x_q) \equiv \frac{1}{2} \sum_{x \in D} (f(x) - \hat{f}(x))^2 \times K(d(x_q, x))$
- 將上面兩者合體後
  - $E_3(x_q) \equiv \frac{1}{2} \sum_{x \in k \; nearest \; nbr \; of \; x_q} (f(x) - \hat{f}(x))^2 \times K(d(x_q, x))$



# Radial Basis Function Networks 放射狀基底函數網路

---



將所有的區域逼近組合成全域型地逼近目標函數 $f$，通常用於影像、訊號分析。



**＜Note＞：**這種網路模型是完全不相干於「Artificial Neural Network - 類神經網路」，相較起來比較與「距離加權回歸」相近，但是以積極的( Eager )的方式而不是以被動的( Lazy )的方式實現。



![radialbasisfunction](\blog\images\radialbasisfunction.png)


$$
\hat{f}(x)  = w_0 + \sum_{u  = 1}^k w_u \times K_u (d(x_u, x))
$$
$a_i$代表 $x$ 的第 $i$ 個環境變因，而 $K_u (d(x_u, x))$ 已經定義為當距離 $d(x_u, x)$ 變大時會隨之變小。

- 常用的 $K_n$ ：Gaussian kernel function

$$
K_u (d(x_u, x)) = e^{-\frac{1}{2 \times {\delta_u}^2} d^2(x_u, x)}
$$

**＜Note＞：**常態分布( 又稱為高斯分布 ) $y = p(x) = \frac{1}{\sqrt{2\pi \delta^2}}e^{-\frac{1}{2}(\frac{x-\mu}{\delta})^2}$



## * 訓練 RBF 網路



- 問題一：對於核心函式$K_u(d(x_u, x))$，如何選定一個 $x_u$ ？
  - **問題亦是在問如何選定套用一個樣板( prototypes )當作訓練模型。**
  - 確認何點為這個案例空間的「常態分布中心點」。
  - 或是以其他的分布套用於該案例空間。
- 問題二：如何訓練權重( 假設使用Gaussian $K_u$的狀態下 )？
  - 對於每個 $K_u$ 先設定為變異數( 或是平均數 )。
  - 將 $K_u$ 固定，訓練出一個線性式的輸出層。



# 參考



[Quora - What is the difference between eager learning and lazy learning?](https://www.quora.com/What-is-the-difference-between-eager-learning-and-lazy-learning#)



[沃羅諾伊圖](https://zh.wikipedia.org/wiki/%E6%B2%83%E7%BD%97%E8%AF%BA%E4%BC%8A%E5%9B%BE)



[K近邻算法(kNN) - 知乎专栏](https://zhuanlan.zhihu.com/p/22717928)



[机器学习：维度灾难问题- CSDN博客](https://blog.csdn.net/ztf312/article/details/50894224)



[残差residual VS 误差 error](https://blog.csdn.net/jmydream/article/details/8764869)



[機器學習技法學習筆記(7)：Radial Basis Function Network與Matrix Factorization](http://www.ycc.idv.tw/YCNote/post/36)



[放射狀基底函數網路- 維基百科，自由的百科全書 - Wikipedia](https://zh.wikipedia.org/zh-tw/%E5%BE%84%E5%90%91%E5%9F%BA%E5%87%BD%E6%95%B0%E7%BD%91%E7%BB%9C)