title: Data structure - Advance binary search tree
author: Willy Wang (willywangkaa)
tags:
  - Binary search tree
categories:
  - Data structure
date: 2018-10-28 20:28:00
---
# AVL tree



Height balance binary search tree



- 性質
  - $|H_L - H_R| \leq 1$，$H_L、H_R$ 為**樹根節點**左右子樹之高度
  - 左右子樹亦為「AVL tree」



> **Example** (True or False)
>
> - 若「Binary search tree」之樹根節點左右子樹皆為「AVL tree」則整棵樹為「AVL tree」(FALSE)
> - **在「AVL tree」，任何節點在左右子樹必為「AVL tree」(TRUE)**
> - **在「AVL tree」，任何兩個葉節點位於的高度 (Level) 差必 ≦ 1 (FALSE)**；如下圖 s、t 葉節點
>
> ![AVLex_1](\willywangkaa\images\AVLex_1.png)
>
> - 在「AVL tree」，左子樹所有節點之數必 ≦ 右子樹所有節點之數值 (TRUE)
> - 在「AVL tree」中使用「Inorder travesal」可以得到「小→大」的排序 (TRUE)



- 調整原則
  1. 中間鍵值往上拉，小的置左大的置右
     - 此三個節點是被標上 LL、LR、RL、RR 兩條邊所連結的三個節點
  2. 孤兒節點 (子樹) 依照「Binary serch tree」性質置入



- LR




![AVL_LR_1](\willywangkaa\images\AVL_LR_1.png)


![AVL_LR_2](\willywangkaa\images\AVL_LR_2.png)

- RR



![AVL_RR_1](\willywangkaa\images\AVL_RR_1.png)



![AVL_RR_2](\willywangkaa\images\AVL_RR_2.png)



> 從「AVL tree」中任取兩個葉節點，該高度差取絕對值~~小於等於一~~。( 任何大於等於 0 的整數都有可能 )
>
> 從「AVL tree」中任取**其根節點位於同層**的兩子樹，其子樹的高度差取絕對值~~小於等於一~~。( 任何大於等於 0 的整數都有可能 )
>
>
>
> ![AVLex_2](\willywangkaa\images\AVLex_2.png)

形成高度為 h 的 AVL tree 所需之**最多節點數** = 「Full binary serach tree」數量 = $2^h -1$

形成高度為 h 的 AVL tree 所需之**最少節點數** = $F_{h+2} - 1$ ( F 為費氏數列 )

- 證明

高度為 0 時為一空樹最少 0 個節點即可，$F_{0+2}-1 = 1-1 = 0$ ，成立。

假設高度小於等於 h-1 時成立，考慮高度等於 h 時，
**最少節點數必定發生在「樹根」之左右子樹高度相差一時，**
不失一般性，令左子樹高度為 h-1；右子樹高度為 h-2，
所以左子樹最少節點個數為 $F_{(h-1)+2}-1 = F_{h+1}-1$；
右子樹最少節點個數為 $F_{(h-2)+2}-1 = F_{h}-1$。

**整棵樹需要最少的節點數為** $F_{h+1} -1 +F_{h} -1 + 1 = F_{h+2} - 1$ ，QED



Example

- 形成高度為 5 的「AVL tree」最少需要的節點數？

$F_{5+2}-1 = F_7 -1 = 12$



**Example**

- **300 個節點的 AVL tree 之最大高度為**？

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   | 13   | 14   |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 1    | 1    | 2    | 3    | 5    | 8    | 13   | 21   | 34   | 55   | 89   | 144  | 233  | 377  |

$F_{13} -1 = 232 \\ \Rightarrow F_{13} = F_{11+2} \\ \Rightarrow h = 11$

> **最小高度為**？
>
> $2^h -1 = 300 \\ \Rightarrow h = \lceil \lg 301 \rceil = 9$



**Example**

- 令 N(H) 代表形成高度 H 之「AVL tree」之最小節點數 N(0) = 0、N(1) = 1
- （1）寫出 N(H) 之遞迴定義
- （2）延續（1），求出 N(5) 與 N(10)

