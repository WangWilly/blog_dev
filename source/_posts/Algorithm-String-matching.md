title: Algorithm - String matching
author: Willy Wang (willywangkaa)
tags:
  - String matching
categories:
  - Algorithm
date: 2019-02-07 20:25:00
---
# Algorithm - String matching

給兩個字串 T 和 P，找出 T 當中是否有一段字串正好是 P，並且找出其位置

> 字串搜尋當中，通常將兩字串的象徵符號取做 T 和 P
>
> - T 意指 Text
> - P 意指 Pattern
>
> 可以想作是從長篇文字 T 之中搜索小段文字 P

- 若 P 在平移 s 個單位後（**T [s+1 … s+m] = P [1 … m]**，0 ≦ s ≦ n-m）
  - 可以在 T 中該片段被找到
    - 稱為「合法平移」（Valid shift）
  - 無法在 T 中該片段被找到
    - 稱為「非法平移」（Invalid shift）

![1549517694185](\willywangkaa\images\1549517694185.png)



| Algorithm              | Preprocessing time | Matching time        |
| ---------------------- | ------------------ | -------------------- |
| Native                 | 0                  | O( (∣T∣-∣P∣+1) ×∣P∣) |
| Rabin-Karp             | θ(∣P∣)           | O( (∣T∣-∣P∣+1) ×∣P∣) |
| Morris-Pratt Automaton | O(∣P∣×∣Σ∣)     | θ(∣T∣)               |
| Knuth-Morris-Pratt     | θ(∣P∣)           | θ(∣T∣)               |



## Naive string matching（窮舉法）

最直覺的算法



1. 挪動 P 以對準 T 的各個位置
2. 逐一比對字元、判斷是否相等



Example

![1549518031466](\willywangkaa\images\1549518031466.png)

![1549518053082](\willywangkaa\images\1549518053082.png)

![1549518063746](\willywangkaa\images\1549518063746.png)

![1549518076863](\willywangkaa\images\1549518076863.png)



- Algorithm

```cpp
void Nativestrinmatcher(string T,string P) {
    n = T.length();
    m = P.length();
    for s = 0 to n-m {
        if(P[1..m] == T[s+1..s+m])
            print("Pattern occurs with shift"+ s);
    }
}
```



- 時間複雜度
  - O( (|T|-|P|+1)×|P| )
    - **O( |T||P| )** 

- 空間複雜度
  - O(1)



## Rabin-Karp 演算法

由 Michael O. Rabin 及 Richard M. Karp 在 1987 年發展利用**雜湊作字串判斷**，帶有數學味道的演算法

- 一個長度為 m 的模板字串 P
  - 視為一個 d 進制（d = ∣Σ∣）的數字 p
  - 令 $t_s$ 為 T[s+1…s+m] 轉換後的結果
- 問題轉換成「是否存在一個 k(0 ≤ k ≤ n − m)，使得 $p = t_k$」

