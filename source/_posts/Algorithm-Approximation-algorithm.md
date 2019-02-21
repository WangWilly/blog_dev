title: Algorithm - Approximation algorithm
author: Willy Wang (willywangkaa)
tags:
  - Approximation algorithm
categories:
  - Algorithm
date: 2018-10-15 18:45:00
---
# Approximation ratio

A ：「Approximation algorithm」

OPT ：「Optimal algorithm」

- 對任何輸入 x
- ∣A(x)∣ ≦ α×∣OPT(x)∣
  - 稱 A 的「Approximation ratio」為 α



**（下方討論皆以「最小化問題」處理）**

> - A(x)：以 x 為輸入產生的輸出大小
>   - **∣A(x)∣ ≦ α×∣OPT(x)∣ 意旨**
>     - **「近似演算法解決方案之值」小於等於 「α × 最佳解決方案之值」**



# Approximation algorithm for minimum vertex cover problem



![vertexcoverproblem](\willywangkaa\images\vertexcoverproblem.png)



```cpp
// Vertex cover algorithm
// input G
set vertex_cover_approximation(Graph G) {
    c  = ∅;                            // 建立一空集合用來儲存「Vertex cover」
    E′ = G.E;
    while(E′ ≠ ∅) {
        (u, v) = get_element(E′);      // 從 E′ 集合中取任意一邊
        c      = c ∪ {u,v};
        E′     = differ(E′,u,v);       // 將 E′ 所有含有頂點 u、v 的邊去除
    }
    return c;
}
```




![vertexcoverapproxi](\willywangkaa\images\vertexcoverapproxi.png)



- **Time complexity**
  - 在 `while` 每回合**至少**去除一條邊
    - G 中總共有 ∣E∣ 條邊
      - $\Theta(∣E∣)$



- **Approximation ratio**
  - **「Vertex cover problem」**
    - 此問題欲找一頂點集合**涵蓋圖中所有的邊**
    - **任挑**圖中一邊
      - **連接該邊其中一點必存在「Minimum vertex cover」集合**
  - **近似演算法分析**
    - 每回合任挑一邊並將其兩頂點加入 c 集合
      - **最差情況下只有一頂點亦存在於「Minimum vertex cover」**
      - **|c| ≦ 2 × |Minimum vertex cover|**
    - **Approximation ratio（α）： 2**



# Approximation algorithm for euclidean traveling salesman problem

> 在**歐式空間**上探討「Traveling salesman problem」（TSP）
>
> - 給歐式平面與平面上 n 個頂點，求
>   - 一**環路（Cycle）**經過每個點恰一次
>   - **其「Euclidean distance」總和最小**





![EDTSP](\willywangkaa\images\EDTSP.png)



步驟

1. 選一頂點 v 作根節點（Root）
   - 將歐式空間上每點兩兩相連成「完全圖」（$G$）
2. 以 v 為起點作「**Prim's algorithm**」算 $G$ 的「最小生成樹」 $T$
3. 令 $L$ 為 $T$ 之「Preorder traversal」序列（Sequence）
   - 依 L 的順序將點相連，以求取其環路 $C$



- **Time complexity**
  - Step 1
    - 任選一頂點
      - $\Theta(1)$
    - 將平面上點兩兩相連
      - $\Theta(∣V∣^2)$
  - Step 2
    - 使用「Prim's algorithm」
      - $\Theta(∣V∣^2)$
  - Step 3
    - 「Preorder traversal」
      - $\Theta(∣V∣)$
    - 求環路
      - $\Theta(∣V∣)$
  - **時間複雜度總和**
    - $\Rightarrow \Theta(∣V∣^2)$




![EDTSPapproxi](\willywangkaa\images\EDTSPapproxi.png)



- **Approximation ratio **（100 中央資工）
  - **假設**
    - $C^＊$
      - TSP 之「最佳環路」
    - $T$ 
      - 第二步求出的「最小生成樹」
    - $W$（路徑序列）
      - 「Full walk」（以前序遍歷 $T$ 的每個頂點）
      - 完整走完 $T$ 每條樹邊兩次
  - **證明**
    - 若去除 $C^＊$ 任一邊
      - **則形成一生成樹 **$T'$
      - 因為 $T$ 為最小生成樹
        - 則 distance($T$) ≦ distance($T'$) ≦ **cost(**$C^＊$**)**...（1）
      - $W$ 將 $T$ 所有邊走過兩次
        - 則 cost($W$) = 2×cost($T$)...（2）
    - 以路經序列 $W$ 求**近似解**之環路序列 $C$
      - **保留頭尾**
      - 自前至後將「已追蹤過點」去除 
      - 因三角不等式（dist(a,b) ≦ dist(a,c) + dist(c,b)）
        - cost($C$) ≦ cost($W$)**...（3）**
    - 從（2）、（3）可知
      - cost($C$) ≦ 2×cost($T$)...（4）
    - 從（1）、（4）可知
      - cost($C$) ≦ 2×cost($C^＊$) 
    - **Approximation ratio（α）：2**




![triangleinequality](\willywangkaa\images\triangleinequality.png)




![EDTSP_optimalsol](\willywangkaa\images\EDTSP_optimalsol.png)