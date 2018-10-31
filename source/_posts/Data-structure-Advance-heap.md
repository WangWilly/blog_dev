title: Data structure - Advance heap
author: Willy Wang (willywangkaa)
tags:
  - Heap
categories:
  - Data structure
date: 2018-10-31 13:37:00
---
# Binary heap



## Build heap



### Top down



依序插入所有資料逐步建立「Heap」

> Time complexity for heap insertion：log n

- Time complexity 若要建立一個 n 筆資料之「Heap」
  - $\sum_{i = 1}^n \log i = \log(n!) = n\log n$
  - $\Theta(n\log n)$



### Buttum up



1. **將 n 個輸入資料先以「Complete binary tree」( 陣列 ) 放置好**
2. 從最後一個父節點 (A[i]) 開始，往回執行 ( A[i-1], A[i-2] ... ) 對各個父節點，調整子樹成為「Heap」，直到 A[1] 為止




![buildheap](\willywangkaa\images\buildheap.png)



#### Time complexity



> 因為是一個「Complete binary tree」( 完全二元樹 )，所以樹高為 $\lceil \log (n+1) \rceil$



討論




![buildheap_2](\willywangkaa\images\buildheap_2.png)



若父節點位於「Level i」調整其子樹成為「Heap」之最大成本為 **K-i**

而第 i 層最多有 $2^{i-1}$ 個子樹需要調整，則最大總成本為 $\sum_{i=1}^k 2^{i-1}\times (K-i) = \sum_{i=1}^{k-1} 2^{i-1}\times (K-i)$


$$
S = 2^0 \cdot (k-1) + 2^1 \cdot (k-2) + \ldots + 2^{k-2} \cdot (k-(k-1)) + 2^{k-1}\cdot (k-k) \\
= 2^0 \cdot (k-1) + 2^1 \cdot (k-2) + \ldots + 2^{k-2} \cdot 1 ...........(1)\\
\Rightarrow 2S = 2^1 \cdot (k-1) + 2^2 \cdot (k-2) + \ldots + 2^{k-1} \cdot 1.........(2) \\
2S-S = -2^0\cdot(k-1) + 2 + 2^2 + \ldots + 2^{k-2} + 2^{k-1} \\
\Rightarrow S = -(k-1) + 2\cdot(\frac{2^{k-1}-1}{2-1}) = -k+1+2^k - 2 = 2^k-k-1 \\ = 2^{\lg n} - \lg n -1 = n - \lg n-1 \\
\Rightarrow \Theta(n)
$$

- Algorithm ( Heapify )

Max-heap

```cpp
// tree: Array[1..n]
// n   : 元素個數
// i   : 節點編號 (調整以 i 為樹根之子樹)
Heapify(tree, i, n) {
    x = tree[i];
    j = 2 * i                     //左子
    while(j <= n) {
        if(j < n)                 // 具有右子
            if(tree[j]<tree[j+1])
                j = j+1;
        if(x >= tree[j])          // 已經為 heap
            break;
        else {
            tree[j/2] = tree[j];
            j = 2 * j;
        }
    }
    tree[j/2] = x;                // 將資料 x 置於正確位置
}
```



- Build



```cpp
Build(tree, n) {
    for(i = n/2; i>= 1; i--) {     // i = n/2 為最後一個父節點
        Heapify(tree, i, n);
    }
}
```



Example ( Delete max )

- Time complexity：O(log n)

```cpp
// ...
x = tree[1];
tree[1] = tree[n];
n--;
heapify(tree, 1, n);
```



Example ( Merge heap )

- Time complexity：O(n)

```cpp
// H1:[1..n]
// H2:[1..m]

Heap H3[1..m+n];
copy(H3[1..n], H1);   // O(n)
copy(H3[n+1..m], H2); // O(n)

Build(H3, m+n);       // O(n)
```



# Double ended priority queue



- 時間複雜度特性
  - Insert：O(log n)
  - Del_min：O(log n)
  - Del_max：O(log n)



## Min-max heap



Complete tree

- 每層由「Min-level」與「Max-level」交互合併
- 樹根位於「Min-level」
- Min-level
  - 位於此層之節點在該子樹中有「最小值」
- Max-level
  - 位於此層之節點在該子樹中有「最大值」



![minmaxheap](\willywangkaa\images\minmaxheap.png)



> Minimum value = 樹根節點
>
> Maximum value = max｛樹根節點之左右子節點｝



### Insert node in min-max heap



步驟：