> p =  ∣Σ∣(P[m-1]+(∣Σ∣(P[m-2]+… (∣Σ∣(P[2]+∣Σ∣P[1]) …))) + P[m]
>
> $\Rightarrow p = ｜Σ｜^{m-1}P[1]+｜Σ｜^{m-2}P[2]+\ldots+｜Σ｜^{0}P[m]$
>
> $t_{s+1} = ｜Σ｜(t_s - ｜Σ｜^{m-1}T[s+1])+T[s+m+1]$



Example

Σ = ｛0, 1, …, 9｝，d = ∣Σ∣ = 10

- Pattern P[1…m]
  - p 為對應的十進位數字
- Text T[1…n]
  - $t_s$ 為**長度為 m 子字串**（T[s+1…s+m]，s = 0, 1, …, n-m）中對應的十進位數字
- 字串 ［3 1 4 1 5 2］
  - 轉換後為 314,152

> $t_{s+1}$ 與 $t_s$ 的關係
>
> $t_{s+1} = 10(t_s - 10^{m-1}T[s+1])+T[s+m+1]$
>
> 所以，上述表示為
>
> T = ［3 1 4 1 5 2］、m = 5、d = 10
>
> $t_s = t_0 = 31,415$
>
> $\Rightarrow t_{s+1} = t_1 = 10(31,415-10^{5-1}\cdot3)+2 = 14,152$



> - 因為 p 以及 $t_k$ **可能非常大**
>   - 因此比較時間不能視為常數
> - 通常將其 mod 一個大質數 q
>   - 因為如此當 $p = t_k$ 時，不一定匹配成功，須再作進一步驗證
>     - Spurious hit（假性命中）
>       - $p = t_k$ 但 P[1…m] **≠** T[s+1…s+m]
>     - Valid hit（完全命中）
>       - $p = t_k$ 與 P[1…m] **=** T[s+1…s+m]



Example

- P[1…5] = 31,415
  - p = **31,415 mod 13** = 7

![1549521217587](\willywangkaa\images\1549521217587.png)

- Algorithm

```cpp
void Rabinkarpmatcher (T,P,d,q) {
    n = T.length();
    m = T.length();
    h = pow(d,m-1) % q;
    t[0] = 0;
    p = 0;
    
    // Preprocessing
    for i = 1 to m {
        p = (d*p + P[i]) % q;
        t[0] = (d*t_0 + T[i]) % q;
    }
    
    // Matching
    for s = 0 to n-m {
        if(p == t[s]){
            if(P[1..m] == T[s+1..s+m]){
                print("Pattern occurs with shift"+ s);
            }
        }
        // Next substring
        if(s < n-m){
            t[s+1] = (d*(t[s]-T[s+1]*h)+T[s+m+1]) % q;
        }
    }
}
```



- 時間複雜度
  - 預處理
    - θ (∣P∣)
  - 比對程序
    - **O( (∣T∣-∣P∣+1)×∣P∣ )**
    - 發生在「Worst case」情況
    - 在多數比對次數少、q 大於 m 的情況為線性複雜度

- 空間複雜度
  - O(1)



## Knuth-Morris-Pratt 演算法

由 Donald Knuth、Vaughan Pratt、J. H. Morris 三人於西元 1977 年共同聯合發表，**最差情況為 O(n) 的字串匹配演算法**



> **觀察暴力演算法**
>
> - 存在不必要的工作
>   - 從左往右一一比對字元，一旦發現字元不同，將 P 往右挪動一位
>   - 往右挪動 P 之前，當下比對成功的字串片段，可以不必花時間在上面
>
> Example
>
> - T = [aabzabzabcz]
> - P = [abzabc]
>
> （從左往右一一比對字元，一旦發現字元不同，將 P 往右挪動一位）
>
> ![1549522985570](\willywangkaa\images\1549522985570.png)
>
> （在往右挪動 P 之前，當下比對成功的字串片段「abzab」可以加以利用）
>
> ![1549523718157](\willywangkaa\images\1549523718157.png)
>
> （繼續往右挪動 P，挪動一個位置、挪動兩個位置、…）
>
> ![1549523787809](\willywangkaa\images\1549523787809.png)
>
> 觀察上述行為
>
> - 挪動一個位置
>   - 比較『abzab 的**後四個字元**』與『abzab 的**前四個字元**』
> - 挪動兩個位置
>   - 比較『abzab 的**後三個字元**』與『abzab 的**前三個字元**』
>
> - 因此若預先知道『 abzab 之「次長相同前綴後綴」是 ab』
>   - **可大幅挪動 P**
>     - 從「V」處繼續向右一一比對字元
>     - 每當比對失敗，就從當前比對成功的字串片段，取其「次長的相同前綴後綴」大幅挪動 P
>
> ![1549524105095](\willywangkaa\images\1549524105095.png)
>
>
>
> **「相同前綴後綴」**（Prefix-suffix）
>
> ![1549524327132](\willywangkaa\images\1549524327132.png)
>
> **「次長相同前綴後綴」**
>
> - 一個字串的「最長相同前綴後綴」為**原字串**
> - 「最短相同前綴後綴」為**空字串**
> - 「次長相同前綴後綴」就是第二長的「相同前綴後綴」
>
> ![1549524493648](\willywangkaa\images\1549524493648.png)
>
> 窮舉法的過程當中，**當前比對成功的字串片段是 P 的前綴**
>
> - 因為無法預測是 P 的哪個前綴
>   - 所以**預先計算 P 每個前綴的「次長的相同前綴後綴」**
>   - 衍生出了「Failure function」



步驟

1. 預先計算 P 的每種前綴的「次長相同前綴後綴」
   - 意旨算出 P 的「Failure function」
2. 從左往右依序比對字元
   - 比對成功時
     - 繼續比對下個字元
   - 比對失敗時
     - 從比對成功的**字串片段取其「次長的相同前綴後綴」以大幅挪動 P**
   - 當全部比對成功搜尋到 P 時
     - **取 P「次長的相同前綴後綴」以大幅挪動 P**



- Algorithm

```cpp
// pattern[0..m]
void GetFailureFunction(string pattern){
    for k = 1 to pattern.size {
        i = failure[k-1];

        while( pattern[k] != pattern[i+1]   // P[k] != P[ F[...F[k-1]]+1 ]
              && i>=0 ){                    // P[ F[...F[k-1]] ] 仍存在『次長的相同前綴後綴』
            i = failure[i];                 // 以 F[...F[k-1]] 繼續尋找
        }
        if(pattern[k] == pattern[i+1]){     // P[k] == P[ F[...F[k-1]]+1 ]
            failure[k] = i+1;               // F[k] = F[...F[k-1]]+1
        }
    }
}

void Morris_Pratt(string T, string P) {
    if P.size > T.size
        return ERROR;
    GetFailureFunction(P);
    
    // 進行字串搜尋，時間複雜度：O(T)
    for k = 1 to T.size {
        s = -1;                                      // 目前 P 字元比對已成功的位置
        // 比對 P 的下一個尚未比對位置（s+1）
        
        // 若 T[k] != P[s+1]，尋找大幅挪動的步伐數
        // 在 P 中找出 P[1..s] == T[k-s..k] 以大幅挪動 P
        while ( P[s+1] != T[k]                       // T[k] != P[ F[...F[k-1]]+1 ]
              && s >= 0 ) {                          // P[ F[...F[k-1]] ] 仍存在『次長的相同前綴後綴』
            s = failure[s];                          // 以 F[...F[k-1]] 繼續尋找
        }
        
        // T[k] 與 P[s+1] 比對成功
        if (P[s+1] == T[k]) {                         
            s++;                                     // P 字元比對已成功的位置後移一位 
        }
        
        if (s == P.size-1) {                        // P 字元比對已成功的位置已移完
            print( " P出現的位置" + (s-P.size+1) );
            
            
            s = failure[s];                         // 如果字串結尾不是'\0'的時候，就必須挪動 P
                                                    // 如果字串結尾是'\0'的時候，就能省略這一行
        }
    }
}
```





### 時間複雜度分析（均攤分析）

以「Multipop stack」概念作均攤分析，以**字元兩兩比對總次數**作為時間複雜度



（1）進行字串搜尋的過程中

- 「Stack」S 的元素
  - 當下比對成功的字串片段 S
    - **一開始 S 長度是零**
- **若字元比對成功**
  - S 增加一字元，**視為「Push stack」**
- **若字元比對失敗**
  - 大幅挪動 P，S 只剩下「次長的相同前綴後綴」，**視為「Multipop」**
  - 實際上 S 瞬間大幅變短只需要 O(1) ，時間複雜度遠比「Multipop」小



1. 最多有 T 個字元放入 S（S 增加一字元）

2. 最多有 T 個字元彈出 S（大幅挪動 P，S 只剩下「次長的相同前綴後綴」）

$\Rightarrow$**字元兩兩比對的總次數不超過 2T 次**



（2）計算 P 的「Failure function」過程中

原理相同，字元兩兩比對的總次數不超過 2P 次



- **總時間複雜度**
  - O(∣T∣+∣P∣)



### Failure function

在比對失敗時會使用之

因為函數的**定義域**是 Prefix，又稱作 Prefix function 

因為此函數的**值域**是 Border，又稱作 Border function



- **字串函數**
  - 輸入字串的其中一個前綴，**輸出該前綴的「次長的相同前綴後綴」**

![1549524903137](\willywangkaa\images\1549524903137.png)

![1549529928795](\willywangkaa\images\1549529928795.png)

- 計算「Failure function」 
  - Dynamic Programming
  - 分割問題
    - P[0...i] 除去尾端字元 P[i] 
    - 利用已知 P[0...i-1] 的「次長相同前綴後綴」
    - 得到 P[0...i] 的「次長相同前綴後綴」



F[k]：P[0...k] 之「次長的相同前綴後綴」**長度**

（1）將 F[0] 初始化為 -1

- 長度為 1 的子字串，不存在「次長相同前綴後綴」



（2）F[k]：**探討 P[1...k-1] 與 P[k] 之間的關係**

![1549530552160](\willywangkaa\images\1549530552160.png)

- **P[ F[k-1]+1 ] == P[k]**
  - 意旨「**第 k 個字元**」與「**P[1...k-1] 之『次長的相同前綴後綴』下一個字元**」相等
  - $\Rightarrow$ **F[k] = F[k-1]+1**

![1549530722575](\willywangkaa\images\1549530722575.png)



> **對「P[1...k-1] 之『次長的相同前綴後綴』」作探討**
>
> ![1549534207507](\willywangkaa\images\1549534207507.png)



- P[ F[k-1]+1 ] ≠ P[k]
  - 「**第 k 個字元**」與「P[1...k-1] 之『次長的相同前綴後綴』下一個字元」相異



**若存在 P[ F...[F[k-1]]+1 ] == P[k]，則 F[k] = F...[F[k-1]]+1**

![1549535284950](\willywangkaa\images\1549535284950.png)



**若不存在 P[ F...[F[k-1]]+1 ] == P[k]，則 F[k] = -1**

![1549535607960](\willywangkaa\images\1549535607960.png)



上述可以表達為：

- $f[k]\left\{\begin{matrix}
  -1 & if \;k = 0\\ f^m[k-1] +1 & 最小的整數\; m\;使得\; P[f^m[k-1]+1] == P[k]
  \\ -1 & 不存在整數\; m \;可以使得 P[f^m[k-1]+1] == P[k]\end{matrix}\right.$



- Algorithm

```cpp
// pattern[0..m]
void GetFailureFunction(string pattern){
    for k = 1 to pattern.size {
        i = failure[k-1];

        while((pattern[k]!=pattern[i+1]) && // P[k] != P[ F[...F[k-1]]+1 ]
              i>=0){                        // P[ F[...F[k-1]] ] 仍存在『次長的相同前綴後綴』
            i = failure[i];                 // 以 F[...F[k-1]] 繼續尋找
        }
        if(pattern[k] == pattern[i+1]){     // P[k] == P[ F[...F[k-1]]+1 ]
            failure[k] = i+1;               // F[k] = F[...F[k-1]]+1
        }
    }
}
```



### Morris-Pratt Automaton



此演算法可以化作自動機，轉化的時間複雜度為 O( ∣P∣×∣Σ∣ ) 

- Σ 為字元集合



> 化作自動機之後，字串搜尋的過程就變得更簡單了，甚至可以設計成電子迴路
>
> 轉化的原理，是針對每個狀態，都找出經由「Failure function」能到達的狀態們，然後建立轉移邊，連到那些狀態們的下一個狀態

![1549541112414](\willywangkaa\images\1549541112414.png)