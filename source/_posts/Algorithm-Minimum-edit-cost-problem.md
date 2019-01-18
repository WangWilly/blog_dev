title: Algorithm - Minimum edit cost problem
author: Willy Wang (willywangkaa)
tags:
  - Dynamic Programming
categories:
  - Algorithm
date: 2019-01-18 16:14:00
---
# Minimum edit cost problem

最常使用在檔案的**比較**以及**更新**上，若在系統上有多個類似的檔案，可以輕鬆比較差異，而若系統中有若干個不同版本的檔案，亦可以藉由此辦法儲存其差異而非整個檔案，如此一來便可以節省其儲存空間。



問題敘述

- 給定兩個字串 A[1...n]、B[1...m]，在下列三種運算之下，使用最小運算成本將字串 A 轉換成字串 B
  1. 插入：在字串中插入一個字元
  2. 刪除：在字串中刪除一個字元
  3. 替換：將字串中的某一個字元轉換成「指定的字元」



- Optimal substructure （以最後一個字元做討論）
  - 假設將 A 轉換為 B 的**最少運算序列為 R[1...k]**，若 A[n] ≠ B[m]，則當 R[k] 為
    1. 「刪除 A 尾端的 A[n]」時，則 R[k-1] 為「A[1...n-1] 轉換為 B[1...m]」的**最少運算序列**
    2. 「 A[n] 中插入一個字元」時，則 R[k-1] 為「A[1...n] 轉換為 B[1...m-1]」的**最少運算序列**
    3. 「將 A[n] 替換成 B[m]」時，則 R[k-1] 為「A[1...n-1] 轉換為 B[1...m-1]」的**最少運算序列**
  - 若 A[n] = B[m]，則 R[k] 為「A[1...n-1] 轉換為 B[1...m-1]」的**最少運算序列**



