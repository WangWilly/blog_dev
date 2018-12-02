title: Data structure - Search and sort
author: Willy Wang (willywangkaa)
tags:
  - Search
  - Sort
  - ''
categories:
  - Data Structure
date: 2018-11-17 19:47:00
---


# Binary search



Example

n 筆資料作「Binary search」最多比較次數？

$\lceil \lg (n+1) \rceil$



> - 200 筆資料：最多 8 次的搜尋
>
> - 1024 筆資料：最多 11 次的搜尋



**Example**

1. n 筆資料作「Binary search」
   - 其「Worst case」為 O(log n)
2. n 筆資料以「Binary search tree」作查找
   - 其「Worst case」為 **O( n )** ( Skew tree )
   - 其「Best case」為 **O( log n )**



# Internal sort and external sort



## Internal sort



資料量少可一次載入到記憶體中進行排序



## External sort



資料量太多無法一次全部置入記憶體之中，需藉由外部儲存體 ( Disk ) 來保存再進行排序



- 常用的「External sorting method」
  - **Merge sort** ( Selection tree 輔佐 )
  - M-way search tree、B-tree、B$^+$-tree



# 排序性質比較表

> 此表為方便比較用圖，欲瞭解其推倒原因詳見下方細述

| Sort algorithm | T.C. (Best)     | T.C. (Worst) | T.C. (Avg.) | S.C.             | Stable  |
| -------------- | --------------- | ------------ | ----------- | ---------------- | ------- |
| Insertion      | **O(n)**        | $O(n^2)$     | $O(n^2)$    | O(1)             | Yes     |
| Selection      | $O(n^2)$☆       | $O(n^2)$     | $O(n^2)$    | O(1)             | No      |
| Bubble         | O(n)            | $O(n^2)$     | $O(n^2)$    | O(1)             | **Yes** |
| Shell          | $O(n^\frac 32)$ | $O(n^2)$     | $O(n^2)$    | O(1)             | No      |
| Quick          | O(nlogn)        | $O(n^2)$☆    | O(nlogn)    | **O(lgn)~O(n)☆** | No      |
| Merge          | O(nlogn)        | O(nlogn)     | O(nlogn)    | O(n)             | Yes     |
| Heap           | O(nlogn)        | O(nlogn)     | O(nlogn)    | **O(1)☆**        | No      |
| Radix          | N/A             | N/A          | O(d×(n+r))  | **O(r×n)☆**      | Yes     |
| Counting       | N/A             | N/A          | O(n+k)      | **O(n+k)☆**      | Yes     |

- 註
  - T.C. = Time complecity
  - S.C. = Space complexity
  - r 為「Redix sort」的基數大小
  - d 為「Redix sort」以 r 作為基數之位數最大值
  - k 為「Counting sort」之資料值域範圍



# 初等排序



## Insertion sort



- 演算法

```cpp
// 將資料 r 插入到已排好的區塊 A[0] ~ A[i]
Insert(A[], r, i) {
    j = i;
    while(r < A[j]) {
        A[j+1] = A[j];
        j = j-1;
    }
    A[j+1] = r;
}

Insort(A[], n) {
    A[0] = -oo;
    for(int i = 2; i <= n; i++) {
        Insert(A, A[i], i-1);
    }
}
```



Time complexity

- Best case：O(n)
  - 當「Input data」恰巧為小到大，每回合檢查一次即可確定 r 的插入位置，共作 n-1 回合所以 O(n)
- Worst case：O( $n^2$ )
  - 當「Input data」恰巧為大到小，$Total = 1+2+ \ldots+n-1 \\=\frac{n(n-1)}{2} = O(n^2)$

![1541038370084](\willywangkaa\images\1541038370084.png)

- Avg. case：O( $n^2$ )
  - 利用遞迴時間函數，令第 n 筆資料之平均比較次數 (Time complexity) 為 O(n)
  - $T(n) = T(n-1) + \frac n2 \\ = T(n-2) + \frac{n-1}2 + \frac n2 \\ \vdots \\ = \frac{1+2+\ldots+n}{2} = \frac{n(n+1)}{4} = O(n^2)$ 
- Space complexity ( 除了「Input data」之外所需空間 )
  - O( 1 )



![1541092068950](\willywangkaa\images\1541092068950.png)

- Stable
  - 因為這行程式碼`while(r < A[j]) do...`所以不會交換一樣大小的資料



## Selection sort



- 演算法

```cpp
void SelectionSort(int a[], int n) { 
    // Iterate through array elements 
    for (int i = 0; i < n - 1; i++) {   
        int min = i; 
        for (int j = i + 1; j < n; j++) 
            if (a[min] > a[j]) 
                min = j;
        if(min != i) swap(a[i], a[min]);
    } 
} 
```



![1541092807581](\willywangkaa\images\1541092807581.png)



- Time complexity
  - Best case：$O(n^2)$
  - Worst case：$O(n^2)$
  - Avg. case：$O(n^2)$

- Space complexity
  - O( 1 )



![1541093304800](\willywangkaa\images\1541093304800.png)

- Unstable



- 多用在大型紀錄( 由多欄位組成之資料 ) 之排序
  - **每回合最多一次資料交換 ( Swap )**，不會吃太多資料存取較有優勢
- `if(min != i) do...` 如果省略，可以省下比較之次數但是會多增加一次資料交換，所以適用於大多資料皆未落在正確位置之上時



## ☆Bubble sort


### 版本一



由左而右，兩兩互相比較，**若前者大於後者交換之**

> 當某一回合在檢查時，未發生資料交換 ( Swap ) 則可以提早結束演算法