1. x 先置於最後一個節點之下一個位置 ( n = Min-max heap.size + 1 )
2. 令 p 為「 x 的父節點」

```cpp
// case1 "p": 位於「Min-level」
if(x < H[p]) {
    H[n] = H[p];       // 父節點下移
    // verifyMin( binary_heap, position, value )
    verifyMin(H, p, x);   // 往祖父點測試有無違反「Min-level」性質
} else {
    verifyMax(H, n, x);   // 往祖父點測試有無違反「Max-level」性質
}
// case2 "p": 位於「Max-level」
if(x > H[p]) {
    H[n] = H[p];       // 父節點下移
    // verifyMax( binary_heap, position, value )
    verifyMax(H, p, x);   // 往祖父點測試有無違反「Max-level」性質
} else {
    verifyMin(H, n, x);   // 往祖父點測試有無違反「Min-level」性質
}
```



- Time complexity
  - Step 1：O(1)
  - Step 2：O(1) + O($\frac{\lg n}{2}$) (Verify) = O(lg n)



### Delete-min in min-max heap



步驟：

1. 移出樹根資料
2. 將最後一節點先暫存於 x 後刪除
3. **x 插入樹根**

```cpp
// H: Heap
// n: Heap 大小
// r: 目前子樹樹根編號
// x: 重新插入值
Del_reinsert(H, n, r, x) {
    // case 1
    if(r*2 > n) {                                   // 檢查子樹樹根有無左子節點，若無則無子節點
        H[r] = x;
    // case 2
    } else if (n*4 > n) {                           // 樹根無孫節點，所以此樹最小值可能落在樹根之子節點中
        int k = r*2;
        if(H[k] > H[k+1])
            k++;
        
        if(x < H[k]) {                              // 此樹最小值在子樹樹根子節點中
            H[r] = x;
        } else {
            H[r] = H[k];
            H[k] = r;
        }
    // case 3
    } else {
        int k;
        int j = r*4;
        int min = oo;
        for(int i = j; i <= j+4 && i<= n; i++) {
            if(H[i] < min) {
                k = i;
                min = H[i];
            }
        }
        int p = k/2;                                 // p 為 k 之父節點
        // k 必位於「Min-level」
        if(H[k] < x) {                               // 此樹最小值在子樹樹根孫節點中
            H[r] = H[k];
            if(x > H[p]) Swap(x, H[p]);              // !!
            Del_reinsert(H, n, k, x);                // "x"插入原本 k 所在之位置
        } else {
            H[r] = x;
        }
    }
}
```



```cpp
Del_min (H, n) {
    swap(H[1], H[n]);
    re = H[n];
    n--;
	Del_reinsert(H, n, 1, H[1]);
}
```



Example (Delete 3 times)



![delminmaxheap_1](\willywangkaa\images\delminmaxheap_1.png)



- 1 st deletion




![delminmaxheap_2](\willywangkaa\images\delminmaxheap_2.png)



![delminmaxheap_3](\willywangkaa\images\delminmaxheap_3.png)


**Seep 3** ↓



![delminmaxheap_4](\willywangkaa\images\delminmaxheap_4.png)



![delminmaxheap_5](\willywangkaa\images\delminmaxheap_5.png)


- 2nd delition



![delminmaxheap_6](\willywangkaa\images\delminmaxheap_6.png)



![delminmaxheap_7](\willywangkaa\images\delminmaxheap_7.png)



![delminmaxheap_8](\willywangkaa\images\delminmaxheap_8.png)


![delminmaxheap_9](\willywangkaa\images\delminmaxheap_9.png)



- 3rd delietion



![delminmaxheap_10](\willywangkaa\images\delminmaxheap_10.png)




![delminmaxheap_11](\willywangkaa\images\delminmaxheap_11.png)




![delminmaxheap_12](\willywangkaa\images\delminmaxheap_12.png)



#### Delete-max


![delminmaxheap_1](\willywangkaa\images\delminmaxheap_1.png)




![delminmaxheap_13](\willywangkaa\images\delminmaxheap_13.png)




![delminmaxheap_14](\willywangkaa\images\delminmaxheap_14.png)





![delminmaxheap_15](\willywangkaa\images\delminmaxheap_15.png)



## Double-ended heap (DEAP)



Complete binary tree

- 樹根不賦值
- 樹根左子樹為「Min heap」
- 樹根右子樹為「Max heap」




![deapnodemap](\willywangkaa\images\deapnodemap.png)


