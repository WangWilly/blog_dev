title: Data structure - Theoretical evaluation of overflow techniques
author: Willy Wang (willywangkaa)
tags:
  - Hash
categories:
  - Test
date: 2019-02-02 20:18:00
---
# Data structure - Theoretical evaluation of overflow techniques（未完成）



## The basic of hashing

資料儲存機制，當資料 x 要在此結構存取時，需經過「Hashing function」求出「Hashing address」（H(x)），再以「Hashing address」視為此資料在此結構的定位點



- 「Hash table」
  - 「Bucket」
    - 定位點
  - 「Slot」
    - 每個定位點中有多少存儲空間
  - 「Hash table size」
    - 「Bucket」×「Slot」

- 「Identifier」：識別字；資料區分的編碼
  - 「Identifier density」
    - T：「Identifier」總數
    - n：「Identifier」**目前正在使用中（目前在「Hash table」中被保存）**的數量
    - B×S：「Hash table」大小
    - 「Identifier density」 = $\frac{n}{T}$
  - 「Loading density」
    - T：「Identifier」總數
    - n：「Identifier」**目前正在使用中（目前在「Hash table」中被保存）**的數量
    - B×S：「Hash table」大小
    - 「Loading density」 = $\frac{n}{b\times s}$ （通常以 $\alpha$ 表示）

- **「Collision」**
  - 出現兩個不同的資料 X、Y，**經過「Hashing function」計算後，得到相同的「Hashing address」**
- **「Overflow」**
  - 當「Collision」發生並**且該「Bucket」無「Slot」可儲存**

> 發生「Collision」不一定會造成「Overflow」，但是當每個「Bucket」由一個「Slot」所組成，則發生「Collision」等價於「Overflow」



### Analysis of linear probing

又稱為「Open addressing mode」，當 H(x) 發生「Overflow」，則探測 H(x)+1、H(x)+2 …H(x)-1，直到找到尚有空間之「Bucket」或是「Hash table」額滿為止

> 缺點
>
> - 容易形成群聚（Clustering）現象，這將會增加未來搜尋資料的探測次數
> - 插入與尋找的探測成本取決於「群聚鍊大小」
> - 平均的「群聚鍊大小」為 $\frac{n}{m}$
>   - **識別字容易雜湊到群長聚鍊較長的位置**
>   - **最差的狀態就是所有識別字都雜湊到同一個群聚鍊**

- 每次探測
  - 遇到碰撞的機率：$\alpha$ （Loading factor）
  - 遇到可用空間的機率：$1-\alpha$
- 探測過程中
  - **恰兩次**探測找到可用空間的機率
    - $\alpha \times(1-\alpha)$
  - **恰 K 次**探測找到可用空間的機率
    - $\alpha^{k-1}(1-\alpha)$

> n：「Identifier」**目前正在使用中（目前在「Hash table」中被保存）**的數量
>
> m：可使用空間量
>
> - 假設一個「Hash table」T，則 T[0]（Bucket）為空的機率
>   - 減去「被占用的機率」
>   - $1 - \frac nm$
>   - 此機率亦為任何一個「Bucket」為空的機率
> - 假設 T[0]、T[k+1] 皆為空，且 T[1]…T[k] 皆被占用，則機率
>   - 恰 k 個識別字被雜湊到 T[0]…T[k]
>     - T[0] 在假設中仍為空
>     - 機率 $ = \binom{使用中的識別字}{k \;個識別字}\times(被占用的機率)^k\times(未使用的機率) \\  = \binom{n}{k}(\frac {k+1}m)^k(1-\frac k{k+1})$
>   - 恰 n-k 個識別字會被雜湊到 T[k+1]…T[m-1]
>     - T[k+1] 在假設中仍為空
>     - 機率 $ = (被占用的機率)^{n-k}\times(未使用的機率)$

- 探測失敗
  - $\frac 12 (1+\frac 1{(1-\alpha)^2}) = \frac 12(1+2\alpha+3\alpha^2+ 4\alpha^3+\ldots)$
- 搜尋成功
  - $\frac 12(1+\frac 1{1-\alpha}) = 1+\frac 12(\alpha+\alpha^2+\alpha^3+\alpha^4+\ldots)$

### Analysis of quadratic probing