（1）：**N(H) = N(H-1) + N(H-2) + 1**，H ≧ 2；

（2）：**N(5) = 12，N(10) = 144**



# M-way search tree



主要應用在「External search (sort)」，資料量大於記憶體空間，必須藉由外部儲存體保存，再分批載入記憶體中搜尋欲求的資料



Example ( Height：h，m-way search tree )

- （1）最多節點個數
- （2）最多儲存資料數量

（1）：$m^0 + m^1 + m^2 + \ldots + m^{h-2} + m^{h-1} = \frac{m^h-1}{m-1}$

（2）：$\frac{m^h-1}{m-1} \cdot (m-1) = m^h -1$



## B-tree of order m (Balance)



- **樹根節點至少有 ≧ 2 個子節點，即 m ≧ 根節點 Degree ≧ 2**
- 除了樹根節點與失敗節點( Failure node；External node ) 之外，**其餘的節點**
  - $\lceil \frac m2 \rceil \leq Degree \leq m$ 或 $\lceil \frac m2 -1 \rceil \leq 節點內的資料數量 \leq m-1$
- **所有的失敗節點皆必須位於同一層 ( Level )**



![mwaybtree](\willywangkaa\images\mwaybtree.png)

從上一層讀取至下一層需要多一次 Block I/O



### **B** tree of order 3



- 樹根節點：2 ≦ Degree ≦ 3
- 除失敗節點之其餘節點：$\lceil \frac 32 \rceil = 2$ ≦ Degree ≦ 3

> 此樹中樹根節點**恰巧**與其餘節點 Degree 的特性相同，所以只會出現兩種節點
>
> - 2 - node：Degree 為 2
> - 3 - node：Degree 為 3
>
> 所以又稱此樹為「2-3 tree」



### B tree of order 4



- 樹根節點：2 ≦ Degree ≦ 4
- 除失敗節點之其餘節點：$\lceil \frac 42 \rceil = 2$ ≦ Degree ≦ 4 

> 此樹中樹根節點**恰巧**與其餘節點 Degree 的特性相同，所以只會出現三種節點
>
> - 2 - node：Degree 為 2
> - 3 - node：Degree 為 3
> - 4 - node：Degree 為 4
>
> 所以又稱此樹為「2-3-4 tree」



> B tree of order 5
>
> - 樹根節點：2 ≦ Degree ≦ 5
> - 除失敗節點之其餘節點：$\lceil \frac 52 \rceil = 3$ ≦ Degree ≦ 5
>
> **此樹中樹根節點與其餘節點 Degree 的特性不同**



在**固定高度**之下：

- 最多節點數

  - $m^0 + m^1 + \ldots + m^{h-1} = \frac{m^h-1}{m-1}$

- 最多資料儲存量

  - 因為每一節點都會有 m-1 筆資料，所以將最多節點數乘每節點最多
    可儲存量 $\frac{m^h-1}{m-1} \times (m-1) = m^h -1$

- **最少節點數**

  - 數根節點最少要有兩個子節點
  - 其餘節點最少要有 $\lceil \frac m2 \rceil$ 個子節點
  - 則 $1 + 2 + 2\cdot \lceil \frac m2 \rceil + 2^2\cdot \lceil \frac m2 \rceil^2 + \ldots + 2^h\cdot \lceil \frac m2 \rceil^{h-2}  \\  = 1 + 2\cdot[\lceil \frac m2 \rceil^0 + \lceil \frac m2 \rceil^1 + \ldots + \lceil \frac m2 \rceil^{h-2} ] \\ =  1 + 2\cdot [\frac {\lceil\frac m2 \rceil^{h-1}-1}{\lceil\frac m2\rceil -1}]$

