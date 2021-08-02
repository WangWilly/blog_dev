title: Discrete mathematics - 泰勒級數
author: Willy Wang
tags:

  - Taylor Series
  - ''
categories:
  - Discrete Mathematics
date: 2018-03-16 14:54:00
---
# 泰勒級數（Taylor series）

英國數學家布魯克·泰勒（Sir Brook Taylor）於1715年發表泰勒公式

以**無限項連加式（級數）**來表示一個函數，其中「每一項」皆由函數在「某一點之導數」求得



- 英國數學家布魯克·泰勒（Sir Brook Taylor）於1715年發表泰勒公式
  - 通過函數在自變量零點的導數求得的泰勒級數
    - 又叫做**麥克勞林級數**

拉格朗日在1797年之前，最先提出帶有餘項的現在形式的泰勒定理。實際應用中，泰勒級數需要截斷，只取有限項，可以用泰勒定理估算這種近似的誤差。一個函數的有限項的泰勒級數叫做**泰勒多項式**。一個函數的泰勒級數是其泰勒多項式的極限（如果存在極限）。即使泰勒級數在每點都收斂，函數與其泰勒級數也可能不相等。開區間（或複平面開片）上，與自身泰勒級數相等的函數稱為**解析函數**。

# 定義

在數學上，一個在實數或複數 $a$ 在 **鄰域**上的**無窮**可微實變函數或複變函數 $f(x)$ 的泰勒級數是如下的冪級數 (若與原函式相等時為**解析函數**)：

$$
f(x) \simeq \sum_{n=0}^{\infty }{\frac {f^{(n)}(a)}{n!}}(x-a)^{n}
$$

而 $f^{(n)}(a)$表示函數 $f$ 在點 $ a$ 處的 $ n$ 階**導數**。如果 $a=0$ ，那麼這個級數也可以被稱為麥克勞林級數。

而多項式函數 $f(x)$ 在 $x = a$ 時，$n$ 階的泰勒展開式 $P_{n}(x)$ 是： 

$$
P_{n}(x) = \sum_{i = 0}^{n} \frac{ f^{(i)}(a) }{ i! }\cdot \left(x-a \right)^{i}
$$