> 「Uniform probing」模型
>
> 由 W.W.Peterson 於1957年提出
>
> - 假設所有的「Identifier」（Key）以非常平均的方式分布於「Hashing table」
>   - 假設「Hashing table」的大小為 m
>   - 「Identifier」占用的大小為 n
>   - 所以有 $\binom{m}{n}$ 種可能的佔用方式
> - **忽略「Primary clustering」與「Secondary clustering」的影響**
>   - 初次探測（First prob）與碰撞（Collision）之後的探測皆假設為**「隨機探測」**
>   - 每次的探測各自為**獨立事件（Independent event）**

  - 每次探測
    - 遇到碰撞的機率：$\alpha$ （Loading factor）
    - 遇到可用空間的機率：$1-\alpha$
  - 探測過程中
      - **恰兩次**探測找到可用空間的機率
          - $\alpha \times(1-\alpha)$
        - **恰 K 次**探測找到可用空間的機率
      - $\alpha^{k-1}(1-\alpha)$
- **探測失敗**（$U(\alpha)$）
  - **觀點一**
    - 探測失敗的機率等價於自「恰 1 次探測失敗的機率」累加至「**恰無限次探測失敗的機率**」
    - $U(\alpha) = \sum_{K = 1}^\infty \binom K1\alpha^{K-1}(1-\alpha) = \sum_{K = 1}^\infty K\cdot\alpha^{K-1}(1-\alpha)$
    - 利用生成函數 $\Rightarrow (1-\alpha)(\sum_{K = 1}^\infty K\cdot \alpha^{K-1}) = (1-\alpha)\frac{1}{(1-\alpha)^2} = \frac{1}{1-\alpha}$
  - **觀點二**
    - 累加所有「**探測失敗的機率**」
    - $U(\alpha) = 1 + \alpha + \alpha^2 + \alpha^3 + \ldots = \frac{1}{1-\alpha}$
- **探測成功**（$S(\alpha)$）
  - **插入第 k+1 個資料最多需要探測** $\frac{1}{1-\frac{j}{m}}$
  - 插入 n 筆資料探測成功的**平均次數**為
    - $S(\alpha) = \frac{1}{n}\sum_{k = 0}^{n-1}\frac{1}{1-\frac km} = \frac{m}{n}\sum_{k = 0}^{n-1}\frac{1}{m-k}\\ \Rightarrow \frac{1}{\alpha}(\sum_{k = 1}^m\frac 1k - \sum_{k = 1}^{m-n}\frac 1k)\\ \Rightarrow \frac 1\alpha (H_m - H_{m-n}) \\ \leq \frac{1}{\alpha}(\ln m - \ln(m-n)) \\ = \frac1\alpha (\ln\frac{m}{m-n}) = \frac 1\alpha(\ln\frac1{1-\alpha})$



### Analysis of chaining



- 每個串列
  - 預期大小：$\alpha$ （Loading factor）
- 探測失敗（$U(\alpha)$）
  - 等價於預期串列大小
  - $U(\alpha) = \alpha$
- 探測成功
  - 當第 n 個識別字要插入到「Hash table」時
    - 每個串列的預期大小為 $\frac{n-1}{m}$
  - 



# 補充試題



Example（105 交通大學資料結構與演算法）

Given a hash table of size m, please answer following questions.

- Under the uniform hashing assumption, if we use the hash table with open addressing to hash 3 keys, the probability that the third inserted key needs exactly three probes before being inserted into the table ts exactly
  - （A）$\frac 2m$
  - （B）$\frac 2{m-1}$
  - **（C）**$\frac 2{m(m-1)}$
  - （D）$\frac 1{m-1}$
  - （E）$\frac 1{m(m-1)}$

> - 第三個鍵值插入時
>   - 碰撞到兩個已插入的機率： $\frac 2{m}$
>   - 假設不會再撞到第一次碰撞的值，作探測後再次碰撞的機率： $\frac 1{m-1}$

- We use the hash table with open addressing to hash n keys, where n is less than m. Under the uniform hashing assumption, the expected cost to insert another element into the table is at most
  - （A）$1+\frac nm$
  - **（B）**$\frac 1{1-\frac nm}$
  - （C）$\frac nm$
  - （D）$\frac 1{\frac nm}$
  - （E）$1-\frac nm$



- After hashing n keys into the hash table that uses chaining to handle collisions, we hash two new keys $k_1$ and $k_2$. Under the simple uniform hashing assumption, the probability that $k_1$ and $k_2$ are hashed into the same table location is exactly
  - （A）$\frac 1n​$
  - **（B）**$\frac 1m$
  - （C）$\frac 1{nm}$
  - （D）$\frac nm$
  - （E）$\frac mn$