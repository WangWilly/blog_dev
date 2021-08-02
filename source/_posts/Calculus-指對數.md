title: Calculus - 指對數
author: Willy Wang
tags:

  - Exponent
  - ''
  - Logarithm
categories:
  - Calculus
date: 2018-03-15 16:40:00
---
# $e^x$



![exp](\willywangkaa\images\exp.png)



## 緣起



- 複利公式
  - 一年後的本利和 = $(1+\frac{年利率}{期數})^{期數}$
    - **期數：一年期間內複利次數**
      - 若為「逐月複利」則期數為 12
  - 若有 1 份借貸以「x 年利率」並**「逐月複利」**
    - 則**每個月為前一月**總值乘以 $1 + \frac{x}{12}$，**一年**的總增值為 $(1 + \frac{x}{12})^{12}$
  - 若有 1 份借貸以「x 年利率」並**「逐日複利」**
    - 則**每天為前一天**總值乘以 $1 + \frac{x}{365}$，**一年**的總增值為 $\left (1+ \frac{x}{365} \right)^{365}$
- 歐拉數 $e$ 進而被提出
  - 若一開始存 1 元、年利率是100%、**「逐秒複利」**
    - 則一年後的利息約為 2.71828 元（1 年 = 31,556,926 秒）
  - 若一開始存 1 元、年利率是100%、**複利期數期數無限大**
    - 則一年後的利息為 $e = \lim_{n \to \infty}\left ( 1 + \frac{1}{n} \right )^{n}$ 元
- **當複利期數為無限大時，為歐拉提出「指數函數」的定義**，由上式可以知道
  - $\lim_{n \to \infty}\left ( 1 + \frac xn \right )^n = \lim_{n \to \infty} \left ( \left ( 1 + \frac{1}{\frac nx} \right )^{\frac nx} \right)^x \approx e^x$
    - 對 n 與 x 進行探討，當 n **趨近於無限大**時 x 相對來說為一定值，所以 $\frac nx$ 亦**趨近無限大**
      - $\lim_{n \to \infty} \left ( 1 + \frac{1}{\frac nx} \right )^{\frac nx} \approx e^1 = e$



> **指數函數基本恆等式**
>
> - 下方兩式等價
>   - $e^{x+y} = e^{x} \cdot e^{y}$
>   - $\exp\left ( x + y \right ) = \exp\left ( x \right ) \cdot \exp\left ( y \right )$ 



## 性質

- 歐拉數 $e$ 的性質，對 $\forall x, y\in \mathrm{R}$
  - $e^{0}=1$
  - $e^{1}=e$
  - $e^{x+y}=e^{x}e^{y}$
  - $e^{x \cdot y}=\left(e^{x}\right)^{y}$
  - $e^{-x}={1 \over e^{x}}$



## 微分



1. 假設 $y = e^x$ 可微
   - 根據其定義**「應變數的微分」**$\mathrm dy = f'(x) \cdot \mathrm dx$
     - 其中 $f(x) = e^x$，而 $f'(x)$ 為**「導函數」**
     - $\mathrm dx$ 為**「自變數的微分」**
2. 探討 $\Delta x$ 與 $\Delta y$ 的「比例」以求其**「導函數」**
   - 根據定義 $\Delta y = f(x+\Delta x) - f(x)$
     - $\Rightarrow \Delta y = e^{x+\Delta x} - e^{x}\\ =  e^x \cdot e^{\Delta x} - e^x \\ = (e^{\Delta x} - 1) e^{x} \quad .......（1） $ 
   - 當 $\Delta x$ 很小時，對 $e^{\Delta x}$ 進行探討
     - $e^{\Delta x} \Rightarrow \lim_{n \to \infty} e^{\frac 1n} \approx \lim_{n \to \infty}\left(\left(1 + \frac1n\right)^n\right)^{\frac 1n} \\ = \lim_{n \to \infty} \left(1 + \frac1n\right) = 1 + \Delta x \quad ......（2）$
3. 將式（2）帶入式（1）可得知其**「導函數」**
   - $\Delta y = (e^{\Delta x} - 1) e^{x} \approx (1+\Delta x -1)e^x = e^x\Delta x$
     - 則 $f'(x) = \lim_{\Delta x \to \infty} \frac {\Delta y}{\Delta x} = e^x$
     - $\Rightarrow \frac {\mathrm dy}{\mathrm dx} = e^x \\ \Rightarrow dy = e^{x}\cdot dx$



**Example**

- 對 $y = e^{-x^2}$ 微分
  - $\mathrm dy = (e^{-x^2}) \cdot \mathrm d(-x^2)$
  - $\mathrm dy= (-2x e^{-x^2}) \cdot dx$ 



**Example**

- 對 $y = a^x $ 微分
  - $\Rightarrow y = e^{\ln{a^x}} = e^{x\cdot\ln a}$
  - 對 $y$ 作微分
    - $\Rightarrow \mathrm dy= e^{x\cdot\ln a} \cdot \mathrm d(x \ln a )$
    - $\Rightarrow \mathrm dy= (\ln a\cdot a^x) \cdot \mathrm dx$