- **「Min heap」與「Max heap」對應編號**
  - 令 i 為在「Min-heap」中某一節點之**編號**，則 j 為「i 在 Max-heap 中對應」之節點編號
  - $j = i + 上一層之最多節點數量 \\ = i + 2^{(目前節點之「Level」-1)-1} \\ = i + 2^{\lceil\lg (i+1)\rceil-2}$
  - 如果該節點 j 不存在 ( j > n )，則 j = $[\frac i2 ]$



### Insert data in deap



步驟：

1. x 先置於最後一個節點之下一個位置 ( n = Deap.size + 1 )
2. 檢查

（1）若 x 目前位於「Min-heap」( 位置假設為 i )，則 j 為 x 在「Max-heap」對應之節點編號

```cpp
if(x > deap[j]) {
    deap[i] = deap[j];
    //max_heap_insert(陣列, 陣列位置, 資料);
    max_heap_insert(deap, j, x);          // 將 x 與該父節點比較並調整
} else {
    //min_heap_insert(陣列, 陣列位置, 資料);
    min_heap_insert(deap, i, x);          // 將 x 與該父節點比較並調整
}
```

（2）若 x 目前位於「Max-heap」( 位置假設為 j )，則 i 為 x 在「Min-heap」對應之節點編號

```cpp
if(x < deap[i]) {
    deap[i] = deap[j];
    //min_heap_insert(陣列, 陣列位置, 資料);
    min_heap_insert(deap, i, x);          // 將 x 與該父節點比較並調整
} else {
    //max_heap_insert(陣列, 陣列位置, 資料);
    max_heap_insert(deap, j, x);          // 將 x 與該父節點比較並調整
}
```



### Delete data in deap



步驟：

1. 將左子樹樹根資料移出
2. 將最後一節點刪除並該資料先由 x 暫存
3. 左子樹樹根之空缺，由其左右子節點中最小遞補之，由上而下直到某「葉節點」產生空缺
4. 對該空缺作「Insert data in deap」



![deap](\willywangkaa\images\deap.png)



Example




![deldeap_1](\willywangkaa\images\deldeap_1.png)



![deldeap_2](\willywangkaa\images\deldeap_2.png)




![deldeap_3](\willywangkaa\images\deldeap_3.png)




![deldeap_4](\willywangkaa\images\deldeap_4.png)



![deldeap_5](\willywangkaa\images\deldeap_5.png)



## Symmetric min-max heap (SMMH)



Complete binary tree

- 樹根不賦值
- 條件一 ( $P_1$ )：左兄弟 ≦ 右兄弟
- **條件二** ( $P_2$ )**：若該節點 x 有祖父節點則「祖父節點之左子節點鍵值」 ≦ x**
- **條件三** ( $P_3$ **)：若該節點 x 有祖父節點則「祖父節點之右子節點鍵值」 ≧ x **



針對下方全部綠色節點，全部鍵值都比 2 大，都比 80 小


![SMMH](\willywangkaa\images\SMMH.png)

> 由上方性質可以反映出
>
> 「以節點邊號 i 為樹根之子樹中，其子孫最小值為左子節點，其子孫最大值為右子節點」



### Insert data in SMMH



步驟：

1. x 先置於最後一個節點之下一個位置 ( n = SMMH.size + 1 )
2. **檢查「條件一」是否滿足，若違反「左右兄弟節點互換」**
3. **檢查「條件二」、「條件三」是否滿足，若違反其中一者則調整之**



### Delete min in SMMH



步驟：

1. 移出左子樹根之資料
2. **刪除最後一個節點並將值暫存於 x**
3. **x 暫置於左子樹根之空缺**
4. **檢查「條件一」是否滿足，若違反「左右兄弟節點互換」**
5. **檢查「條件二」是否滿足，若違反則調整之**
6. 重複步驟四、五直到完全符合條件



Example




![SMMH_2](\willywangkaa\images\SMMH_2.png)




![delSMMH_1](\willywangkaa\images\delSMMH_1.png)

在藍色中選出一個最小值

![delSMMH_2](\willywangkaa\images\delSMMH_2.png)


![delSMMH_3](\willywangkaa\images\delSMMH_3.png)


![delSMMH_4](\willywangkaa\images\delSMMH_4.png)


![delSMMH_5](\willywangkaa\images\delSMMH_5.png)


![delSMMH_6](\willywangkaa\images\delSMMH_6.png)



# Leftist heap