> 此演算法稱為「Bubble」是因為在每一回合檢查完，當時「Sublist」中最大值會往最高位置走，形同大泡泡往水上浮起一般



- 演算法

```cpp
void bubblesort(A[], n) {
    for(int i = n; i >= 1; i--) {
        bool f = false;
        for(int j = 1; j < i; j++) {
            if(A[j]>A[j+1]) {
                swap(A[j], A[j+1]);
                f = true;
            }
        }
        if(!f) {
            break;
        }
    }
}
```



![1541094481788](\willywangkaa\images\1541094481788.png)



### 版本二



由左而右，**兩兩互相比較如果後者小於前者交換之**

> 此處之「Bubble」是將最小值往最小位置走



- 演算法

```cpp
void bubblesort(A[], n) {
    for(int i = 1; i <= n; i++) {
        bool f = false;
        for(int j = n; j > i; j--) {
            if(A[j] < A[j-1]) {
                swap(A[j], A[j-1]);
                f = true;
            }
        }
        if(!f)
            break;
    }
}
```



![1541094963273](\willywangkaa\images\1541094963273.png)



- Time complexity
  - Best case：O( n )
    - 在第一回合檢查時**歷經 (n-1) 次比較**，但無須作「Swap」即可將排序完成
    - $T(n) = 0 + (n-1), T(1) = 0$ ( 第一回合之比較次數 + 剩下 n-1 筆資料之「Bubble sort」)
  - Avg. case：O( $n^2$ )
    - $T(n) = O(n) + T(n-1)$ ( 每回合之平均比較次數 + 剩下 n-1 筆資料之「Bubble sort」)
    - $T(n) = O(n) + T(n-1) \\ \vdots \\ = T(1) + c \cdot(2 + \ldots + n) = O(n^2)$
  - Worst case：O( $n^2$ )
    - 在「Input data」恰巧為大至小之狀況 (見下圖)
    - 處理 n 筆資料時則為 $n + (n-1) + \ldots + 1 = O(n^2)$

![1541214129532](\willywangkaa\images\1541214129532.png)

- Space complexity
  - O( 1 )
- Stable
  - 因為 `if(A[j]>A[j+1])` 必須要大於才會交換資料所以為穩定排序法



## [Weiss 版本] Shell sort



從 1 元素到 n-span 元素，比較 A[i] 與 A[i+span] ，若前者大於後者則交換；每一回合需持續到**沒有交換**為止，再進入下一回合



- Span 型式（將會決定總回合數）
  - ( 一般型 ) $\lceil \frac {n}{2^k}\rceil$ 或 $[\frac{n}{2^k}]$
    - 第一回合 span 等於 $\frac n2$
    - 第二回合 span 等於 $\frac n4$
    - 以此類推，**最後一回合 span 為 1**
  - $2^k-1$
    - 第一回合 span 等於 15
    - 第二回合 span 等於 7
    - 以此類推，**最後一回合 span 為 1**
  - （自訂型）
    - 第一回合 span 等於 7
    - 第二回合 span 等於 5
    - 第三回合 span 等於 2
    - **最後一回合 span 為 1**

> 最後一回合 span 必須為 1

> 當此回合的 span 值為 k 時，則表示有 k 條「Sublist」要排序，



Example ( Span型：5、3、2、1 )

排序：9 8 7 2 3 5 1 4 6



![1542454674621](\willywangkaa\images\1542454674621.png)



- Algorithm

```cpp
// span 型態 => n/(2^k)
Shellsort(A[], n) {
    span = n/2;
    while(span >= 1) {
        do {
            f = 0;
            for(int i = 1; i <= (n-span); i++) {
                if(A[i] > A[i+span]) {
                    swap(A[i], A[i+span]);
                    f = 1;
                }
            }
        } while(f != 0);
    }
}
```



- Time complexity
  - Avg. case：$O(n^2)$
  - Worst case：$O(n^2)$
  - Best case：目前無定論，與「Span 型態」有關
    - $O(n^{\frac 32})、O(n^{\frac 54})、O(n^{\frac 76})$ 
- Space complexity
  - O(1)



![1542455003562](\willywangkaa\images\1542455003562.png)



- Unstable



# 高等排序



## Comparesion based ( Comparsion and swap ) sort 研究與探討



使用比較大小的方式進行排序可以使用一個「Decision tree」來表達



Example

三筆資料 R1, R2, R3 不知其大小關係，在排序之後之所有可能



![1541935896940](\willywangkaa\images\1541935896940.png)



> 根據上圖之「Decision tree」：
>
> 1. 為一個「Binary tree」
>    - 非葉節點：「比較過程之節點」( Comparsion node )
>    - 葉節點：「某個排序的結果」
> 2. 假設排序 n 筆資料：會產生 n! 之排序可能結果，會有 n! 個葉節點
>    - $\because 總節點數量 = 葉節點數量 + 非葉節點數量  = 2\times 非葉節點數量 + 1 \\有\; n! \;個葉節點 \Rightarrow n! -1 \;個非葉節點$
>    - $\because 有\; n! \;個葉節點，又為「Binary \;tree」 \\ Tree \;height (h) \Rightarrow 2^{h-1} \geq n! \\ \Rightarrow h-1 \geq \lceil \lg n! \rceil \\ \Rightarrow h \geq \lceil \lg n! \rceil +1 \Rightarrow \\  \therefore 總共的比較需要大於等於 \lceil n \lg n \rceil \approx n\lg n  \\ 「Comparsion\;based \; sort」 最快之\; Time \;complexity= \Omega ( n\lg n )$



**Example**