- **最少資料儲存量**

  - 根節點最少可以儲存 1 筆資料
  - 每個節點最少可以儲存 $\lceil \frac m2 \rceil -1 $ 筆資料
  - 則 $1 + 2\cdot [\frac {\lceil\frac m2 \rceil^{h-1}-1}{\lceil\frac m2\rceil -1}] \times (\lceil \frac m2\rceil-1) \\ = 1 + 2\cdot(\lceil\frac m2\rceil^{h-1}-1) \\ = 2\cdot\lceil\frac m2\rceil^{h-1}-1$ 


## Insert data in B-tree



步驟：

1. 先找尋資料 x 在樹中的置入位置
2. **檢查該置入節點是否「Overflow」( 即資料數量等於 m )**
   1. 如果無「Overflow」，結束
   2. 如果「Overflow」針對該點「Split」，再針對該父點做第二步的檢查



> Split
>
> 將節點內的第 $\lceil \frac m2 \rceil$ 個資料上拉至父節點，其餘資料分裂成兩個子節點
>
> ![mwaybtree_split](\willywangkaa\images\mwaybtree_split.png)

> 註解：B tree 不直接新增節點，而是等到整個資料樹的樹根節點「Overflow」才會新增節點。



## Delete data in B-tree



步驟：

1. 在樹中找尋 x 資料位於的節點
2. 將節點分為「葉節點」與「非葉節點」
   - 「葉節點」
     1. 將資料 x 移除
     2. 檢查節點是否「Underflow」( 資料數量 $\geq \lceil\frac m2\rceil-1$ )；
        若無「Underflow」結束；
        若有「Underflow」檢查是否可以「Rotation」，可以則執行之並結束，**若否則作「Combine」( Merge ) 並對父節點執行此步驟 ( 檢查節點是否「Underflow」 )**
   - **「非葉節點」( 注意 )**
     1. **找出 x 之左子樹中的最大值 y ( 或右子樹中的最小值 )**
     2. y 必存在於某「葉節點」之中，以資料 y 取代資料 x 並移除 y **( 相當於刪除「葉節點」的一筆資料 )**



> Rotation
>
> ![mwaybtree_ratation](\willywangkaa\images\mwaybtree_ratation.png)



> Combine
>
> 
![mwaybtree_combine](\willywangkaa\images\mwaybtree_combine.png)
>
> 若父節點因此發生「Underflow」，則兩代 ( 父、子 ) 所有鍊節全部斷開，再**針對父節點的「Underflow」逐層往上處理**
>
> ![mwaybtree_split_2](\willywangkaa\images\mwaybtree_split_2.png)



Example ( 刪除 Leaf 之資料 )

2 - 3 Tree ( 2 ≦ Degree ≦ 3；1 ≦ 資料量 ≦ 2 )，刪除 5, 50, 8, 90

![mwaybtree_deletion](\willywangkaa\images\mwaybtree_deletion.png)



刪除 5 後




![mwaybtree_deletion_2](\willywangkaa\images\mwaybtree_deletion_2.png)



刪除 50 後



![mwaybtree_deletion_3](\willywangkaa\images\mwaybtree_deletion_3.png)



刪除 8 後




![mwaybtree_deletion_4](\willywangkaa\images\mwaybtree_deletion_4.png)



刪除 90 後




![mwaybtree_deletion_5](\willywangkaa\images\mwaybtree_deletion_5.png)





# Red black tree



Balanced binary search tree



1. 節點顏色非黑即紅
2. 樹根節點必為黑節點
3. 「Nil」節點也視為黑節點
4. 紅節點其子節點必為黑節點 ( 任何路徑上不得出現連續紅節點 )
5. 樹根至任意不同之「葉節點」路徑**皆具有相同數量的黑色節點 ( Balance )**


![rbtree](\willywangkaa\images\rbtree.png)