- Recursion（c[i,j]：A[1..i] 轉換成 B[1..j] 之最大成本）
  - 若 **A[i] ≠ B[j]**
    - $min \left\{\begin{matrix}
      c[i-1, j]+1 & ,刪除\;A\; 尾端的 \;A[i] \\ 
      c[i, j-1]+1 & , 在\;A[1..i]\;尾端插入B[j]  \\ 
      c[i-1, j-1]+1 & ,直接將\; A[i]\; 轉換成 \;B[i]
      \end{matrix}\right.$
  - 若 **A[i] = B[j]**
    - $c[i-1, j-1] \quad , 為\;A[1...i-1] \;與 \;B[1...j-1] \; 的最少運算序列$
  - $\forall i, \quad c[i,0] = i \quad：將\;A[1..i] \;轉換成\; \phi \;的最短運算序列$
    - 等價於 C[i-1,0]+1；遞迴刪除 A[i] 的末端字元
  - $\forall j, \quad c[0,j] = j \quad：將 \;\phi\; 轉換成\; B[1..j] \; 的最短運算子序列$
    - 等價於 C[0,j-1]+1；遞迴在 $\phi$ 末端新增 B[j] 字元



- Algorithm

```cpp
char* MED(char* A, char* B, n, m) {
    for i from 1 to n {
        c[i,0]      =  i;
        label[i,0]  = '↑'; // 等價於 C[i-1,0]+1
    }
    for i from 1 to m {
        c[0,i]      =  i;
        label[0,i]  = '←'; // 等價於 C[0,j-1]+1
    }
    
    for i from 1 to n {
        for j from 1 to m {
            if A[i] != B[j]
                c[i,j] = min(c[i-1,j]  +1, // 刪除 A[i] 的字元
                             c[i,j-1]  +1, // 在 A[1..i] 字串末端插入 B[j] 字元
                             c[i-1,j-1]+1);// 將 A[i] 直接轉換成 B[j] 字元
            else
                c[i,j] = min(c[i-1,j]  +1, // 刪除 A[i] 的字元
                             c[i,j-1]  +1, // 在 A[1..i] 字串末端插入 B[j] 字元
                             c[i-1,j-1]);  // 不做任何運算
            // 依照上面的運算決定 lebel[i,j] 的性質
        }
    }
}
```



Ex （87 年交大資工）

A = acbabca；B = babcbac ，使用最少轉換將字串 A 轉換成 B 字串

- Recursion（c[i,j]：A[1..i] 轉換成 B[1..j] 之最大成本）
  - 若 **A[i] ≠ B[j]**
    - $min \left\{\begin{matrix}
      c[i-1, j]+D[i] & ,刪除\;A\; 尾端的 \;A[i] \\ 
      c[i, j-1]+I[j] & , 在\;A[1..i]\;尾端插入B[j]  \\ 
      c[i-1, j-1]+C[i,j] & ,直接將\; A[i]\; 轉換成 \;B[i]
      \end{matrix}\right.$
  - $\forall i, \quad c[i,0] = \sum_{k =1}^iD[k] \quad：將\;A[1..i] \;轉換成\; \phi \;的最短運算序列$
    - 等價於 c[i-1,0] + D[i]；遞迴刪除 A[i] 的末端字元
  - $\forall j, \quad c[0,j] = \sum_{k=1}^jI[k] \quad：將 \;\phi\; 轉換成\; B[1..j] \; 的最短運算子序列$
    - 等價於 c[0,j-1] + I[j]；遞迴在 $\phi$ 末端新增 B[j] 字元



![1547791902730](\willywangkaa\images\1547791902730.png)



Ex （轉換的成本不一致）

給定兩序列 A[1..3]，B[1..4]，以最少成本將 A 序列轉換成 B 序列

- 轉換成本
  - 刪除 A[i] 字元 D[1..3]
    - $\begin{bmatrix}
      6 & 1 & 2
      \end{bmatrix}$
  - 插入 B[i] 字元 I[1..4]
    - $\begin{bmatrix}
      5 & 3 & 1 & 1
      \end{bmatrix}$
  - 將 A[i] 字元轉換成 B[j] 字元 C[1..3, 1...4]
    - $\begin{bmatrix}
      1 & 2 & 1 & 1\\ 
      2 & 1 & 2 & 2\\ 
      3 & 1 & 2 & 4
      \end{bmatrix}$



![1547794356093](\willywangkaa\images\1547794356093.png)



## DNA comparsion problem

Ex（生物資訊比較問題）

在生物資訊學科裡，會給定兩個 DNA 序列 A[1..n]、B[1..m]，並以下列**「運算/比較 — 權重」**來決定兩個序列之相似度

- 相似度權重
  - A[i] = B[j]：比對直接命中
    - 相似度加 2
  - A[i] ≠ B[j]：比對失敗
    - 相似度減 3
  - **在 A[i] 前插入空字元 ∅ （意旨跳過 B[i] 字元的比較）讓 A[i..n] 與 B[i+1..m] 達到最大相似度**
    - 相似度減 1
  - **將 A[i] 字元刪除（意旨跳過 A[i] 字元的比較）讓 A[i+1..n] 與 B[i..m] 達到最大相似度**
    - 相似度減 1

- Recursion（**w[i,j]：A[1..i] 與 B[1..j] 比對的最大相似度**）
  - $ max\left\{\begin{matrix}
    w[i-1, j-1]+2 & A[i]\;與\;B[j]\; 相等，其剩餘最大相似度等於\;w[i-1, j-1]\\ 
    w[i-1, j-1]-3 & A[i]\;與\;B[j] \;不等，其剩餘最大相似度等於\;w[i-1, j-1] \\ 
    w[i-1, j]-1 & 跳過對 \;A[i]\; 的比較，其剩餘最大相似度等於\;w[i-1, j] \\ 
    w[i, j-1]-1 & 跳過對 \;B[i]\; 的比較，其剩餘最大相似度等於\;w[i, j-1]\end{matrix}\right.$

給定兩個 DNA 序列 A = ACGCTGA；B = AACTGT，使用最少**「運算/比較 — 權重」**以比較 A、B 字串：

![1547798815815](\willywangkaa\images\1547798815815.png)