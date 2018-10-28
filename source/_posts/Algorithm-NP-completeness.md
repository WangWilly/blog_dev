title: Algorithm - NP-completeness
author: Willy Wang (willywangkaa)
tags:
  - NP-Complete
  - Reduce
categories:
  - Algorithm
date: 2018-10-15 18:41:00
---
# 基本概念



- 目的
  - 將問題依照**難度**做分類
  - **如何定義 A 問題比 B 問題難？**
  - **如何分類？**
  - **分幾類？**



## Decision problem



答案只有 **Yse/No** 的問題



- Ex
  - 一個圖中**有無**「Hamiltonian cycle」？



每一個**最佳化問題 ( Optimization problem )** 均可用**參數化**的方式化成一**對應的「Decision problem」**



- Ex
  - KP-optimal problem：給定一背包負重 W 以及 n 個物件，問**最大的獲利是多少**？
  - KP-decision problem：給定一背包負重 W 以及 n 個物件與一個**整數 k**，問**獲利是否可大於 k**？



> 當輸出只有 0 或 1 時，稱之為「Language」

- 為何要化成「Decision problem」？
  - 將原本問題的輸出 ( Output ) **一致化**，轉換成只有「Yes (1)/No (0)」的輸出方便比較**原問題是否相同**
  - 利用每個**「Decision problem」**互相比較難度




![decisionproblem](\willywangkaa\images\decisionproblem.png)



## Reduce



> 當 A 可以「Polynominal time reduce」至 B，記做，$A \leq_P B$

- 令 A、B 為兩個問題，當 A 可以「Polynominal time reduce」至 B，則滿足：
  1. 存在一個函數 f ：A 的**輸入(input)** → B 的**輸入(input)**；Transformation
  2. f 為**「Polynominal time computable」；多項式時間**
  3. 對於任何一個 A 的輸入 x
     $A(x) = Yes \Leftrightarrow B(f(x)) = Yes$




![reduce_1](\willywangkaa\images\reduce_1.png)



- 若 $A \leq_P B$ 代表可以用**解決 B 問題的演算法**，來實作**解決 A 問題的演算法**
- 若 $A \leq_P B$ 代表**A 問題的難度小於 B 問題的難度 ( B 至少比 A 難 )**
  - 若 B 問題可以解決同時代表 A 問題也可以被解決




![reduce_2](\willywangkaa\images\reduce_2.png)



# P、NP、NP-hard、NP-complete



> - 「Deterministic algorithm」
>   - 使用該演算法碰到子問題時，只會有一種解答，不會有多種解答。(有限狀態機)

- P 問題集合
  - 問題皆有一個「Deterministic algorithm」可以在「Polynominal time」中**解決( Solve )**。

Ex：「排序問題」$\in$ P；存在一個演算法 ( Quick sort ) 可以在 $\Theta(n\lg n)$ 中解決



> - **NP 非「Non-polynomial time」**
>
> - **P** $\subset$ **NP**

- **NP 問題集合**
  - 問題皆有一個「Non-deterministic algorithm」可以在「Polynominal time」中解決。
  - 給一個**可能的解**，若可以在「Polynominal time」中**判斷( Verify )**是否為該問題的解，則此問題屬於 NP 集合。




- NP-hard 問題集合
  - $\forall Q \in NP, Q \leq_P A \Leftrightarrow A \in NP–hard$



> 在 NP 集合中的問題均有演算法可以解決，所以**在 NPC 集合即為「有可用演算法解的最難問題」**

> NPC 集合的問題在**「Worst case」**下沒有**「Polynominal time」**的演算法可解決 (**至少都要「Exponential time」**)
>
> - 非「Any case」
> - **有「演算法」可以解**



- NP-complete 問題集合
  - $A \in NP–complete \Leftrightarrow A \in NP \;且\; A \in NP–hard$ 





## 集合關係



![nprelation](\willywangkaa\images\nprelation.png)



- Ex ( true or false )
  - $P \supset NP$；True
  - $P \supset NP$；Unknown
  - $P = NP$；Unknown
  - $P \neq NP$；Unknown



- 有一個在 NPC 集合的問題之「Worst case」若可被「Polynominal time」的演算法解決
  - $\Leftrightarrow P = NP$



# 證明問題是否為 NP-complete



令 A 為一問題，欲證明 $A \in NPC$

1. 證明 $A \in NP$
2. 任找一個 $B \in NPC$，證明 $B \leq_P A$
   - $\Leftrightarrow$ **證明** $A \in NP–hard$



- (94 交大) ( True or False )
  - 已知 Clique 問題屬於 NPC 集合，Vertex-cover 問題屬於 NP 集合，欲證明 Vertex-cover 屬於 NPC 集合，則將 Vertex-cover $\leq_P$ Clique 即可；False。



- (99 政大)
  - 欲證明 A 問題屬於 NPC 集合，則將一個 B 屬於 NPC 「Reduce」到 A 即可；False。

> - 不知道 A 是否屬於 NP 集合，否則 A 有可能為 NP-hard 集合 (無演算法可以解)。



## SAT problem



**第一個 NP-complete 問題**