> 在紅黑樹中
>
> 1. 最短路徑：全為黑色 ( 鏈結、節點 ) 之路徑
> 2. 最長路徑：黑紅交錯 ( 鏈結、節點 ) 之路徑
> 3. 「2-3-4 tree」高度 = $\lceil \log(n+1)\rceil$
>    - 全為「2－節點」：$\lceil \log_2(n+1)\rceil$ ( 最小高度 )
>      - 因為當「2-3-4 tree」全為「2－節點」時，**所建出之紅黑樹會一樣高 ( 全部皆為黑色路徑 )**
>    - 全為「4－節點」：$2 \cdot \lceil \log_2(n+1)\rceil$ (最大高度)
>      - 因為當「2-3-4 tree」全為「4－節點」時，**所建出之紅黑樹會是兩倍高 ( 黑紅路徑交錯出現 )**



## Insert x in RB-tree (Top-down)



步驟：

1. 先找尋 x 的插入位置
   - **在找尋 x 的插入位置時，所經過得路徑上，若發現有節點具有兩個紅色子節點，使其「顏色交換 ( color change )」，再檢查有無違反「連續之紅色節點」，若有作「Rotation」侯進行下一步 (插入)，若無直接進行下一步**
2. 將插入之節點標為紅色後放置在該插入位置
3. 檢查有無違反「連續之紅色節點」，若有作「Rotation」進行下一步，若無則直接進行下一步
4. 如果需要，將樹根節點改為黑色節點



> **注意**：第一步與第三步只會有一者發生



> Rotation
>
> 分為 LL、LR、RL、RR ，與 AVL tree 旋轉相似，只是加上顏色變化：中間鍵值往上拉標黑，兩子點標紅



## Delete x in RB-tree



目前不討論



# Red black tree ( 不同定義 )



為一棵被「2-3-4 tree」所對應之「Binary search tree」



1. 「鏈結」非黑即紅
2. 若該「鏈結」在原本「2-3-4 tree」就已經存在則視為黑色鏈結，否則視為紅色鏈結
3. 任何路徑上不可連續出現紅色鏈結
4. 樹根節點到不同葉節點的路徑上皆具有相同數量之黑色鏈結



- 2－結點




![2-nodetorbtree](\willywangkaa\images\2-nodetorbtree.png)



- 3－結點



![3-nodetotbtree](\willywangkaa\images\3-nodetotbtree.png)


> 3－結點轉換時會有兩種可能，所以轉換不唯一



![4-nodetorbtree](\willywangkaa\images\4-nodetorbtree.png)



# Optimal binary search tree (OBST)



給 n 個內部節點加權值：$p_i$，**1 ≦ i ≦ n**，與 n+1 個外部節點加權值：$q_i$，**0 ≦ j ≦ n**

在所有 $\frac {1}{n+1}\binom{2n}{n}$ 不同種二元搜尋樹中，具有最小的搜尋總成本之二元搜尋樹 ( 不唯一 )



- Search total cost = 成功搜尋成本 + 失敗搜尋成本
- 成功搜尋成本 = $\sum^n_{i = 1} (內部節點_i \;之「Level」值\times p_i)$
  - 內部節點$_i$ 之「Level」值：**比較次數**
- 失敗搜尋成本 = $\sum^n_{j = 0} ((外部節點_j \;之「Level」值-1)\times q_i)$
  - **外部節點**$_j$ **之「Level」值 - 1：比較次數**




![orbtree](\willywangkaa\images\orbtree.png)

- 圖（1）
  - 成功搜尋成本 = 1 × 0.5 + 2 × 0.1 + 3 × 0.05 = 0.85
  - 失敗搜尋成本 = 1 × 0.15 + 2 × 0.1 + 3 × 0.05 + 3 × 0.05 = 0.65
  - 總成本 = 0.85 + 0.65 = 1.5
- 圖（2）
  - 成功搜尋成本 = 1 × 0.1 + 2 × 0.5 + 2 × 0.05 = 1.2
  - 失敗搜尋成本 = 2 × ( 0.15 + 0.1 + 0.05 + 0.05 ) = 0.7
  - 總成本 = 1.2 + 0.7 = 1.9



> 在有加權值之影響下，不見得高度越小，成本越小



## 建立 OBST



> Dynamic programming 三要件
>
> - 定出針對該問題之遞迴公式
> - 建立表格
> - 利用表格



