title: Principle of inclusion and exclusion - 排容原理
author: Willy Wang (willywangkaa)
tags:
  - Algorithm
  - 排容原理
categories:
  - Discrete Mathematics
date: 2018-09-11 10:51:00
---
# Principle of inclusion and exclusion - 排容原理



假設 $U$ 為與集合，其中 $|U| = N$，令 $a_1, a_2, ..., a_n$ 為 n 個性質，其中這些性質直接定義在 $U$ 上且 $N(a_i)$ 表示 $U$ 中滿足性質 $a_i$ 的元素個數。

- 定義
  - $N(\bar{a_i})$ 表示 $U$ 中不滿族性質 $a_i$ 的元素個數。
  - $N(a_ia_j)$ 表示 $U$ 中同時滿足性質 $a_i$ 與 $a_j$ 的元素個數。
  - $N(\bar{a_i}\bar{a_j})$ 表示 $U$ 中同時滿足性質 $a_i$ 與 $a_j$ 的元素個數。



## Lemma 1



若有一數 $n$ 有正因數解，該組解必有一正因數「小於等於」$\sqrt{n}$。



### Proof



若兩正因數都大於 $\sqrt{n}$ ，相乘後不可能為 $n$



## Ex ( 97 清大 )



1 ~ 100 中質數有幾個？



### Sol


$$
\because \sqrt{100} = 10 \therefore Consider: 2, 3, 5, 7\\
\begin{matrix}
令 & \; U \; 表示 2 到 100 的所有數所成集合，則 \; N = |U| = 99 \\
 & a_1 表示 U 中大於 2 且為 2 的倍數的性質 \\
 & a_2 表示 U 中大於 3 且為 3 的倍數的性質 \\
 & a_3 表示 U 中大於 5 且為 5 的倍數的性質 \\
 & a_4 表示 U 中大於 7 且為 7 的倍數的性質
\end{matrix} \\
欲求: N(\bar{a_1}\bar{a_2}\bar{a_3}\bar{a_4})\\
\Rightarrow 100 - (\lfloor\frac{100}{2}\rfloor + \lfloor\frac{100}{3}\rfloor+ \lfloor\frac{100}{5}\rfloor+ \lfloor\frac{100}{7}\rfloor) + \\
(\lfloor\frac{100}{6}\rfloor + \lfloor\frac{100}{15}\rfloor+ \lfloor\frac{100}{35}\rfloor+ \lfloor\frac{100}{10}\rfloor + \lfloor\frac{100}{14}\rfloor + \lfloor\frac{100}{21}\rfloor) - \\
(\lfloor\frac{100}{30}\rfloor + \lfloor\frac{100}{70}\rfloor+\lfloor\frac{100}{105}\rfloor + \lfloor\frac{100}{42}\rfloor) + \lfloor\frac{100}{350}\rfloor\\
= 22 -1 + 4  = 25
$$


## Ex ( 97 台大 ) 尤拉公式說明



$n = P_1^{e_1}\times P_2^{e_2}\times P_3^{e_3}, P_i \;is \;prime , e_i > 0\quad Prove: \Phi(n) = n\times(1 - \frac1{P_1}) \times (1 - \frac1{P_2}) \times (1 - \frac1{P_3})$ 



### Proof


$$
U = ｛1, ..., n｝, a_1 = ｛1 < x < n \;\vert\; P_1|x ｝, a_2 = ｛1 < x < n \;\vert\; P_2|x ｝, a_3 = ｛1 < x < n \;\vert\; P_3|x ｝\\
求 \; N(\bar{a_1}\bar{a_2}\bar{a_3}) \\
\Rightarrow n - (\frac{n}{P_1}+\frac{n}{P_2}+\frac{n}{P_3}) + (\frac{n}{P_1P_2} + \frac{n}{P_2P_3} + \frac{n}{P_1P_3}) - \frac{n}{P_1P_2P_3} \\
 = n \times (1-(\frac{1}{P_1}+\frac{1}{P_2}+\frac{1}{P_3}) + (\frac{1}{P_1P_2} + \frac{1}{P_2P_3} + \frac{1}{P_1P_3}) - \frac{1}{P_1P_2P_3}) \\
  = n\times(1 - \frac1{P_1}) \times (1 - \frac1{P_2}) \times (1 - \frac1{P_3})
