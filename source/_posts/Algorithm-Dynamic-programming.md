title: Algorithm - Dynamic programming
author: Willy Wang (willywangkaa)
date: 2018-09-23 10:46:21
tags:
---
# Dynamic programming



將已**計算的結果**記錄在**表格中**的技巧，目的是為了要避免**重複計算相同子問題**，以**「Button up」**的方式實踐。



- 為何要使用這種方法，以**費氏數列程式**開始討論。


$$
F_n = \left\{\begin{matrix}
0 & , if \; n = 0\\
1 & , if \; n = 1\\
F_{n-1} + F_{n-2} & , if \; n \geq 2   
\end{matrix}\right.
$$
欲求出 $F_5$ ，以遞迴程式執行，會造成「Overlapping subproblem」，如下圖 $F_2$ 重複計算了 3 次，$F_3$ 重複計算 2 次。




![overlappingsubproblem](\willywangkaa\images\overlappingsubproblem.png)



使用動態表格執行之，可以省去很多不必要的重複計算，如下圖。



![fibonassidp](\willywangkaa\images\fibonassidp.png)



- 構思動態規劃題目的流程
  - Optimal substructure：一個**母問題的最佳解**如何由**其子問題的最佳**解構成。



![observedp](\willywangkaa\images\observedp.png)



## Shortest path problem



- **Optimal substructure**




![shortestpathproblem_subproblem](\willywangkaa\images\shortestpathproblem_subproblem.png)



> (98 交大資工) 問 longest path problem 有無 optimal substructure。
>
>  無，所以不可以使用動態規劃解此問題。
>
> 
![longestpathproblem](\willywangkaa\images\longestpathproblem.png)
>
> **但是**如果此圖加上「Acyclic」、「Direct」等條件限制之後，可以用動態規劃解「longest path problem」。



> 通常要在各種條件之下都可以使用**動態規劃解此問題**，才可以**宣稱此問題可以使用動態規劃解**。



- Ex (99 交大)
  - 下列何者**正確**？
    - （a）Dynamic programming always provides polynominal time algorithm.
    - （b）Huffman coding for compresion is a typical dynamic programming algorithm.
    - （c）Dynamic programming use the table to design the algorithm.
    - （d）Optimal is a important element in the dynamic programming.
    - （e）**Single source all shortest path problem** has optimal substructure.
  - Ans
    - （a）：false。反例為 Subset-sum problem (暴力法：$O(2^n)、動態規劃法：$$O(n2^{\frac n2})$)
    - （b）：false。為典型的 Greedy algorithm。
    - **（c）**
    - **（d）**
    - **（e）**



## Knapsack problem



### Fractional knapsack problem



> 1. 此方法僅限於物品重量為正整數時
> 2. 0/1 KP 為 NP - Completed 問題



- Input：
  - n 個物件
    - 第 i 個重量為 $w_i$ ，價值為 $v_i$。
  - 背包最大負重
    - $W$
- Output：
  - 最大的獲利值
- 限制條件：
  - 取得物品的總重量 $\leq$ W
  - 可取物品的**部分**



- 想法
  - **Greedy**：從目前 $\frac{v_i}{w_i}$ 最高的物品開始拿取，直到物品取完，或是取得物品負重已達 W。
- Time Complexity
  - $\Theta(n\log n)$
- Ex ( W = 5 )



| Item | $v_i$ | $w_i$ |
| ---- | ----- | ----- |
| 1    | 10    | 2     |
| 2    | 6     | 1     |
| 3    | 12    | 3     |

$$
\frac{v_2}{w_2} = 6 \geq \frac{v_1}{w_1} = 5 \geq \frac{v_3}{w_3} = 4
$$



1. 取物品 2，取該物品之 1 單位重量，背包剩餘空間 4 單位重量，獲利 6 單位價值。
2. 取物品 1，取物品之 2 單位重量，背包剩餘空間 2 單位重量，獲利 16 單位價值。
3. 取物品 3，取物品之 2 單位重量，背包剩餘空間 0 單位重量，**獲利 24 單位價值**。



![kpdp](\willywangkaa\images\kpdp.png)



|      | 0    | 1    | 2    | 3    | 4    | 5      |
| ---- | ---- | ---- | ---- | ---- | ---- | ------ |
| 0    | 0    | 0    | 0    | 0    | 0    | 0      |
| 1    | 0    | 0    | 10   | 10   | 10   | 10     |
| 2    | 0    | 6    | 10   | 16   | 16   | 16     |
| 3    | 0    | 6    | 10   | 16   | 18   | **22** |



- Ex ( 98 交大 ) P.3-66 ex10
  - Maximize $\sum_{i = 1}^n v_ix_i \;  subject\; to \sum_{i = 1}^n w_ix_i \leq W, 0 \leq x_i \leq 1$ 
  - Greedy choise property ( $\frac{v_1}{w_1} \geq \frac{v_2}{w_2} \geq\ldots$ )
  - 最佳解的 $x_1$ 需取多少。


$$
x_n = \left\{\begin{matrix}
1 & , if \; w_1 \leq W \\
\frac{W}{w_1} & , if \; w1 > W \\   
\end{matrix}\right.
$$




### 0-1 Knapsack problem



- Input：
  - n 個物件
    - 第 i 個重量為 $w_i$ ，價值為 $v_i$。
  - 背包最大負重
    - $W$
- Output：

  - 最大的獲利值
- 限制條件：
  - 取得物品的總重量 $\leq$ W
  - 只能取物品的**整體**

- 想法

  - 無法使用 Greedy method 解決

  - **使用動態規劃解決，物品的重量必為正整數**

    - 遞迴結構：(令 $C[i][k]$ 在負重 k 之下考慮物品 1 ... i 之最大獲利

    $$
    C[i][k] = \left\{\begin{matrix}
    0 & , if \;i = 0\;OR\; k = 0 \\
    max(C[i-1][k-w_i]+v_i, C[i-1][k] )& , if \; w_i \leq k \\
    C[i-1][k] & , if \; w_i > k \\
    \end{matrix}\right.
    $$











- Algorithm ( Bottom up )

```
for k <- 0 to w
	c[0, k] <- 0
for i <- 1 to n
{
    c[i, 0] <- 0
    for k <- 1 to w
    {
        if k < w_i
        	c[i, k] <- c[i-1, k]
        else
        	c[i, k] <- max(c[i-1, k], c[i-1, k-w_i]+v_i)
    }
}
```



- Time complexity
  - $\Theta( nW )$  **Pseudo-polynominal**
- Space complexity
  - $\Theta( nW )$ 



### Branch and bound 解 0-1 Knapsack problem



> - 對於一個 NP-Completed 問題來說，可以使用「Branch and bound」來解決
> - 「Branch and bound」演算法中，**「Bounding function」**的設計會是影響整體效能最大的關鍵 ( 收斂快慢 )。
> - Time complexity：O( 葉節點個數 )
>   - 葉節點個數取決問題本身。
>     - 組合性問題：$2^n$ 
>     - 排列性問題：$n!$ 



- 將球最佳解的過程視為在一個「State space tree」中尋找最好的節點。

- 實務上，通常是設計一 個**「Bounding function」**以估計目前狀態可到最佳解的可能性。
  - 建構「State space tree」時，每次都先**展開「Bounding function」的節點。( Branch )**
  - 在每次展開的過程中，都可以得到一個**目前最佳解 (葉節點)，接著，之後不展開「Bounding function 值** $\leq$ **目前最佳解」**的內部節點。**( Bound )**



- 以 Branch and bound 解 0-1 Knapsack problem
  - 每個節點須紀錄
    - **目前的獲利**
    - **目前的負重**
    - **「Bounding function」算出的值**



1. 為了計算「Bounding funciton」括號中要估計「Fractional knapsack」的未來最大值，需要將物品依照 $\frac{v_i}{w_i}$ 排序。

$w  = 4$ 



| Item | $v_i$ | $w_i$ |
| ---- | ----- | ----- |
| 1    | 6     | 1     |
| 2    | 10    | 2     |
| 3    | 12    | 3     |



$$
\frac{v_2}{w_2} = 6 \geq \frac{v_1}{w_1} = 5 \geq \frac{v_3}{w_3} = 4
$$

2. **設計「Bounding function」。(括號部分就是用來估計以目前節點拓展，獲利的上限)**


$$
Bounding \;funciotn(目前的節點)\\
= 在「目前的節點」上可得的獲利 + \\
(將背包剩餘的重量以「Fractional \;knapsack \;problem」拿取剩下的物品的獲利)
$$




![KPstatesearchtree](\willywangkaa\images\KPstatesearchtree.png)



3. 拓展節點
   1. 展開 **Root**
   2. 因為該節點「Bounding function」最大，所以展開 **A**
   3. 展開節點 **C**
   4. **E** 節點因為超重所以為「Infeasiable solution」
   5. **F** 為一可能解，使 $Max = 16$
   6. 因為節點 **D** 在樹中較深處，先展開之
   7. **G** 與 **H** 均為一解，設 $Max = 18$ 
   8. 因為節點 **B** 的「Bounding function」$\leq$ **Max**，不展開該節點
4. Ans：18 ( 取**物品一**與**物品三** )



## Longest Common Subsequence



- Sequence
  - X = ＜a, b, c, a＞
- Subsequence
  - ＜a, c＞ 為 X 的「Subsequence」
- Prefix ( 前綴 )
  - $X_3 = ＜a, b, c＞$ 
- Common subsequence
  - Y = ＜a, c, b, c＞
  - 則＜a, c＞為 X 與 Y 的「Common subsequence」
- Longest common subsequence
  - ＜a, b, c＞ 為 X 與 Y 的「LCS」



> 「LCS」不一定唯一



- 遞迴結構
  - 令 $c[i , j]$ 為 $LCS(X_i, Y_j)$ 的長，則：

$$
c[i, j] = \left\{\begin{matrix}
0 & , if \;i = 0\;OR\; j = 0 & \\
c[i-1, j-1] + 1 & , if \; X[i] = Y[j] & \\
max(c[i-1, j], c[i, j-1])& , if \; X[i] \ne Y[j] \; & （該兩個字絕對不會同時出現在LCS）
\end{matrix}\right.
$$

- 演算法 ( Bottom-up )

```
for j <- 0 to n
	c[0, j] <- 0
for i <- 0 to m
	c[i, 0] <- 0
for i <- 1 to m
	for j <- 1 to n
	{
        if(X[i] = Y[i])
        	c[i, j] = c[i-1, j-1] + 1
        else
        	c[i, j] = max(c[i-1, j], c[i, j-1])
	}
```



- Time complexity
  - $\Theta(mn)$
- Space complexity
  - $\Theta(mn)$ 



- Ex
  - X = ＜a, b, a, c＞
  - Y = ＜a, b, c, a＞
  - 求「LCS」



|    -    |   -   |   -   |  "a"   |  "b"   |  "a"   |  "c"   |
| :-----: | :---: | :---: | :----: | :----: | :----: | :----: |
|  **-**  | **-** | **0** | **1**  | **2**  | **3**  | **4**  |
|  **-**  | **0** |   0   |   0    |   0    |   0    |   0    |
| **"a"** | **1** |   0   | 1（↖） | 1（←） | 1（↖） | 1（←） |
| **"b"** | **2** |   0   | 1（↑） | 2（↖） | 2（←） | 2（←） |
| **"c"** | **3** |   0   | 1（↑） | 2（↑） | 2（←） | 3（↖） |
| **"a"** | **4** |   0   | 1（↖） | 2（↑） | 3（↖） | 3（←） |



### Longest Increasing Subsequence



- Ex
  - X = ＜5, 1, 3, 2, 4＞
  - LIS(X) = ＜1, 2, 4＞
- 演算法
  1. Y <- sort(X)
  2. LCS(X, Y)

- Time complexity
  - $\Theta(n^2)$
    - 排序：$\Theta(n\lg n)$
    - LCS：$\Theta(n^2)$ 



### Longest Common Substring



- Ex
  - X = ＜a, b, a, c＞
  - Y = ＜a, b, c, a＞
  - **Output：＜a, b＞**

$$
c[i, j] = \left\{\begin{matrix}
0 & , if \;i = 0\;OR\; j = 0 & \\
c[i-1, j-1] + 1 & , if \; X[i] = Y[j] & \\
0& , if \; X[i] \ne Y[j] \; & （該兩個字絕對不會同時出現在LCS）
\end{matrix}\right.
$$



- Algorithm

```
// X[1...n]
// Y[1...m]
lcstring = {}
length = 0
// initialize
for i <- 0 to n
    c[0, i] <- 0
for i <- 0 to m
    c[i, 0] <- 0

for i <- 1 to m
    for j <- 1 to n {
        if(X[i] != Y[i])
            c[i, j] = 0
        else {
            c[i, j] = c[i-1, j-1] + 1
            if(length < c[i, j]) {
                length = c[i, j]
                lcstring = X[(i-length+1)...i]
            }
        }
    }
```





## Matrix Chain Multiplication



- Input：
  - **n** 個矩陣的**大小 P[0 ... n]** ( 其中$A_i$ 的大小為 $P_{i-1}\times P_i$ )
- Output：
  - 算出 $A_1 \times A_2 \times \ldots \times A_n$ 所需最少的**純量乘法數**



> n 個矩陣的「Matrix chain」有 $C_{n-1} = \frac{1}{(n-1)+1}\binom{2(n-1)}{(n-1)}$ 相異種乘法可能，所以列出所有乘法順序需要**指數時間**。 



-  Ex. 給定三個矩陣的大小如下
   -  $A_1：10 \times 100$
   -  $A_2：100 \times 5$
   -  $A_3：5 \times 50$
   -  求算出 $A_1 \times A_2 \times A_3$ 所需最少的純量乘法數。



> 若每一個矩陣均為相同大小的「方陣」，改變乘法的順序無法影響所需的純量乘法數，只能使用**「Strassen's algorithm」以加速。**



- 遞迴結構
  - 令 m[i, j] 為算出 $A_i \times \ldots \times A_j$ 所需最少乘法數


$$
m[i, j] = \left\{\begin{matrix}
0 & , if \;i \geq j\\
MIN_{i\leq k \leq j-1}(m[i, k] + m[k+1, j] + P_i\times P_k \times P_j) & , if \; i < j
\end{matrix}\right.
$$


- 演算法

```
for i <- 1 to n (「Matrix chain」長度為一)
	m[i, i] <- 0
for l <- 2 to n (「Matrix chain」長度為二以上)
	for i <- 1 to n-l+1 (起點)
		{
            j <- i+l-1 (終點)
            m[i, j] <- infinity
            for k <- i to j-1
            {
            	tmp <- m[i, k] + m[k+1, j] + P_i-1 * P_k * P_j
                if tmp < m[i, j]
                	m[i, j] <- tmp  (純量乘法數)
                	s[i, j] <- k    (切點)
            }
		}
```



- Time complexity
  - $\Theta(n^3)$：$\sum_{l=2}^n \sum_{i = 1}^{n-l+1}\sum_{k = i}^{i+l-2} 1$ 
- Space complexity
  - $\Theta(n^2)$



- Ex
  - $A_1：3 \times 3$
  - $A_2：3 \times 7$ 
  - $A_3：7 \times 2$
  - $A_4：2 \times 9$
  - $A_5：9 \times 4$
  - 算出 $A_1 \times A_2 \times \ldots \times A_5$ 最少的乘法數。




![matrixchaindp](\willywangkaa\images\matrixchaindp.png)





![martixchainproblem_2](\willywangkaa\images\martixchainproblem_2.png)





| m (乘法數) | 1    | 2       | 3       | 4        | 5            |
| ---------- | ---- | ------- | ------- | -------- | ------------ |
| 1          | 0    | 63（←） | 60（←） | 114（↓） | **156**（↓） |
| 2          |      | 0       | 42（←） | 96（↓）  | 138（↓）     |
| 3          |      |         | 0       | 126（←） | 128（←）     |
| 4          |      |         |         | 0        | 72（←）      |
| 5          |      |         |         |          | 0            |



| s (切點) | 2    | 3    | 4    | 5    |
| -------- | ---- | ---- | ---- | ---- |
| 1        | 1    | 1    | 3    | 3    |
| 2        |      | 2    | 3    | 3    |
| 3        |      |      | 3    | 3    |
| 4        |      |      |      | 4    |



最少乘法數：156

最佳乘法順序：$(A_1\times (A_2 \times A_3)) \times (A_4 \times A_5)$



# 補充例題



Example（107交通大學資料結構與演算法）

- We define the maximum subarray of an array A to be the nonempty, contiguous subarray of A whose value have the largest sum
- Fill in the blank (a), (b) in the following c++ function so that it returns value are placed in A[1], A[2], …, A[n-1]

```cpp
int maxSubarray(int A[], int n) {
    for(int i = 1; i<n; ++i) {
        A[i] += A[i-1];
    }
    int ans = A[0];
    int k = 0;
    
    for(int i = 0; i<n; ++i) {
        ans = max(ans,  (a)  );
        k = min(k,      (b)  );
    }
    return ans;
}
```

> 考慮一個「Maximum subarray」為 A[x…y]
>
> - A[x…y] = A[1…y] - A[1…x]
> - 若欲使 A[x…y] 最大化
>   - A[1…y] 必為最大
>   - A[1…x] 必為最小
>
> (a)：`A[i]-k`
>
> (b)：`A[i]`