|             |     Min-heap     | Leftist heap |
| :---------: | :--------------: | :----------: |
| Insert data |     O(log n)     |   O(log n)   |
| Delete min  |     O(log n)     |   O(log n)   |
| Merge heap  | O(n)：build heap |   O(log n)   |



## Leftist tree (Minimum leftist tree)



- Shortest：到任一個外部節點之最短路徑長

令 x 為「Extended binary tree」中某個節點，則

Shortest(x) = $\left\{\begin{matrix}
0 &, x 為「外部節點」 \\ 
1 + min｛Shortest(x\rightarrow Lchild), Shortest(x\rightarrow Rchild)｝ & , 其他
\end{matrix}\right.$





- Leftist tree
  - 針對任何一個內部節點 x，其 Shortest(x→Lchild) ≧ Shortest(x→Rchild)



## Leftist heap



本身也為一棵「Leftist tree」且為一棵「Min-tree」( 父節點值 ≦ 子節點值 )



### Merge leftist heap ($H_1, H_2$)



步驟：

1. 比較 $H_1$ 與 $H_2$ 之根節點值，找出最小值。( 假設 $H_1$ 之根節點比較小 )
2. **以** $H_1$ **之根節點作為新樹之根節點，該新樹保留** $H_1$ **之左子樹**
3. Merge( $H_1$ 之右子樹, $H_2$ ) **成為新樹之右子樹** (遞迴)
4. 檢查所有節點是否符合「Leftist tree」之性質，若不滿足則作「Swap」



Example


![leftisttreemerge_1](\willywangkaa\images\leftisttreemerge_1.png)


![leftisttreemerge_2](\willywangkaa\images\leftisttreemerge_2.png)


![leftisttreemerge_3](\willywangkaa\images\leftisttreemerge_3.png)



### Delete-min in leftist heap



步驟：

1. 刪除根節點並輸出，斷開鏈結後得兩子樹 $H_1, H_2$
2. Merge($H_1, H_2$)



### Insert data in leftist heap



步驟：

1. 插入節點自成為一個「Leftist heap」$H_2$，令被插入之「Leftist heap」為 $H_1$
2. Merge($H_1, H_2$)



# Binominal heap



## Binominal tree



> **令樹高從 0 開始**

- 高度為 0 的「Binominal tree」記為 $B_0$，只有樹根一個節點
- 高度為 k 的「Binaminal tree」 記為 $B_k$，由兩棵 $B_{k-1}$ 組成 (任挑其中一點作為樹根)



> 1. $B_k$ 中「i-th level」之節點為 $\binom{k}{i}$
> 2. $B_k$ 之節點總數為 $2^k$



## Binominal heap



是由 ≧ 0 棵「Binominal tree」所組成之集合 ( 森林 ) 且每棵樹也為「Min-tree」



> 「Binominal heap」有 13 筆資料( 節點 )，則由 3 個「Binomal tree」組成
>
> - 若總結點數量 n = $2^k$
>   - 「Binominal heap」只有一棵「Binominal tree」
> - 若總結點數量 n = $2^k-1$
>   - 有 k 棵「Binominal tree」
> - k = lg ( n+1 )



### Merge binominal heap



步驟：

1. 將具有相同高度之「Binominal tree」合併成一棵新的「Binominal tree」
2. 合併直到此**集合中無相同高度之「Binominal 」**




![leftistheapmerge_1](\willywangkaa\images\leftistheapmerge_1.png)



![leftistheapmerge_2](\willywangkaa\images\leftistheapmerge_2.png)

> 1. 合併兩棵樹花費 O(1)，最多大約 lg n 棵樹合併 ( O(lg n) )
> 2. 上述之合併方法稱為「勤勞合併」( Hard-working merge )
> 3. **另一種合併方法為「偷懶合併」( Lazy merge )**
>    - **相同高度不合併，純粹串接在同一集合中( O(1) )**



### Delete-min in binominal heap



步驟：

1. 從各個樹根找出最小值，令存在最小值數根之樹為 T，其餘樹之集合稱為 $H_1$ (Binominal heap)
2. 刪 T 的樹根會生成子樹，稱之為 $H_2$
3. Merge($H_1, H_2$)



> 1. 在所有樹根找最小值，因為最多才 lg (n+1) 棵樹，所以尋找最小值只需 O( log n ) 即可
> 2. 在作「Delete-min」時，必採取「勤勞合併」



### Insert data in binominal heap ($H_1$)



步驟：

1. 插入之資料 x，自己成為一個「Binominal heap」( $H_2$ )
2. Merge( $H_1, H_2$ )



Example (**Amotize 計算**) 給 1, 2, 3, 4, 5, 6, 7 以建立「Binominal heap」


![insertbinominalheap_1](\willywangkaa\images\insertbinominalheap_1.png)



![insertbinominalheap_2](\willywangkaa\images\insertbinominalheap_2.png)



![insertbinominalheap_3](\willywangkaa\images\insertbinominalheap_3.png)



> - 大部分插入的時間為 O( 1 )
> - 少部分插入的時間為 O( lg n )
>   - 當 n = $2^k -1$ 增加一個節點變成 $2^k$，有 k 棵樹要合併成一棵
> - Amotized cost：O( 1 )



## Binominal heap ( 不同版本 )




![datastructurebinominalheap](\willywangkaa\images\datastructurebinominalheap.png)

- 著重於「Data structure」之表示



# Fibonacci heap



> [Corman] Fibonacci heap 也是用上述相同的資料結構



| Fibonacci heap |     Algorithm wiss版本      |    Data structure 版本     |
| :------------: | :-------------------------: | :------------------------: |
|  Insert data   |     **O(1)：Amotized**      |     **O(1)：Amotized**     |
|   Delete min   | O( log n )：採**勤勞合併**  | O( log n )：採**勤勞合併** |
|     Merge      | O( log n )：採**勤勞合併**  |   O( 1 )：**採偷懶合併**   |
|    Find-min    | O( log n )：有 log n 個樹根 |  O( 1 )：採用「Min」指標   |



## Fibonacci heap - Data structure



為「Extended binominal heap」亦為「Binominal heap」之「Supperset」；除了「Binominal heap」之「Insert data」、「Merge」、「Delete min」，另外有：

1. **Delete x (任意刪除資料)：O( log n ) 考慮刪除資料恰為「Min-data」**
2. **Decrease key of a node (減少某點之鍵值)：O(1) 採用「Amotized cost」**
   - 應用於圖論中，**如「Minimum spanning tree」之「Kruskal、Prim 演算法」**



### Delete x in fibonacci heap




![delfibonacciheap](\willywangkaa\images\delfibonacciheap.png)



Example 刪除節點 12

刪除該節點後，其兩棵子樹獨立，成為樹根之間之「Sibling」，**並且遇到相同高度「Binominal tree」時不合併 ( 偷懶合併法 )**




![delfibonacciheap_2](\willywangkaa\images\delfibonacciheap_2.png)



### Decrease key in fibonacci heap


![delfibonacciheap](\willywangkaa\images\delfibonacciheap.png)



- 針對節點（1）減少 1：**（1）→（0），其餘不動**
  - O( 1 )
- 針對節點（3）減少 1：**（3）→（2），其餘不動**
  - **O( 1 )**
- 針對節點（3）減少 3：**（3）→（0），其餘不動**
  - **需要改變「Min」指標 O( 2 )**
- 針對節點（15）減少 1：（15）→（14）
  - （14）> （12）：不用更動 O( 1 )
- **針對節點（15）減少 4：（15）→（11）**
  - （11）< （12）：**該子樹獨立 O( 1 )**，成為樹根之間之「Sibling」並檢查是否比「Min」還小




![decreasekeyfibonacciheap_1](\willywangkaa\images\decreasekeyfibonacciheap_1.png)


![decreasekeyfibonacciheap_2](\willywangkaa\images\decreasekeyfibonacciheap_2.png)





> |    Operation    |         Binary heap (worst cast)         |   Fibonacci heap (amotized)    |
> | :-------------: | :--------------------------------------: | :----------------------------: |
> | Make empty heap |               $\Theta(1)$                |          $\Theta(1)$           |
> |    Insert x     |             $\Theta(\log n)$             |          $\Theta(1)$           |
> |   Extract min   |             $\Theta(\log n)$             |        **O( \log n )**         |
> |  Union (Merge)  |         $\Theta(n)$：Build heap          |   $\Theta(1)$：**偷懶合併**    |
> |  Decrease key   |             $\Theta(\log n)$             |          $\Theta(1)$           |
> |    Delete x     | $\Theta(\log n)$：視為子 heap 中 Del-min |          **O(log n)**          |
> |    Find min     |            $\Theta(1)$：樹根             | $\Theta(1)$：採「Min pointer」 |
>
> - Decrease key in binary heap ( $\Theta(\log n)$ )
>
> ![decreasekeyinbinaryheap](\willywangkaa\images\decreasekeyinbinaryheap.png)