**五筆資料排序之比較次數至少為何？**

~~5 × lg 5 => 10~~

$\lceil \lg n!\rceil = \lceil\lg 5!\rceil = \lceil \lg 120 \rceil \approx 7$



> 若非使用「Comparsion based」則可以不受到此限制，**時間複雜度最快可達到線性時間**



## Quick sort



- Avg. case 在實際排序時間最快的方法
- 採用「Divide and conquer」作法



令陣列最左資料 A[1] 作為「Pivot key」，經過「分割」( Partition ) 動作後，將「Pivot」置於大小關係"最正確"的位置上



![1541215583779](\willywangkaa\images\1541215583779.png)

> 可用多線程電腦加速執行



### Quicksort1 ( Hoare partition )



![1541216603304](\willywangkaa\images\1541216603304.png)

Example

排序：6, 8, 3, 7, 5, 9, 4, 1, 10, 2



![1541217794632](\willywangkaa\images\1541217794632.png)



- Algorithm

```cpp
// 排序陣列 A[l] ~ A[u]
Qsort(A[], l, u) {
    // Partition
    if(l < u) {
        i = l;
        j = u+1;
        p = A[l];
        do {
            do {
                i++;
            } while(A[i] < p);
        
            do {
                j--;
            } while(A[j] > p);
        
            if( i <= j ) {
                swap(A[i], A[j]);
            }
        } while(i <= j);
        swap(A[l], A[j]);
        
        Qsort(A[], l, j-1);
        Qsort(A[], j+1, l);
    }
}
```



Example

排序：5*, 5, 5, 5, 5

![1541218724819](\willywangkaa\images\1541218724819.png)

- **Unstable**

- **Time complexity**
  - **Best case：O( n log n ) (下圖一)**
    - 「Partition」恰將「Input data」分成兩等分
    - $T(n) = O(n) + T(\frac n2) + T(\frac n2)$：**Partition time + 左右邊作「Quick sort」**
    - $T(n) = 2T(\frac n2) + cn\\ =nT(1) +c\cdot n\log n = O(n \log n)$
  - **Avg. case：O( n log n ) (下圖二)**
    - $T(n) = c\cdot n + \frac 1n\cdot \sum_{s = 1}^n (T(s) + T(n-s))$：**Partition time + 全部狀況之平均**
    - $\Rightarrow nT(n) = c \cdot n^2 + \sum_{s = 1}^n(T(s) + T(n-s))\\ \Rightarrow nT(n) = [(T(1)+T(n-1)) + (T(2)+T(n-2)) + \ldots + (T(n)+ T(0))] + cn^2 \\ = 2 \cdot[T(1)+T(2)+\ldots+T(n-1)] + T(n) + cn^2 ......(1) \\ (n-1) 代入\; (1) \; \\ \Rightarrow (n-1)T(n-1) = 2\cdot[T(1)+ T(2)+\ldots+T(n-2)]+T(n-1)+c\cdot(n-1)^2 ......(2) \\ (1) - (2) \Rightarrow nT(n) - (n-1)T(n-1) = 2T(n-1)+T(n)-T(n-1)+c\cdot(n^2 - (n-1)^2) \\ \Rightarrow nT(n) - nT(n-1) + T(n-1) = T(n-1) + T(n) + c(n^2-(n-1)^2) \\ \Rightarrow (n-1)T(n) = nT(n-1) + c(n^2- (n-1)^2) \\ \Rightarrow \frac{T(n)}{n} = \frac{T(n-1)}{n-1} + c (\frac{2n-1}{n(n-1)}) \Rightarrow \frac{T(n)}{n} = \frac{T(n-1)}{n-1} + c (\frac 1n + \frac{1}{n-1}) \\ \Rightarrow \frac{T(n)}{n} = c(\frac 1n+\frac 1{n-1} + \ldots+\frac 12) + c (\frac 1{n-1} + \frac 1{n-2} + \ldots + \frac 11) \\ \frac{T(n)}{n} = c(H_n -1) + c(H_n + \frac 1n) \\ \Rightarrow T(n) = 2 c\cdot n \cdot H_n - cn -c \\ = 2\cdot c \cdot n \log n - cn -c = O(n\log n)$ 
    - 上述之遞迴時間表示式**忽略了「Partition」後右邊還會少一筆資料**(下圖二)
      所以該表示應該為 $T(n) = \frac 1n \sum_{s = 0}^{n-1}(T(s) + T(n-1-s)) + c\cdot n$
  - **Worst case：O(** $n^2$ **) (下圖三)**
    - 當 pivot 恰為最小最大值時，作「Partittion」不會使「Devide and conquer」的優點顯現，所以當**整體資料為「由小到大」或「由大到小」時會有「Worst case」**
    - $T(n) = O(n) + T(n-1) \\ = T(1) + (2 + 3 + \ldots + n) \cdot c \\ = c \cdot \frac{(n+2)(n-1)}{2} = O(n^2)$



![1541765950716](\willywangkaa\images\1541765950716.png)

圖一



![1541766239354](\willywangkaa\images\1541766239354.png)

圖二



![1541766394245](\willywangkaa\images\1541766394245.png)

圖三



> 如何避免「Worst case」發生？
>
>
> 避免 Pivot 為最小值或最大值
>
> Randomized quicksort
>
> - 亂數挑一個數作為 Pivot
> - **還是有可能發生「Worst case」( 無法完全解決問題 )**
>
>
>
> Middle of three (下圖)
>
> - 做法
>    1. $M = \frac {L+U}{2}$
>    2. 比較 A[L], A[M], A[U] 找出三者中之中間值，以此**中間值**與 A[l] 交換
>    3. 選擇 A[L] 作為 Pivot (中間值) ，作「Quicksort partition」
> - 可以解決「Worst case」問題
>
>
>
> ![1541766823939](\willywangkaa\images\1541766823939.png)
>
>
>
> Median of medians
>
> ＜下方細述＞