> **求 $y = a^x$ 之「導函數」**
>
> - $\frac{\mathrm dy}{\mathrm dx} = \lim_{\Delta x \to 0} \frac {a^{x+\Delta x}-a^x}{\Delta x} = (\lim_{\Delta x \to 0} \frac {a^{\Delta x}-1}{\Delta x})a^x$
>
> 其「導函數」為「原函數」乘上**與自己成正比**之值
>
> - 對 $\lim_{\Delta x \to 0} \frac {a^{\Delta x}-1}{\Delta x}$ 進行探討，將 $a$ 以 $e$ 代入可得
>   - $\lim_{\Delta x \to 0} \frac {e^{\Delta x}-1}{\Delta x} \approx \lim_{\Delta \to 0} \frac {(1+ \Delta x)-1}{\Delta x} = \lim_{\Delta \to 0} \frac {\Delta x}{\Delta x} = 1$
>     - 推估此式為反函數：$\log_e x = \lim_{n \to 0} \frac {x^n-1}{n} \equiv \lim_{n\to\infty} n(x^{\frac 1n}-1)$
>     - 亦為**「歐拉對自然對數的定義」**



# $\ln x$

 

![log](\willywangkaa\images\log.png)


$$
y = \log_{e}{x} = \ln{x} \quad, x = e^{y}
$$

$$
\ln (a) = \int_1^a \frac{1}{x} dx
$$



![log2](\willywangkaa\images\log2.png)



**Proof**：$\ln x = \int_1^x \frac{1}{x} dx$

1. $\frac{\mathrm d}{\mathrm dx}(\ln x) = \lim_{\Delta x \to 0} (\frac{\ln(x+\Delta x) - \ln x}{\Delta x})$
   - $\Leftrightarrow \lim_{\Delta x \to 0} (\frac 1{\Delta x}\cdot \ln(\frac{x+\Delta x}{x})) \\ \Leftrightarrow \lim_{\Delta x \to 0} (\ln (1+\frac{\Delta x}{x})^{\frac{1}{\Delta x}})$
2. 令 $u = \frac {\Delta x}{x}、\Delta x = ux$
   - 欲使 $\Delta x \to 0$ 時
     - 則必使 $u \to 0$
   - $\Rightarrow \lim_{u \to 0} (\ln (1+u)^{\frac{1}{u x}} ) = \lim_{u \to 0} (\ln(((1+u)^{\frac{1}{u}})^\frac{1}{x}))\\ \Leftrightarrow \lim_{u \to 0} (\frac 1x \cdot ln (1+u)^{\frac 1u}) \\ \Leftrightarrow \frac 1x \lim_{u \to 0} (\ln(1+u)^{\frac 1u})$
3. 令 $n = \frac 1u$
   - 欲使 $u \to 0$
     - 則必使 $n \to \infty$
   - $\Rightarrow \frac 1x lim_{n \to \infty} (\ln (1 + \frac 1n)^n) \\ \Leftrightarrow \frac 1x \ln( \lim_{n \to \infty} (1 + \frac 1n)^n ) \\ \Leftrightarrow \frac 1x \ln e \Leftrightarrow \frac 1x$
     - 則 $\frac{\mathrm d}{\mathrm dx}(\ln x) = \frac 1x = \frac{\mathrm d}{\mathrm dx} \int_1^x \frac 1t \mathrm dt$
     - 根據「微積分第一基本定理」，表明不定積分是微分的逆運算，其保證某連續函數之原函數的存在性
       - $\frac{\mathrm d}{\mathrm dx} (\ln x) = \frac{\mathrm d}{\mathrm dx} (\int_1^x \frac{1}{t} \mathrm dt) \\ \Leftrightarrow \ln x = \int_1^x \frac{1}{t} \mathrm dt$

> $\ln x$ **的微分**
>
> - $y = \ln x $
>   - $\Rightarrow x = e^y $
>   - $\mathrm dx = e^y \mathrm dy $
>     - （對 $e^{y}$ 作移項）
>     - $\Rightarrow \mathrm dy = \frac{1}{e^y} \mathrm dx = \frac{1}{x} \mathrm dx $



**Example**

- 對 $y=\log_a x $ 作微分
  - $ y = \frac{\ln x}{\ln a}$
    - $\Rightarrow dy= (\frac{1}{\ln a} \cdot  \frac 1x) \cdot dx$




# 補充例題



**Example**

- 對 $y = f \left( x\right) = x^x$ 作微分
  - **兩側取自然對數函數**
    - $\ln ( f ( x ) ) = x \cdot \ln x$
  - **對兩側取微分**
    - $\frac{f'(x)}{f(x)} = \left( 1 \cdot \ln{x} \right) + \left( x \cdot \frac{1}{x}\right)$
  - **對兩側乘上** $f(x)$
    - $f'(x) = \left( \left( 1 \cdot \ln{x} \right) + \left( x \cdot \frac{1}{x}\right) \right) \cdot f(x) \\ \Rightarrow f'(x) = \left( \ln{x}  + 1 \right) \cdot  \left( x^x \right)$ 



# 參考



[成大微積分指對數函數的微分（第四週共筆）](http://moodle.ncku.edu.tw/mod/wiki/view.php?pageid=363)

[維基百科 - e （數學常數）](https://zh.wikipedia.org/wiki/E_%28%E6%95%B0%E5%AD%A6%E5%B8%B8%E6%95%B0%29)

[維基百科 - 指數函數](https://zh.wikipedia.org/wiki/%E6%8C%87%E6%95%B0%E5%87%BD%E6%95%B0)

[中華科大 - PART 10：指數與對數微分公式彙整](http://aca.cust.edu.tw/online/custcalculusi/10/07_03_10.html)