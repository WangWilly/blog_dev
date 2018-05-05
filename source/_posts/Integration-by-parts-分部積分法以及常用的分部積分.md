title: Integration by parts - 分部積分法以及常用的分部積分
author: Willy Wang
tags:
  - Integration
  - Partial Integration
  - Integration By Parts
  - logarithm
categories:
  - Calculus
  - ''
date: 2018-05-01 09:33:00
---


**分部積分法**是種[積分](https://zh.wikipedia.org/wiki/%E7%A9%8D%E5%88%86)的技巧常應用於微積分數學與數值分析之中。它是由[微分](https://zh.wikipedia.org/wiki/%E5%BE%AE%E5%88%86)的[乘法定則](https://zh.wikipedia.org/wiki/%E4%B9%98%E6%B3%95%E5%AE%9A%E5%88%99)和[微積分基本定理](https://zh.wikipedia.org/wiki/%E5%BE%AE%E7%A7%AF%E5%88%86%E5%9F%BA%E6%9C%AC%E5%AE%9A%E7%90%86)推導而來的。其基本思路是將不易求得結果的積分形式，轉化為等價的但易於求出結果的積分形式。

# 規則

---

當 $u = u(x)$ 、 $du = u'(x)dx$ 、 $v = v(x)$ 與 $dv = v'(x)dx$ ， 那分部積分就可以寫為：

$$\int_a^b u(x)v'(x) dx = [u(x)v(x)]_a^b - \int_a^b u'(x)v(x)dx$$

$$\Leftrightarrow u(b)v(b) - u(a)v(a) - \int_a^b u'(x)v(x)dx$$

或是以更常見的簡寫：

$$\int u \; dv = uv - \int v \; du$$



# 定理

---

假設 $u(x)$ 與 $v(x)$ 是兩個連續可導函數 ([continuously differentiable](https://en.wikipedia.org/wiki/Continuously_differentiable) [functions](https://en.wikipedia.org/wiki/Function_(mathematics))). 由乘法定理 ([product rule](https://en.wikipedia.org/wiki/Product_rule)) 可知(用來布尼茲表示法 [Leibniz's notation](https://en.wikipedia.org/wiki/Leibniz%27s_notation))：

$\frac{d}{dx} ( u(x) \cdot v(x) ) = \frac{d(u(x))}{dx}\cdot v(x) + u(x)\cdot \frac{d(v(x))}{dx}$

對兩側求不定積分：

$uv = \int (\frac{d(u(x))}{dx}\cdot v(x) + u(x)\cdot \frac{d(v(x))}{dx})dx$

$\Leftrightarrow \int d(u(x))\cdot v(x) + \int u(x)\cdot d(v(x))$

$\Rightarrow \int u \; dv = uv - \int v \; du$



# 常用的分部積分

---



## $\int \ln(x) dx = x\ln(x) - x + C$

$\int \ln(x)dx$

令 $u = \ln(x)$ 、 $dv = dx$ ， 則 $du = \frac{1}{x} dx$ 、 $v = x$

帶入：

$\int \ln(x) dx = \ln(x) \cdot x - \int x\cdot \frac{1}{x}dx$

$\Leftrightarrow \ln(x)\cdot x - \int(1)dx$

$\Leftrightarrow \ln(x)\cdot x - x + C$



## $\int \log (x) dx = x\cdot \log (x) - \frac{x}{\ln 10} + C$

令 $u = \log (x)$ 、 $dv = dx$ ，

則 $du= d(\log (x)) \Leftrightarrow d(\frac{\ln x}{\ln 10})$

**＜乘法定理＞：** 上微下不微 + 下微上不微

$\Leftrightarrow d(ln x)\cdot \frac{1}{\ln 10} + \ln x\cdot d(\ln 10)$

$\Leftrightarrow \frac{1}{x}\cdot \frac{1}{\ln 10} + \ln x \cdot 0$

$\Leftrightarrow \frac{1}{x\ln 10}$

接著 $v = x$ 。

$\int \log x dx = \log x \cdot x - \int x \cdot \frac{1}{x \ln 10} dx$

$\Leftrightarrow x\cdot \log x - \int \frac{1}{\ln 10} dx$

$\Leftrightarrow x\cdot \log x - x\cdot\frac{1}{\ln 10} + C$



# 參考

---

[Wiki - 分部積分法](https://zh.wikipedia.org/wiki/%E5%88%86%E9%83%A8%E7%A9%8D%E5%88%86%E6%B3%95)



[Math2.org Math Tables: *Integral ln(x)*](http://math2.org/math/integrals/more/ln.htm)



[Derivative of Log X](https://www.emathzone.com/tutorials/calculus/derivative-of-log-x.html)