- Space complexity ( 遞迴所需的「Stack space」 )
  - Best case (下圖一)：O( log n )
  - Worst case (下圖二)：O( n )



![1541816631365](\willywangkaa\images\1541816631365.png)

圖一



![1541816965630](\willywangkaa\images\1541816965630.png)

圖二



### Quicksort2 ( 「Algotithm」書中版本 )



Example

2 8 7 1 3 5 6 4 之第一次「Partition」

![1541818660118](\willywangkaa\images\1541818660118.png)



- Algorithm

```cpp
Quicksort(A[], p, r) {               // 排序 A[p] ~ A[r]
    if(p < r) {
        q = Partition(A[], p, r);    // 見下圖一
        Quicksort(A[], p, q-1);
        Quicksort(A[], q+1, r);
    }
}

Partition(A[], p, r) {
    pivot = A[r];
    i = p-1;
    for(l = p; i <= r-1; j++) {
        if(A[j] <= pivot) {
            i++;
            swap(A[i], A[j]);
        }
    }
    swap(A[i+1], A[r]);
    return i+1;
}
```



Example

the output of quicksort pass1

- 1 2 3 4 5



![1541927350444](\willywangkaa\images\1541927350444.png)



-  5 4 3 2 1



![1541927780299](\willywangkaa\images\1541927780299.png)



-  **5 5 5 5 5* ( 在「Quicksort 2」演算的情況下會是「Worst case」（O(**$n^2$**)），但是在「Quicksort 1」演算的情況下會是「Best case」 )**



![1541928492844](\willywangkaa\images\1541928492844.png)



> 如何改善上述問題？
>
> 1. 在「Partition」前先檢查該陣列是否相同：O( n )
> 2. 改採用「 Hoare partition」：Best case O( n log n )



### 問題與討論—Selection problem



問題概要：想要在一個**未排序的一維陣列**中，找到其最大值與最小值



- Native solution
  1. 歷經 n-1 次比較後找出最大值
  2. 剩下 n-1 筆資料中歷經 n-2 次比較找出最小值
     - 總**比較**次數：(n-1) + (n-2) = **2n-3 次比較**

- 改良解法
  1.  A[1] 與 A[2] **比較一次**知道兩數大小，令兩者之大數為 m 、小數為 n
  2. 針對後面 n-2 筆資料以**遞迴**找出**最大值**與**最小值**，並且令 n-2 筆資料的最大值為 x、最小值為 y
  3. m 與 x **比較一次**找出最大值；n 與 y **比較一次**找出最小值
     - 總**比較**次數：$T(0) = T(1) = 0,  T(2) = 1 \\T(n) = T(n-2) + 3 \\  =  T(n-4) + 6 \\= T(n-6) + 9 \\\vdots = T(0) + 3 \cdot \frac n2 < 2n-3$ 
     - 稍微在比較上可以減少比較次數



### 問題與討論—Select i-th item among n unsorted data array



問題概要：在未排序的陣列中找到第 i 小的資料

> **想法**：以學過的算法中，若不知道其值域則只能使用「Quick sort」等算法來求取資料各個大小的資訊（O(nlogn)），倘若知道資料範圍，可以「Radix sort」等算法求取資料的大小資訊（O(n)）
>
> **如果要以「Comparsion based」來解決這個問題有無更快的算法？**

**利用「Quick sort」中的「Partition」為此算法基底**



- Algorithm

```cpp
// 在 A[p]~A[r] 中找到第 i 小的資料
Select(A[], p, r, i) {
    q = Partition(A, p, r); // 將 pivot 大小定位在 q 點
    k = q-p+1;              // 算出 pivot 是第 k 小的資料
    if(i == k) {            // pivot 即為 A[p]~A[r] 中第 k 小的資料
        return A[q];
    } else if(i < k) {
        Select(A, p, q-1, i);
    } else {
        Select(A, q+1, r, i-q);
    }
}
```



**Time complexity**

- Best case：**Pivot 的定位恰將資料切為兩等分**
  - $T(n) = T(\frac n2) + cn \\  = c(n + \frac n2 + \frac n4 + \ldots+1) = \Theta(n)$ （對左半或是右半作「selection」+ 「Partition」）



- Avg. case
  - $T(n) = \frac 1n\sum_{s = 0}^{n-1} T(s)+ cn = O(n)$



- Worst case
  - 當 pivot 恰為最大或是最小值
  - $T(n) = T(n-1) + cn\\ = n + (n-1) + (n-2) + \dots + 1 = O(n^2)$
  - **解決：採取「Median of medians」選擇 pivot ，將 worst case 弭平為 O(n)**



#### Selection with median of medians



![1542449916157](\willywangkaa\images\1542449916157.png)



步驟：

1. 先將 n 筆資料分成 $\lceil \frac n5 \rceil$ 個群組，每個群組有**五筆資料**（可能有一群組不足 5 筆資料）
   - 時間複雜度：O(n)



2. 針對每個群組各自排序（Eg. Insertion sort）
   - 時間複雜度：每個群組最多花費 O(25) 次比較，共有 $\lceil \frac n5 \rceil$ 個群組，所以總共需 O(n)