假設 $a_{i+1}, a_{i+2}, \ldots, a_{j}$ 為內部節點，且 $a_{i+1} < a_{i+2} < \ldots < a_{j}$

令內部節點加權值 = $p_{i+1}, p_{i+2}, \ldots, p_{j}$；外部節點加權值 = $q_{i}, q_{i+1}, \ldots, q_{j}$

> 定義
>
> $T_{i, j}$ 代表由 $a_{i+1}, a_{i+2}, \ldots, a_{j}$ 所組成之「OBST」，且令 $T_{i, i}$ 為空樹，而當 i > j 時無定義



Example

- $T_{2, 5}$：$a_3, a_4, a_5$ 所組成之 OBST
- $T_{2, 3}$：$a_3$ 所組成之 OBST



定義

- $C_{i, j}$ 代表 $T_{i, j}$ 的成本
- $r_{i, j}$ 代表 $T_{i, j}$ 之樹根節點編號
- $w_{i, j}$ 代表 $T_{i, j}$ 之**內外部節點加權值和**
- 若為空樹 $C_{i, i} = 0 \\ r_{i, i} = nil \\ w_{i, i} = q_i$

### 遞迴推導

1. 在 $a_{i+1}, a_{i+2}, \ldots, a_{j}$ 中任挑一個作為樹根 ( $a_k$ )



![OBST_1](\willywangkaa\images\OBST_1.png)


則 L 子樹以 $T_{i, k-1}$ 表示； R 子樹以 $T_{k, j}$ 表示

$C_{i, j} = 1\cdot p_k + c_{i, k-1} + c_{k, j} + w_{i, k-1} + w_{k, j} \\ = c_{i, k-1} + c_{k, j} + w_{i, j}$ 



![OBST_2](\willywangkaa\images\OBST_2.png)



2. 在所有可能中挑出一個能使當前子樹成本最低之樹根


$$
C_{i, j} = w_{i, j} + min_{i+1 \leq k \leq j}｛C_{i, k-1}, C_{k, j}｝
$$


> 在 Comman 之演算法教科書中
>
> 失敗搜尋成本 = $\sum^n_{j = 0} (外部節點_j \;之「Level」值\times q_i)$，不用多扣除一次「搜尋次數」
>
> 所以，可以先依上述方式計算完後之總成本**再加上多出來的成本**，如下：
> $$
> ［Comman］總成本 = ［上述算法］總成本 + \sum_{j = 0}^n q_j
> $$
>



## 特殊題型



若題目未給出外部節點之加權值，將外部節點加權值視為 0，再沿用原本作法計算



- 重新定義 $T_{i, j}$ 
  - 為 $a_{i} , a_{i+1} , \ldots , a_{j}$ 所組成之 OBST
  - 內部節點之加權值為 $p_{i}, p_{i+1}, \ldots, p_{j}$
  - $C_{i, j}$ 為 $T_{i, j}$ 之總成本




![OBST_3](\willywangkaa\images\OBST_3.png)


$$
C_{i, j} = 1\cdot p_k + C_{i, k-1} + C_{k+1, j} + w_{i, k-1} + w_{k+1, j} \\
= w_{i, j} + C_{i, k-1} + C_{k+1, j} \\
\Rightarrow C_{i, j} = w_{i, j} + min_{i\leq k \leq j}｛C_{i, k-1} + C_{k+1, j}｝
$$


# Splay tree



|                     | AVL tree | Splay tree |
| ------------------- | -------- | ---------- |
| 調整過程            | 複雜     | 簡單       |
| 實際最差行況        | O(log n) | O(n)       |
| 分攤成本 (Amortize) | N/A      | O(log n)   |



正常的插入與刪除節點只是會需要加上「Splay」操作



> Splay：將該起點旋轉至整棵樹樹根節點
>
> - Search (x)：x 為作「Splay」之起點
> - Insert (x)：x 為作「Splay」之起點
> - Delete (x)： x 之父節點為作「Splay」之起點 ( 若存在 )