$$


## Theorem



> m 個**相異物**放置 n 個**相異箱子「不允許」**空箱的方法數。



$|A| = m, |B| = n, m\geq n \Rightarrow onto(m, n) = \sum_{i = 0}^n(-1)^i\binom{n}{i}(n-i)^m$  ( 由 A 集合至 B 集合的**映成函數**個數 )



> 箱子不得為「空」，採用**排容原理**。



### Proof


$$
onto(m, n) = n^m - \binom{n}{1}(n-1)^m + \binom{n}{2}(n-2)^m - ... + (-1)^{n-1}\binom{n}{n-1}(1)^m + (-1)^{n}\binom{n}{n}(0)^m\\
 = \sum_{i = 0}^n(-1)^i\binom{n}{i}(n-i)^m
$$


## Stirling number of the second kind



> ＜Note＞ 分堆
>
> m 個**相異物**放置於 n 個**相同箱子「不允許空箱」**的方法數，也就是將「映成函數個數」扣除重複計算到相同箱子的個數



- 假設 $m, n$ 為兩個整數，其中 $m \geq n \geq 1$，定義

$$
S(m, n) = \frac{onto(m, n)}{n!} = \frac{\sum_{i = 0}^n(-1)^i\binom{n}{i}(n-i)^m}{n!}
$$

稱為第二種 Stirling 數，有時記作 $\begin{Bmatrix} m \\  n \end{Bmatrix}$ ，另外為了方便起見，當 $m < n$ 時，定義 $S(m, n) = 0$



> ＜Note＞ 
>
> m 個**相異物**放置於 n 個**相同箱子「允許空箱」**的方法數
> $$
> = S(m, n) + S(m, n-1)+S(m, n-2)+\ldots+S(m, 2)+S(m, 1)
> $$
>



## Theorem - 第二種 Stirling 數的遞迴結構



> 可用在演算法的使用上，通常以「Dynamic programming」的方式撰寫。

$$
S(m+1, n) = S(m, n-1)+S(m, n)
$$



> - Dynamic programming ( 直向軸為 m，橫向軸為 n )
>
> |      |      1      |      2       |      3       |      4       |      5       |      6      |  7   |
> | ---- | :---------: | :----------: | :----------: | :----------: | :----------: | :---------: | :--: |
> | 1    | S(1, 1) = 1 |      -       |      -       |      -       |      -       |      -      |  -   |
> | 2    | S(2, 1) = 1 | S(2, 2) = 1  |      -       |      -       |      -       |      -      |      |
> | 3    | S(3, 1) = 1 | S(3, 2) = 3  | S(3, 3) = 1  |      -       |      -       |      -      |  -   |
> | 4    | S(4, 1) = 1 | S(4, 2) = 7  | S(4, 3) = 6  | S(4, 4) = 1  |      -       |      -      |  -   |
> | 5    | S(5, 1) = 1 | S(5, 2) = 15 | S(5, 3) = 25 | S(5, 4) = 10 | S(5, 5) = 1  |      -      |  -   |
> | 6    | S(6, 1) = 1 | S(6, 2) = 31 | S(6, 3) = 90 | S(6, 4) = 65 | S(6, 5) = 15 | S(6, 6) = 1 |  -   |
>
> - 使用方法
>   - 欲求 $onto(6, 3) + onto(6, 4)$
>   - 查表加上轉換後：$90\times 3! +65\times 4!$



### Proof ( 組合證法 )



- S(m+1, n)：m+1 個相異物**分成 n 堆**，固定一物 A
  - A 為邊緣人$\Rightarrow S(m, n-1)$
  - A 有孤單病，所以在 n 堆中挑一堆進入 $\Rightarrow S(m, n)\times n$