3. 每個已排序群組中第三個資料為該群組之中間值，針對 $\lceil \frac n5 \rceil$ 個群組的中間值，作「Select」，找出這些中間值中的中間值即為**「Median of medians」**
   - 時間複雜度：遞迴呼叫「Select」函式，在中間值中找第 $\frac{\lceil\frac{n}{5}\rceil}{2}$ 小的中間值；需要 $T(\lceil\frac n5\rceil)$



4. 以「Median of medians」作為「Pivot」進行「Partition」
   - 時間複雜度：O(n)



5. 繼續尋找 i 大小的值
   - 時間複雜度：O(s)，取決於「Median of medians」作為「Pivot」將資料切割之程度

```cpp
k = q-p+1;
if (i == k) {
    return A{q];
} else if (i < k) {
    Select(A, p, q-1, i);
} else {
    Select(A, q+1, r, i-k);
}
```





- (下圖)扣除「Pivot」所在的群組以及不滿五筆資料之群組，**約有一半的群中組必有 3 筆資料** ≧ 「Pivot」

  - 比「Pivot」大的資料個數 $(\frac 12 \cdot \lceil \frac n5\rceil -2) \cdot 3 = \lceil\frac {3n}{10} \rceil-6 $ 

  - 相反的會有 $\lceil \frac{7n}{10}\rceil +6$ 筆資料 ≦ 「Pivot」



![1542450245855](\willywangkaa\images\1542450245855.png)



- Time comlexity
  - (下圖)在資料比較多的地方作「Select」以代表最糟糕的情況，所以在 $\lceil \frac{7n}{10}\rceil +6$ 中遞迴找出目標值，所以第五步的遞迴最壞需要 $T(\lceil \frac{7n}{10}\rceil + 6)$
  - $T(n) = T(\lceil\frac n5\rceil) + T(\lceil \frac{7n}{10}\rceil+6)+O(n) \\ = T(\frac 15n)+ T(\frac 7{10}n) + cn = O(n)$ ( 以樹狀結構解決 )



![1542451499380](\willywangkaa\images\1542451499380.png)



> - (下圖)如果以 7 筆資料為一群組
>   - 至少有 $(\frac 12\cdot\lceil\frac n7\rceil -2)\cdot 4$ 筆資料 ≧ 「Pivot」
>   - 即 $\frac{2n}{7}-8$ 筆 ≧ 「Pivot」，則 $\lceil \frac {5n}{7}\rceil+8$ 筆 ≦ 「Pivot」
>   - $T(n) = O(n) + O(n) + T(\lceil\frac n7\rceil) +O(n) + T(\lceil \frac{5n}{7}\rceil+8) \\ = T(\frac n7)+ T(\frac 57n)+cn = O(n)$
>
>
> ![1542450561714](\willywangkaa\images\1542450561714.png)
>
> - (下圖)如果以 3 筆資料為一群組
>   - 至少有 $(\frac 12\cdot\lceil\frac n3\rceil -2)\cdot 2$ 筆資料 ≧ 「Pivot」
>   - 即 $\frac{n}{3}-4$ 筆 ≧ 「Pivot」，則 $\lceil \frac {2n}{3}\rceil+4$ 筆 ≦ 「Pivot」
>   - $T(n) = O(n) + O(n) + T(\lceil\frac n3\rceil) +O(n) + T(\lceil \frac{2n}{3}\rceil+4) \\ = T(\frac n3)+ T(\frac 23n)+cn = O(n\log n)$ 
>
> ![1542450928741](\willywangkaa\images\1542450928741.png)



## Merge sort



適用於「External sort」，所以可以又稱為「External merge sort」；其特性可讀入一些能放在內存內的數據量，在內存中排序後輸出為一個順串（即是內部數據有序的臨時文件），處理完所有的數據後再進行歸併

