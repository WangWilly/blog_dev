title: Algorithm - Pseudo polynomial time complexity
author: Willy Wang (willywangkaa)
tags:
  - Time complexity
categories:
  - Algorithm
date: 2019-01-26 20:28:00
---
# Pseudo polynomial time complexity

如果一個演算法的**傳統時間複雜度**位於多項式時間，而**標準時間複雜度**不在多項式時間

則我們稱這個演算法位於**偽多項式時間**，以下探討



> **多項式時間複雜度是甚麼？**
>
> $O(n^k), k \;is\; constant$
>
> - Selection sort
>   - $O(n^2)$，為「Polynomial time」
> - Travel salesman problem
>   - Brutal force：$O(n\cdot n!)$，為「Non polynomial time」

通常認為輸入變數 n 代表**數據規模**

- Selection sort
  - n 代表欲排序的數據組元素個數
- Travel salesman problem
  - n 表示圖中節點的數量

但是上述的數據規模，為直觀的定義**並不夠嚴謹**，所以需要**標準化輸入的數據規模**

- 輸入的數據規模
  - 保存輸入數據**所需的 Bit 位數**
- 針對**排序**的輸入
  - n 個「32-bit 整數」數組
  - 其數據規模為 32×n，n 為數組中的元素個數
- 針對**圖論**的輸入
  - n 個點、m 個邊
  - 其數據規模為 $\Omega(n+m)$

> 「Polynomial time complexity」的標準定義：
>
> - 在輸入**數據規模為 x** 的清況下，存在一個演算法能在 $O(x^k)$ （k 為一**常數**）的時間複雜度能解決問題 
>
> 所以，再來討論剛才的時間複雜度
>
> - Selection sort
>   - 輸入
>     - n 個「32-bit 整數」數組
>     - 其數據規模為 32×n，n 為數組中的元素個數
>   - 時間複雜度
>     - $x = 32n$
>     - 傳統的時間複雜度為 $O(n^2)$
>     - 則 $n = \frac{1}{32}x \Rightarrow O(\frac{1}{1024}x^2) = O(x^2)$，為「Polynomial time complexity」
> - Depth first search
>   - 輸入
>     - n 個點、m 個邊
>     - 其數據規模為 $\Omega(n+m)$
>   - 時間複雜度
>     - $x = \Omega(n+m) $
>     - 傳統的時間複雜度為 $O(n+m)$
>     - 則 $O(x)$，為「Polynomial time complexity」

接著，對「質數演算法」進行討論：

```cpp
bool isPrime(n) {
    for(int i = 2; i < n-1; i++) {
        if(n%i = 0)
            return false;
    }
    return true;
}
```

- 質數演算法
  - 輸入
    - 這種輸入規模會隨著 n 的大小而改變
      - $x = \lg n$
    - 與排序的規模相比排序的 n 為元素多寡，但是每個元素皆為 32-bit 或是其他**固定大小**的整數範圍
      - （排序的輸入規模）$x = c \cdot n$，**c 必為一常數**
  - 時間複雜度
    - $x = \lg n$
    - 傳統的時間複雜度為 $O(n)$
    - 則 $n = 2^x \Rightarrow O(2^x)$ ，為「Non-polynomial time complexity」

> 進一步體會
>
> - 一個二進位數（**1010 1011**）
>   - 其「質數演算法」的**真實複雜度**為 $2^8$
> - 另一個二進位數（**1 1010 1011**）
>   - 其「質數演算法」的**真實複雜度**為 $2^9$
>
> 可以發現僅增加一個 bit 就會使的演算法的運行時間倍增