title: Algorithm - Approximation algorithm
author: Willy Wang (willywangkaa)
tags:
  - Approximation algorithm
categories:
  - Algorithm
date: 2018-10-15 18:45:00
---
# Approximation ratio



設 A 為一個「Approximation algorithm」， OPT 為一個「Optimal algorithm」，

對於任何輸入 x ，|A(x)| ≦ |$\alpha$ OPT(x)|，則稱 A 的「Approximation ratio」為 $\alpha$。

(*假設處理「最小化問題」*)

> A(x)：以 x 為輸入產生的輸出大小。
>
> 所以「|A(x)| ≦ |$\alpha$ OPT(x)|」的意義是說**「Approximation algorithm」的解不會大於最佳解的 **$\alpha$ **倍。**



## Approximation algorithm for mimimum vertex cover question



![vertexcoverproblem](\willywangkaa\images\vertexcoverproblem.png)



```
// Vertex cover algorithm
// input G

c  <- {} // empty set
E' <- G.E

while(E' != {}) {
	(u, v) <- E' // 任意 E'
	c <- c \union {u, v}
	將 E' 所有含有 u 與 v 的邊去除
}

return c
```




![vertexcoverapproxi](\willywangkaa\images\vertexcoverapproxi.png)



- Time complexity
  - 因為在 While 中，每次至少會去除一條邊，且邊總共有|E|條
    - $\Theta(|E|)$



- **Apprimation ratio**
  - 因為「Vertex cover」要涵蓋圖中所有的邊，所以演算法中任挑一邊時，該連接兩點的其中一點**必在「Minimum vertex cover」集合之中**。
  - 在此近似演算法中，每次都挑一邊並將兩頂點加入 c 集合之中，**在最差的狀況下，都恰一個點在「Minimum vertex cover」中**
  - 所以 **|c| ≦ 2 × |Minimum vertex cover|**
    - 「Approximation ratio」為 2



## Approximation algorithm for euclidean traveling salesman problem

> **歐式空間**上的「Traveling salesman problem」。

給定平面上 n 個點，求一個**環路 ( Cycle )** 經過每個點恰一次且**「Euclidean disteance」和最小**



![EDTSP](\willywangkaa\images\EDTSP.png)



- Algorithm
  1. 選一個點 v 當作根節點 ( Root )
  2. 以 v 為起點，用「**Prim's algorithm**」算出 n 個點的「Minimum spainning tree」
     - 將歐式空間上的每一點兩兩相連成為一個「Completed graph」
  3. 令 L 為「Minimum spainning tree」做「Preorder traversal」的序列 (sequence)
  4. 依照 L 的順序將點相連，則可以得到欲求取的環路 $C$



- **Time complexity**
  - Step1：$\Theta(1)$
  - Step2：將平面上點兩兩相連 $\Theta(|V|^2)$，使用 Prim's algorithm $\Theta(|V|^2)$
  - Step3：「Preorder traversal」$\Theta(V)$
  - Step4：*已在第三步算完*
  - $\Rightarrow \Theta(|V|^2)$




![EDTSPapproxi](\willywangkaa\images\EDTSPapproxi.png)



- **Approximation ratio ** ( 100 中央資工 )
  - 假設最佳環路為 $C^＊$，令 T 為第二步所求出的最小生成樹
  - 若將 $C^＊$ 去除任一邊**會形成一個生成樹 T'**
  - **因為 T 為最小生成樹**，所以 distance(T) ≦ distance(T') ≦ **cost(** $C^＊$ **)**......（1）
  - 假設 w 為「Preorder traversal」的「full walk」( 完整走過整顆最小生成樹 )，又最小生成樹的邊被「Full walk」走過兩次，**所以 cost(w) = 2 × cost(T)**......（2）
    首先，將 w 序列**以保留頭尾**、由前至後將「追蹤該點前已經有追蹤過」 的點去除，並且兩兩相連成為一個環路 $C$ ，因為 **三角不等式（dist(a, b) ≦ dist(a, c) + dist(c, b)）所以 cost( C ) ≦ cost( W )**......（3）
    因為（2）、（3）$\Rightarrow$ cost( $C$ ) ≦ 2 × cost( T )......（4）
    因為（1）、（4）$\Rightarrow$ cost( $C$ ) ≦ 2 × cost( $C^＊$ ) $\Rightarrow$ **「Approximation ratio」：2**




![triangleinequality](\willywangkaa\images\triangleinequality.png)




![EDTSP_optimalsol](\willywangkaa\images\EDTSP_optimalsol.png)