(見[外排序- 維基百科，自由的百科全書 - Wikipedia](https://zh.wikipedia.org/zh-tw/%E5%A4%96%E6%8E%92%E5%BA%8F))



- 名詞
  - 「Run」：**順串**；排序好的片段資料
  - 「Run」的長度：順串中的資料量



### Iterative merge sort (Two way merge)



Example



![1542088579919](\willywangkaa\images\1542088579919.png)



- Algorithm ( Merge 2 runs )

```cpp
// A[l....m]: 子陣列「順串一」
// A[m+1..n]: 子陣列「順串二」
Merge(A[], l, m, n) {
    int p = l, q = m+1;
    
    int tmp[n-l+1];   // 暫存陣列 [1..(n-l+1)]
    int i = 1;
    
    // 當 run1 與 run2 皆未掃描完
    while(p <= m && q <= n) {
        if(A[p] <= A[q]) {
            tmp[i++] = A[p++];
        } else {
            tmp[i++] = A[q++];
        }
    }
    // run1 未掃描完
    while(p <= m) {
        tmp[i++] = A[p++];
    }
    // run2 未掃描完
    while(q <= n) {
        tmp[i++] = A[q++];
    }
    copy(A[l..n], tmp[1..(n-l+1)]);
}
```

- Time complexity：O( n log n )
  - 「順串一」的長度為 m、「順串二」的長度為 n，合併兩順串：
    - $\left\{\begin{matrix}最少比較次數： & m \;or \;n\\ 最多比較次數 (有一方先掃描完)： & m+n-1\end{matrix}\right.$ 
  - (下圖一) 所以假設整體要排序的資料總量為 n ，合併一次所有的順串需 O(n)
    - n 筆資料作「2-way merge sort」
    - (下圖二) 可以看成一棵「Completed binary search tree」
    - $\because 「Merge」回合數 = 樹高 -1 \\ \Rightarrow 2^{k-1} = n \Rightarrow k = \lceil \lg n\rceil +1 \\ \Rightarrow 「Merge」回合數 = \lceil \lg n\rceil \\ \because 每個回合作「Merge」需\; O(n) \; \\ \therefore 總共的時間複雜度為\; O(n \log n)$



![1542088930143](\willywangkaa\images\1542088930143.png)

圖一

![1542089353231](\willywangkaa\images\1542089353231.png)

圖二

### Recursive merge sort (Two way merge)



採用「Devide and conquer」的技巧



步驟：

1. 一律切割成兩等分之「子串列」( Sublist )

   - O( 1 )
2. 左右子串列各自作「Merge sort」，算出左右之「順串」

   - $2 \times T(\frac n2)$
3. 對左右順串作「Merge」
   - O( n )

> 與「Quicksort」相比，「Mergesort」把時間（O(n)）花在合併的階段，而「Quicksort」則是把時間（O(n)）花在作分割



![1542090860348](\willywangkaa\images\1542090860348.png)



- Algorithm

```cpp
// A[l....m]: 子陣列「順串一」
// A[m+1..n]: 子陣列「順串二」
Merge(A[], l, m, n) {
    int p = l, q = m+1;
    
    int tmp[n-l+1];   // 暫存陣列 [1..(n-l+1)]
    int i = 1;
    
    // 當 run1 與 run2 皆未掃描完
    while(p <= m && q <= n) {
        if(A[p] <= A[q]) {
            tmp[i++] = A[p++];
        } else {
            tmp[i++] = A[q++];
        }
    }
    // run1 未掃描完
    while(p <= m) {
        tmp[i++] = A[p++];
    }
    // run2 未掃描完
    while(q <= n) {
        tmp[i++] = A[q++];
    }
    copy(A[l..n], tmp[1..(n-l+1)]);
}

// 對 A[L]~A[R] 排序
Mergesort(A[], L, R) {
    if(L < R) {
        m = (l+u)/2;
        Mergesort(A, L  , m);
        Mergesort(A, m+1, R);
        Merge(A, L, m, R);
    }
}
```



- Time complexity
  - Best / Worst / Avg.：O( nlogn )
    - $\because T(n) = 2\times T(\frac n2) + cn \Rightarrow T(n) = O(n\log n)$

- Space complexity
  - O( n )
    - 在作「Merge」的時候為了要暫存合併的結果，所以空間占用大小等於資料量（n），空間需求高
- **Stable**
  - 因為在作「Merge」時，`if(A[p]<=A[q])..` **會讓左順串與右順串在有兩個同樣大小的值時，左順串優先進入新的順串之中**



### [輔助結構] Selection tree



如果在「Mergesort」中一次合併多個順串（k 個順串），稱之為「K-way mergesort」



Example

**4-way merge**



![1542091456582](\willywangkaa\images\1542091456582.png)



上圖每次要對 k 個順串作合併時，如果資料總量為 n ，而**每次從 k 個順串中找到最小的值必須花 k-1 次比較**，並且最多要**歷經 n-1 個回合**，**所以作一次「k-way merge」之時間複雜度為 O( n × k )**

> 為了減少比較的次數，所以以此資料結構輔佐，有兩個資料，「Winner tree」與「Loser tree」，通常在實現上，多採用「Loser tree」



#### Winner tree



Example

**8-way merge with winner tree**



![1542091906501](\willywangkaa\images\1542091906501.png)



![1542092026110](\willywangkaa\images\1542092026110.png)



![1542092171261](\willywangkaa\images\1542092171261.png)



![1542092353259](\willywangkaa\images\1542092353259.png)



![1542092514867](\willywangkaa\images\1542092514867.png)

( 重複動作直到「新順串」建立完 )



- Time complexity (假設為「k-way merge」、總資料數為 n)
  - 建立「Winner tree」：O( k )
    - 分別從 k 個順串中複製出**最小值**作為「Winner tree」的葉節點：O( k )
    - 在「Winner tree」中的葉節點，以 k-1 次的比較找出最小值節點作為「根節點」：O( k )
  - 輸出「根節點」至「新順串」中，被輸出的順串之下一筆資料遞補，重複 n-1 回合：O( n×log k )
    - 決定根節點 ( 最小值 )，假設 l 為葉節點數量，則目前的「Winner tree」之 l = k =  8，樹高 h 為 $O(\lceil\log l\rceil+1)$ ，所以決定根節點需要歷經 $O(h-1) \equiv O(\lceil\lg(l)\rceil+1-1)$：$O(\log k)$
    - 輸出根節點：O( 1 )
    - 被輸出之順串下一筆資料遞補：O( 1 )
  - 總體時間複雜度
    - O(k) + O( nlogk ) = O( nlogk )



#### Loser tree



Example

8-way merge with loser tree

![1542093764103](\willywangkaa\images\1542093764103.png)



![1542093996385](\willywangkaa\images\1542093996385.png)

( 執行直到「新順串」建立完成 )



Time complexity (假設為「k-way merge」、總資料數為 n)

- 建立「Loser tree」：O( k )
  - 分別從 k 個順串中複製出**最小值**作為「Loser tree」的葉節點：O( k )
  - 在「Loser tree」中的葉節點，以 k-1 次的比較找出最小值節點作為「根節點」：O( k )
- 輸出「根節點」至「新順串」中，被輸出的順串之下一筆資料遞補，重複 n-1 回合：O( n×log k )
  - 決定根節點 ( 最小值 )，假設 l 為葉節點數量，則目前的「Loser tree」之 l = k =  8，樹高 h 為 $O(\lceil\log l\rceil+1)$ ，所以決定根節點需要歷經 $O(h-1) \equiv O(\lceil\lg(l)\rceil+1-1)$：$O(\log k)$
  - 輸出根節點：O( 1 )
  - 被輸出之順串下一筆資料遞補：O( 1 )
- 總體時間複雜度
  - O(k) + O( nlogk ) = O( nlogk )



> 在執行「External merge sort」時，假設欲排序資料為 n ，其中「K-way merging on m runs」的時候，在使用「Selection tree」的情況下，其時間複雜度為 O( n lg m )
>
> ![1542095449837](\willywangkaa\images\1542095449837.png)
>
> 首先，為了分析假設 m 個順串被 k-way 分成了**兩堆**，由上面的「Selection tree」知道每次作「k-way merge」之時間複雜度為 $O( \frac n2 \times \lg k )$，又因為有兩堆所以總共一次「Merge」需要 O( n lg k )，所以推廣後無論分幾堆作「Merge」之時間複雜度為 O( n lg k )
>
> 因為 $回合數 = 樹高 - 1$ (見下圖)，在「Sorting」時需要執行 $\lceil log_km \rceil$ 回合
>
> ![1542097917874](\willywangkaa\images\1542097917874.png)
>
>
>
> - Time complexity
>   - 由樹來看 Degree = k，Leaf = m $\Rightarrow k^{h-1} = m \\ \Rightarrow h = \lceil\log_k m\rceil +1\\ 回合數 = h -1 = O(\log_k m)$
>   - 總共時間複雜度 $O((n \cdot \lg k) \times \log_k m) \\ = O(n \cdot \frac{\log k}{\log 2}\cdot\frac{\log m}{\log k})\\ = O(n\cdot\lg m)$





## Heap sort



步驟：

1. 先將 n 筆資料建構成「Max heap」
   - Time complexity：O(n)
2. 執行「Del-max」並將取出資料放置於陣列最後端資料被移除處 ( 見「Binary heap」之刪除節點 )，重複動作 n-1 回合



Example

排序：5 3 8 2 6 9 1 4 10 7



![1542105475686](\willywangkaa\images\1542105475686.png)

![1542105714121](\willywangkaa\images\1542105714121.png)

![1542105818821](\willywangkaa\images\1542105818821.png)

![1542105935299](\willywangkaa\images\1542105935299.png)

(重複動作)

![1542106039971](\willywangkaa\images\1542106039971.png)



-  Algorithm

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

// tree[]: 目標排序資料
// n: 排序資料數量
Heapsort(tree[], n) {
    // Build heap
    for(int i = n/2; i>=1; i--) {
        heapify(tree, i, n);
    }
    
    // 執行 n-1 回合之 heap sorting
    for(int i = n-1; i>= 1; i--) {
        swap(tree[1], tree[i+1]);
        heapify(tree, 1, i);
    }
}
```



- Time complexity
  - Best case / Worst case / Avg. case = O(n log n)
    1. Build heap：O( n )
    2. 執行 n-1 回合「Del max」：O ( n log n )
- Space complexity
  - O(1)



Example

排序： 5 5* 1

![1542106445237](\willywangkaa\images\1542106445237.png)

- **Unstable**



# Linear time sorting method



**不採用「Comparsion based」技巧之排序手法**



> 注意！
>
> 在探討線性時間複雜度之排序技術前，要知道這類排序都要有一個前提，**資料值域必須有範圍限制**，才能將排序降低為線性時間
>
> - Example：當排序資料只有 0 與 1 兩種類型資料時，最快的排序方法
>   - **「Counting sort」、「Radix sort」**
> - Example：當資料只有 -1、0、1 三種資料做排序，最快的排序方法
>   - **「Counting sort」、「Radix sort」**



## Radix sort 基數排序法 ( Data structure 書上版本 )



又稱為「Bucket sort」，採取「Distribution and merge」之技巧



- 書本名詞差異

| [DS 版]（Radix = Bucket） | [Algorithm 版]  |
| :-----------------------: | :-------------: |
|      LSD radix sort       | **Radix sort**  |
|      MSD radix sort       | **Bucket sort** |



### LSD radix sort (Least significant digital)



步驟：

1. 令 r 為基底( Base ) ，準備 r 個「Bucket」編號為 0 ~ (r-1)
2. 令 d 為「Input data」**中所有值以 r 為基底之最大位數個數**：O( n )
   - **之後在執行排序時只需要 d 回合即可完成**
3. 由最低位元至最高位元執行：
   1. 分派 ( Distribution )：依各個資料之該位元分派到對應的「Bucket」
   2. 合併 ( Merge )：將所有「Bucket」由小到大（0 ~ (r-1)）合併



Example ( 基底為 10；十進位 )

排序：329、457、657、839、436、720、355

![1542447949542](\willywangkaa\images\1542447949542.png)

- Time complexity
  - 作 d 回合 $\left\{\begin{matrix} 分派：O(n) \\ 合併：O(r) \end{matrix}\right. \\ \Rightarrow 一回合需要\; O(n+r) \\ \Rightarrow 總共時間複雜度：O( d \times (n+r) )$
  - 為「Linear time」：r 可以視為常數 c1 ，而因為值域受到限制，所以 d 為固定值亦為常數 c2
    - O(c2 × (n+c1)) = O( c2 × n + c2 × c1 ) = O( n )
- Space complexity
  - 額外空間需求為「Bucket space」，而有 r 個大小為 n 的「Bucket」：O( r × n )



- Stable






### MSD radix sort (Most significant digital)



步驟：

1. 令 r 為基底( Base ) ，準備 r 個「Bucket」編號為 0 ~ (r-1)
2. 依照「最高位元」的數值分派資料至「Bucket」中
3. 每個「Bucket」內各自排序
4. 由小到大合併所有「Bucket」



> 與「LSD radix sort」最大的區別，「MSD radix sort」只會作「分派」與「合併」各一次，所以若資料位數很大時，或是基底很小時，建議採用「MSD radix sort」





#### Bucket sort ( Algorithm 書上版本 )



步驟：

1. 將資料轉化成「純小數」
2. 以各個純小數之小數後第一位值有序的插入「Bucket」中
3. 由小到大合併所有「Bucket」



Example

排序：179、208、306、93、859、984、55、9、271、33



![1542448961965](\willywangkaa\images\1542448961965.png)



## Counting sort



假設 n 為欲排序資料之總數，且該資料範圍介於 1 ~ k 之間



步驟

1. 統計各個鍵值出現次數，並記錄在 count[1...k] 之中
2. 利用 count[1...k] 求出各個鍵值**未來**排序時之起始位置，紀錄在 start[1...k] 中
3. 依據 start[1...k] 之指示，將「Input array」置入「Output array」中對應的位置



- Algorithm

```cpp
// A: 欲排序陣列
// n: 資料個數
// k: 值域範圍
Countingsort(A[], n, k) {
    new count[1...k];
    new start[1...k];
    new output[1...n];
    
    
    for(int i = 1; i <= k; i++) {         // Time complexity: O(k)
        count[i] = 0;
    }
    // 步驟一：統計鍵值出現個數
    for(int i = 1; i <= n; i++) {         // Time complexity: O(n)
        count[A[i]]++;
    }
    
    // 步驟二：計算每個鍵值排序時之起點位置
    start[1] = 1;
    for(int i = 2; i <= k; i++) {         // Time complexity: O(k)
        start[i] = start[i-1]+count[i-1];
    }
    
    // 步驟三：依照指示放置資料
    for(int i = 1; i <= n; i++) {         // Time complexity: O(n)
        // 先使用 start[A[i]] 原本資料後，
        // 將 start[A[i]]+=1
        output[start[A[i]]++] = A[i];
    }
}
```



- Time complexity
  - O(k) + O(n) +O(k) + O(n) = O(n+k)

> 探討為何 `O(k)+O(n)+O(k)+O(n)` 為線性時間複雜度
>
> - 觀點一
>
> 若鍵值值域之範圍變化是 O(n) 的，注意！此處的 O(n) 為資料範圍，所以也就是說「Input array」中每一個元素在資料範圍中**均勻分布**；以此觀點來看時間複雜度就為 **O(n+k) => O(n+O(n)) => O(n)**
>
> - 觀點二
>
> 若「Input array」之值域受到限制，則 k 可以視為一個常數，所以 O(n+k) = O(n)



- Space complexity
  - `count[1...k]、start[1...k]、output[1...n]`，O(n+k)



- Stable



### Counting sort 問題與探討



延續上面探討時間複雜度之觀點一，已知「Counting sort」之時間複雜度為 O(n+k)，若**資料範圍** k 為線性等級 O(k) ，則整理排序時間複雜度為 O(n)

倘若**資料範圍** k 為平方等級 O($n^2$) （**也就是說「Input array」中的元素在資料範圍超出原本「Counting sort」可執行排序的資料範圍**），則必須探討其時間複雜度之變化 O(n + O($n^2$))



Example ( 用例子解釋 )

有一「Counting sort」只能對值域 0 ~ 9 的資料集合**（O(n)）**作排序，那麼該「Counting sort」要如何對一個值域為 0 ~ 99 **（O(**$n^2$**)）**的資料集合作排序

以**「基數排序法」的想法為基礎**，則可以在兩回合中將排序完成

1. 對每個鍵值作「mod n」（n 為「Countign sort」能夠排序的範圍亦可以想為「基底」）取其作為排序鍵值，對每個資料以新排序鍵值作「Counting sort」，而結果會等同於以「Radix sort」作第一回合後合併之狀態
   - 因為值域介於 0 ~ (n-1) 之間，所以第一回合使用「Counting sort」的排序時間複雜度為 O(n)
2. 以第一回合之「Output array」作為「Input array」，對每個鍵值作「÷ n」再作「mod n」取其作為排序鍵值，並對每個資料以新排序鍵值作「Countint sort」，而結果會等同於以「Radix sort」作第二回合後合併之狀態
   - 若要採用「基數排序法」作為基底，每回合的排序必須是「Stable」才能正確排序，而在這個情況下使用的「Counting sort」亦為「Stable」，所以排序執行得以成功，並且第二回合之時間複雜度一樣為 O(n)

- **總時間複雜度為 2 × O(n) = O(n) ，仍為線性時間複雜度**

> 若資料範圍為 O($n^3$) 則依照上述想法， 三回合即可排序完成
>
> 1. 第一回合的排序鍵值為 $原本鍵值i \;％\; n $ 作為「Counting sort」排序鍵值（時間複雜度為 O(n)）
> 2. 第一回合的排序鍵值為 $\lceil\frac{原本鍵值i}{n}\rceil \;％\; n $ 作為「Counting sort」排序鍵值（時間複雜度為 O(n)）
> 3. 第一回合的排序鍵值為 $\lceil\frac{原本鍵值i}{n^2}\rceil \;％ \;n ​$ 作為「Counting sort」排序鍵值（時間複雜度為 O(n)）
>
> - 當資料範圍為 $O(n^4)、O(n^5)$ 亦可以以此類推