# 解析函數
[Read more-1 (wiki)](https://zh.wikipedia.org/wiki/%E6%B3%B0%E5%8B%92%E7%BA%A7%E6%95%B0#%E8%A7%A3%E6%9E%90%E5%87%BD%E6%95%B8)，[Read more-2 (wiki)](https://zh.wikipedia.org/wiki/%E8%A7%A3%E6%9E%90%E5%87%BD%E6%95%B0)

---

如果泰勒級數對於區間 $(a-r,a+r)$中的所有 $x$ 都收斂並且級數的和等於 $f(x)$ ，那麼我們就稱函數 $f(x)$ 為解析的（analytic）。若且唯若一個函數可以表示成為冪級數的形式時，它才是解析的。為了檢查級數是否**收斂於** $f(x)$，通常採用**泰勒定理**估計級數的餘項 (數值方法)。上面給出的**冪級數展開式**中的係數正好是**泰勒級數**中的**係數**。

**泰勒級數的重要性體現在以下三個方面：**

1. 冪級數的求**導和積分**可以逐項進行，因此求**和函數**相對比較容易。
2. 一個解析函數可被延伸為一個定義在**複平面**上的一個開片上的解析函數，並使得複分析這種手法可行。 
3. 泰勒級數可以用來***近似計算函數的值***。

**對定值 x 而言，函數的精準度會隨著多項式的次數 n 的增加而增加。**
**對一個固定次數的多項式而言， 確度隨著 x 離開 x=0 處而遞減。**

# [泰勒級數](https://zh.wikipedia.org/wiki/%E6%B3%B0%E5%8B%92%E7%BA%A7%E6%95%B0#%E6%B3%B0%E5%8B%92%E7%BA%A7%E6%95%B0%E5%88%97%E8%A1%A8)列表(常用)

**注意：**核函數 $ x$ 為 *複數* 時它們依然成立！

- 幾何級數([等比數列](https://zh.wikipedia.org/wiki/%E7%AD%89%E6%AF%94%E6%95%B0%E5%88%97))

$$
\frac {1}{1-x} = \sum_{n=0}^{\infty }x^{n}\quad \forall x:\left|x\right|<1
$$

- [二項式定理](https://zh.wikipedia.org/wiki/%E4%BA%8C%E9%A1%B9%E5%BC%8F%E5%AE%9A%E7%90%86) 

$$
(1+x)^{\alpha }=\sum_{n=0}^{\infty }C^\alpha_n \cdot x^{n}\quad \forall x:\left|x\right|<1,\forall \alpha \in \mathbb{C}
$$

- 指數函數

$$
e^{x}=\sum_{n=0}^{\infty }{\frac {x^{n}}{n!}}\quad \forall x
$$

 - $f(x) = e^x$ 在 $x = 0$ 的泰勒展開式。
 當$n = 1$時，$P_{1}(x) = 1+ \frac{\left( e^0\right)'}{1!}\cdot\left( x - 0 \right)^1$
 當$n = 2$時，$P_{2}(x) = 1+ \frac{\left( e^0\right)'}{1!}\cdot\left( x - 0 \right)^1 + \frac{\left( e^0\right)''}{2!}\cdot\left( x - 0 \right)^2$
 當$n = 3$時，$P_{3}(x) = 1+ \frac{\left( e^0\right)'}{1!}\cdot\left( x - 0 \right)^1 + \frac{\left( e^0\right)''}{2!}\cdot\left( x - 0 \right)^2 + \frac{\left( e^0\right)^{(3)}}{3!}\cdot\left( x - 0 \right)^3$<br>
 $...$

- 自然對數

$$
\ln(1+x)=\sum_{n=1}^{\infty }{\frac {(-1)^{n+1}}{n}}x^{n}\quad \forall x\in (-1,1]
$$

# 牛頓插值公式的淵源

[Read more-1(wiki)](https://zh.wikipedia.org/wiki/%E7%89%9B%E9%A1%BF%E5%A4%9A%E9%A1%B9%E5%BC%8F)，[Read more-2(wiki)](https://zh.wikipedia.org/wiki/%E6%B3%B0%E5%8B%92%E7%BA%A7%E6%95%B0#%E8%88%87%E7%89%9B%E9%A0%93%E6%8F%92%E5%80%BC%E5%85%AC%E5%BC%8F%E7%9A%84%E6%B7%B5%E6%BA%90)

**牛頓插值公式**也叫做**牛頓級數**，由「牛頓 *前向* 差分方程」的項組成，得名於伊薩克·牛頓爵士。一般稱其為連續「泰勒展開」的**離散對應**。

## 差分

差分，又名差分函數或差分運算，是數學中的一個概念。它將原函數 $f(x)$ 映射到 $f(x+a)-f(x+b)$ 。差分運算，相應於微分運算，是**微積分**中重要的一個概念。


### 定義

**前向差分**的定義為：

$$
\Delta_{h}^{1}[f](x) = f(x + h) - f(x)
$$

$$
\Delta_{h}^{n}[f](x) = \Delta_{h}^{n-1}[f](x + h) - \Delta_{h}^{n-1}[f](x)
$$

$, where $ $ h =$ $ "x"$ $一步的間距，若無下標h，那間距h = 1。$

#### 前向差分

函數的前向差分通常簡稱為**函數的差分**。對於函數 $f(x)$ ，如果在等距節點：

$$
x_{k}=x_{0}+kh,(k=0,1,...,n)
$$

$$
\Delta f(x_{k})=f(x_{k+1})-f(x_{k})
$$

則稱 $\Delta f(x)$，函數在每個小區間上的增量 $y_{k+1}-y_{k}$ 為 $f(x)$ 一階差分。

#### 後向差分

對於函數 $f(x_{k})$，如果：

$$
\nabla f(x_{k})=f(x_{k})-f(x_{k-1})
$$

則稱 $\nabla f(x_{k})$ 為 $f(x)$ 的**一階逆向差分**。