給定一個「Conjection normal form」$F$ ，問**有無**一組「Assignment」可使 $F$ 輸出為 True。

> 令 $F = (x_1 \or \bar{x_2} \or \bar{x_3}) \and (\bar{x_1} \or x_3) \and (x_2 \or \bar{x_3})$ 
>
> 當給定 $x_1 = true、x_2 = true、x_3 = true$ ，$F$ 的輸出也為 True，
> 所以 $F$ 在 SAT problem 的輸入，則其解為 True。

> 該問題使用了一個「Non-deterministic algorithm」解決。



## 常見的證明流程


$$
「SAT」 \leq_P 「3-SAT」 \leq_P 「Clique」 \leq_P 「Vertex–cover」\\
\leq_P Hamilton \; cycle \leq_P \left\{\begin{matrix}
Hamilton \; path\leq_P Longest \; path \\ 
Traveling \; salesman \; prblem
\end{matrix}\right.
$$


### 證明 Longest path problem 屬於 NPC 集合



> 給定一個圖 G = (V, E) 與 **k**，問 G 中**有無**長度大於 **k** 的「Simple path」。



1. Claim：Longest path problem 屬於 NP
   1. (**給定可能的解**) 給定一組 Longest path 的輸入 (G, k) 與任意一條「Simple path」$P$
   2. (**在「Polynominal time」內可以確認其解**) 則可以在「Polynominal time」中判斷 P 是否為 G 中長度大於等於 k 的「Simple path」
   3. 所以「Longest path problem」屬於 NP 集合。

> 給定 $P = ＜u_1, u_2, \ldots, u_m＞$ 為一條「Path」
>
> 1. 確認 m 是否大於等於 k+1 ？
>    - $\Theta(1)$
> 2. 確認 $(u_i, u_{i+1}) \in E$ ？
>    - 若 G 使用「Adjacency matrix」儲存，若要判斷一邊是否屬於 E ；$\Theta(1)$

2. Claim：Hamilton path $\leq_P$ Longest path
   1. 給定一個 Hamilton path problem 的輸入：G = (V, E)，
      定義一個函數 f ：G → (G, k = |V|-1)，則 (G, |V|-1) 為 Longest path problem 的輸入
   2. f 顯然為「Polynominal time computable」
   3. G 有 Hamilton path $\Leftrightarrow$ G 有長度大於等於 |V|-1 的「Simple path」
      G 有 Hamilton path $P$ ，即：
      - $\Leftrightarrow P$ 必過 G 中每一點恰一次，且 $P$ 為「Simple path」
      - $\Leftrightarrow P$ 為 G 中長度大於等於 |V|- 1 的「Simple path」




![longestpathproblem_NPC](\willywangkaa\images\longestpathproblem_NPC.png)


### 證明 Traveling salesman problem 屬於 NPC 集合



> 給定一個無向、有權重的**完全圖** G = (V, E) 與 **k** ，問 G 中有無總權重**小於等於 k** 的「Hamilton cycle」？



1. Claim：Traveling salesman problem 屬於 NP 
   1. (**給定可能的解**) 給定一個無向、有權重的**完全圖** G = (V, E) 與 **k**，與一個 G 上的一個 Hamilton cycle $C$
   2. (**在「Polynominal time」內可以確認其解**) 則可以在「Polynominal time」中驗證 $C$ 的權重和是否**小於等於 k** ，所以 TSP 問題屬於 NP 集合
2. Claim：Hamilton cycle problem $\leq_P$ Traveling salesman problem
   1. **給定一個 Hamilton 的輸入：G = (V, E)，定義一個 f：G → (G', k = |V|)，**
      **其中 G' 的定義如下：G' = (V, E')，** $\left\{\begin{matrix} (u, v) \in E', weight(u, v) = 1, if \;(u, v)\in E \\ (u, v) \in E', weight(u, v) = 2, if \;(u, v)\notin E \end{matrix}\right.$ 
   2. f 顯然為「Polynominal time computable」
   3. **G 有 Hamilton cycle** $\Leftrightarrow$ **G' 有一「Hamilton cycle」且其總權重 ≦ |V|**




![TSP_NPC](\willywangkaa\images\TSP_NPC.png)



> ($\Rightarrow$)
>
> G 若有一個「Hamilton cycle」$C$
>
> $\because (u, v) \in E', weight(u, v) = 1, if \;(u, v)\in E$ 
>
> $\Rightarrow$ G' 必也有一個相對應的「Hamilton cycle」$C'$ 其權重總和 ≦ |V|
>
> ($\Leftarrow$)
>
> G' 若有「Hamilton cycle」$C$ 且權重總和 ≦ |V|
>
> $\because (u, v) \in E', weight(u, v) = 2, if \;(u, v)\notin E$
>
> $\Rightarrow C$ 上的每一邊在 G' 的權重必為 1 
>
> $\because (u, v) \in E', weight(u, v) = 1, if \;(u, v)\in E$ 
>
> $\Rightarrow C$ 通過 G 每節點一次
>
> $\Rightarrow C$ 為一個在 G 中的「Hamilton cycle」