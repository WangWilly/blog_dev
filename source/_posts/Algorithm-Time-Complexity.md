title: Algorithm - Time Complexity
author: Willy Wang
tags:
  - Time Complexity
categories:
  - Algorithm
date: 2018-08-28 21:57:00
---
# Time complexity



- 重點
  - Asymptotic notation 考點著重
    - 定義
    - 特性
    - **證明題**、**選擇題**
  - Time complexity 的比較
    - 定義法
    - 極值法 ( limit )：以大資料的手法比較演算法的優劣
    - 對數法 ( log )：當該算法的等級差距大時方便判斷與解釋
  - 計算題要點 ( 離散數學 )
    - $H _n = \Theta(\lg n)$
    - $\log (n !) = \Theta (n \lg n)$
    - $( \log_a n )^b = o (n ^k), k > 0$



## Asymptotic notation



當輸入資料變大時，程式執行的時間以何種趨勢成長



###  符號定義



- $f(n) = O(g(n))$
  - $\exists C, n_0 > 0 , \ni f(n) \leq C \times g(n) , n \geq n_0$
  - 稱 $g(n)$ 為 $f(n)$ 的 Asymptotic upper bound
  - f(n) 的 Order 會**小於** g(n) 的 Order
- $f(n) = \Omega (g(n)) $
  - $\exists C , n_0 > 0  \ni f(n) \geq C \times g(n) , n \geq n_0$
  - 稱 $g(n)$ 為 $f(n)$ 的 Asymptotic lower bound
  - f(n) 的 Order 會**大於** g(n) 的 Order
- $f(n) = \Theta (g(n)) $
  - $\exists C_1 , C_2, n_0 > 0  \ni C_1 \times g(n) \leq f(n) \leq C_2 \times g(n) , n \geq n_0$
  - 稱 $g(n)$ 為 $f(n)$ 的 Asymptotic tight bound
  - f(n) 的 Order 會**等於** g(n) 的 Order
- $f(n) = o (g(n)) $
  - 以比較 Order 的上面來看，是**絕對小於**的意義
  - 用極值法的方式方便解釋
  - Ex：true or false
    - $n = o(2n)$：false
    - $n = o(n^2)$：true
    - $n = O(2n)$：true
    - $n = O(n^2) $：true
- $f(n) = \omega(g(n))$
  - 以比較 Order 的上面來看，是**絕對大於**的意義
  - 用極值法的方式方便解釋。
    - $2n = \omega(n)$：false
    - $n^2 = \omega(n)$：true



**＜Note＞**

- ？若固定 $g(n)$，可以將**所有函數**作以下分類：
  - Ex：$g(n) = n^3$
- 考古題 ( 99政大資料科學 )
  - Q
    - 寫出 2 個在 $O(n^3)$ 中但不在 $o(n^3)$ 的函數
  - A
    - 題目亦等於在問舉出兩個函數在 $\Theta(n^3)$ 的函數
      ，所以 $f(n) = n^3、2n^3$
- 考古題 ( 100 交大 )
	- Q
		- NCTU：$\Theta(n)$、CS：$\Omega(n)$ 下列何者正確？
			- （a） NCTU 總是比 CS 快
			- （b） 當 n $\geq$ 1000000000000 時 NCTU 比 CS 快
			- （c） 兩者執行時間相同
			- （d） 稱 CS 的複雜度為 $\Theta(n)$ 
			- （e） 以上皆非
	- A
		- （e）



### 特性



- $f(n) = O (g(n)) \Rightarrow f(n)+g(n) = O(g(n))$
  - Ex (98 交大資工)
  - $P(n) = \sum_{i = 0}^{d} a_in^i$ 是 d 次多項式，下方表格何者正確？



|      | 是否屬於右側集合 | $O(n^k)$ | $o(n^k)$ | $\Omega(n^k)$ | $\omega(n^k)$ | $\Theta(n^k)$ |
| :--: | ---------------- | -------- | -------- | ------------- | ------------- | ------------- |
| P(n) | $k>d$            | true     | true     |               |               |               |
| P(n) | $k < d$          |          |          | true          | true          |               |
| P(n) | $k = d$          | true     |          | true          |               | true          |





> - Ex (98 交大資工)
>
> 寫出 $O(n^2) + \Theta(n^2)$ 最適合的等級
> (理解： $\exists f(n) \in O(n^2) , \exists g(n) \in \Theta(n^2) \quad f(n) + g(n) \in ？$) 
> $$
> O(n^2) + \Theta(n^2) = \Theta(n^2)
> $$
>



> - Ex (100 中央)
>
> Prove or disprove：$f(n) + g(n) = \Theta(\;max｛f(n), g(n)｝)$
>
> Proof：(以定義證明)
> $$
> \exists C_1 = 1, C_2 = 2, n_0 = 1 \\ \ni n \geq n_0, C_1(\;max｛f(n), g(n)｝) \leq f(n) + g(n) \leq C_2(\;max｛f(n), g(n)｝)
> $$
>



- $f(n) = O(g(n)) 且 f(n) = \Omega (g(n)) \Leftrightarrow f(n) = \Theta(g(n))$ 
  - Ex (95 台大資工) **(離散數學)**
    - Q $\sum_{i = 0}^n i^5 = \Theta(n^a), a = ？$
    - A
      1. Prove：$\sum_{i = 0}^n = O(n^6)$ 
      2. Prove：$\sum_{i = 0}^n = \Omega(n^6)$ 



### 函數比較等級



> - **L'Hôpital's rule 的使用**
>   - 若 $\lim_{n\rightarrow\infty}\frac{f(n)}{g(n)}$ 是**不定型 (相除為 0 或 無限大)**，則該函數等於 $\lim_{n\rightarrow\infty}\frac{f'(n)}{g'(n)}$ 
>
>
>
> - $(\ln x)' = \frac{1}{x}$ 
>
>
>
> - log n 的底只要是**常數**其**等級均相同**。
>   - Ex
>     - $\ln n, \lg n, \log n, \log_{100}n$等級均相等
>



#### 定義法



- 適用時機
  - **題目分數多。**
  - **函數的型態簡單帶值可算。**
- Ex (96 成大資工)
  - True or false (10%)
  - Q
    - $n^2 + n\lg n + \frac{n}{2} = O(n^8)$
  - A
    - $\exists C = 5, n_0 = 10\ni n\geq n_0, n^2 + n \lg n + \frac{n}{2} \leq c \times n^8$
- Ex (91 交大資工)
  - Prove the following is incorrect
  - Q
    - $\frac{n^2}{\log n} = \Theta(n^2)$
  - A
    - (矛盾證明法)
    - $\frac{n^2}{\log n} = \Theta(n^2) \Rightarrow \frac{n^2}{\log n} = O(n^2), \frac{n^2}{\log n} = \Omega(n^2)$ ，後者明顯錯誤所以從後者開始證明。
    - 設$\frac{n^2}{\log n} = \Omega(n^2)$成立，則 $\exists C, n_0 > 0 \ni n > n_0, \frac{n^2}{\log n} \geq C \times n^2$ 
    - $\Rightarrow \frac{1}{\log n} \geq C (\rightarrow\leftarrow)$，當 $n$ 成長時$\frac{1}{\log n}$ 會無限靠近 0 ，導致不存在 C > 0 所以矛盾。
- Ex (100 中央資工)
  - Prove or disprove
  - $f(n) = \Theta(g(n)) \Rightarrow h(f(n)) = \Theta(h(g(n)))$，其中 $h(*)$ 為**遞增函數**。
  - false， 直接舉例 $f(n) = 2n, g(n) = n \Rightarrow f(n) = \Theta(g(n))$ 
  - 而取 $h(n) = 2^{n}$ 為遞增函數，但 $h(f(n)) \ne O(h(g(n)))$



#### 極值法



- 適用時機
  - 函數**型態複雜**，但可藉由**微分**使之容易分析。
  - 證明 $o, \omega$ 時可當作**該集合之定義以分析**。
- 使用方法
  - $\lim_{n\rightarrow\infty} \frac{f(n)}{g(n)} = 0 \Leftrightarrow f(n) = o(g(n))$ 
  - $\lim_{n\rightarrow\infty} \frac{f(n)}{g(n)} = \infty \Leftrightarrow f(n) = \omega(g(n))$ 
  - $ \exists L > 0 \;is \;constant \ni \lim_{n\rightarrow\infty} \frac{f(n)}{g(n)} = L \Leftrightarrow f(n) = \Theta(g(n))$



- Ex
  - $f(n) = \log_34n, g(n) = \log_43n$，問 $f(n)$ 在 $g(n)$ 的會在哪個等級集合 ($O, \Omega, \Theta$) 之中？
  - $\lim_{n\rightarrow\infty}\frac{log_34n}{\log_43n} =_{微分} \lim_{n\rightarrow\infty}\frac{(\frac{\ln 4n}{\ln3})'}{(\frac{\ln 3n}{\ln 4})'}$
  - $\Rightarrow lim_{n \rightarrow \infty} \frac{ \frac{4}{4n\ln3} }{\frac{3}{3n\ln4}} \Rightarrow  \frac{\ln4}{\ln3}$ 為常數，所以 $f(n) = \Theta ,\Omega , O (g(n))$。
- Ex ( 96 成大資工 ) (10%)
  - True or false
  - $n^b = o(a^n)$, 其中 $a > 1, b \in \mathbb{R}$
  - true，$\lim_{n\rightarrow \infty} \frac{n^b}{a^n} =_{兩邊取微分} \lim_{n\rightarrow \infty} \frac{b \times n^{b-1}}{\ln a\times a^n} = \ldots = \lim_{n\rightarrow\infty} \frac{b!}{(\ln a)^n\times a^n} = 0$ 所以正確。
- **Ex ( 98 台大電機 )**
  - true or false
  - 對任何**正數** a, b 而言 $n^b = o(a^n)$ 。
  - **False，陷阱！**因為題目只有說 a, b 為正數而非正整數，所以當 a 為**分數**時，此命題錯誤。



#### 對數法



- 適用時機
  - 題目分數少的計算題。
  - 函數為指數函數型態。
  - 函數為階乘型態。($\log n! = \Theta(n\log n)$)
- 定義
  - $\log(f(n)) = o, \omega(\log(g(n))) \Rightarrow f(n) = o, \omega (g(n))$
  - 而 $\log(f(n)) = \Theta(\log(g(n)))$ 則**無法判斷等級**，因為等級過於相同
    ，取對數後無法判斷，要使用別的方法。
- Ex
  - 比較 $f(n) = 1.1^{0.01n}, g(n) = n^{100}$ 的等級。
  - $\log(f) = 0.01n \times log 1.1, log(g) = 100 \times log n \Rightarrow \lim_{n\rightarrow\infty}\frac{\log(f)}{\log(g)} = \infty$，所以 $f(n) = \omega(g(n))$。

- Ex (98 交大)

  - Prove：(log n)! 不是 Polyniminally bounded ( = O($n^k$))。
  - ＜分析＞ 若 f(n) 是 Polyniminally bounded，則 $\exists k \in R \ni f(n) = O(n^k)$；
    $\log f = O(\log n^k) = O(\log n)$ ，若為 Polyniminally bounded 則等級小於 log n
    $\log((\log n)!) = \Theta(\log n (\log\log n))$ 其等級大於 log n 也就是 $\log(\log n)! = \omega(\log n)$




## 複雜度計算



### $\Theta$ 集合的求取



#### Close form 的求取



- **Close form 最高次項即為其 Tight bound。**
  - $T(n) = 1 + ...+n = \frac{n(1+n)}{2} = \frac{n^2+n}{2} = \Theta(n^2)$
- 並不是所有函數都有 Close form。



#### 使用 Upper bound 與 Lower bound 的夾擊



- 若 $f(n) = O, \Omega (g(n))$ 同時成立，稱 $g(n)$ 為 $f(n)$ 的 Asymptotic tight bound。
  - Ex $T(n) = \sum_{i=1}^n i^5 = \Theta(n^a)$，求 a = ？
  - 先證明 $T(n) = \sum_{i=1}^n i^5 = O(n^6)$、再證明$T(n) = \sum_{i=1}^n i^5 = \Omega(n^a)$ 即可。
- 調和級數( $\mathbb{H}_n = 1 + \frac{1}{2} + \frac{1}{3} + ... +\frac{1}{n}= \Theta(\lg n)$ )因為調和級數沒有 Close form，必須使用**夾擠法**
  - $\because \mathbb{H}_n - 1 = \sum_{i = 1}^n \frac{1}{i} - 1 \leq \int_1^n \frac{1}{n} dx = (\lg n - \lg 1) = \lg n \\ \therefore \mathbb{H}_n \leq \ln n + 1 \leq 2 \times \ln n \Rightarrow \exists C = 2, n_0 \geq 3 \ni n \geq n_0 , \\  \mathbb{H}_n \leq C \times \lg n \\ \Rightarrow \mathbb{H}_n = O(\lg n) \\ \because \mathbb{H}_n = \sum_{i = 1}^n \frac{1}{n} \geq \int_1^n \frac{1}{n} dx = \ln n \\ \therefore \mathbb{H}_n \geq \ln n \Rightarrow \exists C = 1, n_0 \geq 1 \ni n \geq n_0 , \mathbb{H}_n \geq C \times \lg n \\ \Rightarrow \mathbb{H}_n = \Omega(\lg n) \\ \Rightarrow \mathbb{H}_n = \Theta(\lg n)$
- **＜Note＞ 收斂(Converge)的級數**  

  - $T(n) = 1^a + \frac{1}{2^a} + \frac{1}{3^a} + \ldots = \sum_{i = 1}^n \frac{1}{i^a} = \frac{\pi^2}{6}, for\; some\; a = 2, 3 ...$ 
  - $\Rightarrow T(n) = \Theta(1)$
  - [複變分析證明](https://www.quora.com/What-is-1-1-2-+-1-2-2-+-1-3-2-till-infinity)
- $\log n! = \Theta(n\lg n)$ 
  - 證明：
    1. Prove $\log(n!) = O(n\lg n)$
       - $\log n! = \log ( \Pi_{i = 1}^n i ) = \sum_{i = 1}^n \log i \leq \sum_{i = 1}^n \log n = n \log n$
       - $\therefore \exists C = 1, n_0 = 1 \ni n\geq n_0, \log n \leq n \log n \Rightarrow \log n! = O(n\lg n)$ 
    2. Prove $\log(n!) = \Omega(n\lg n)$ ( 離散筆記本 P.118 )
       - $\log n! = \log ( \Pi_{i = 1}^n i ) \\ = \sum_{i = 1}^n \log i \geq \sum_{i = 1}^{ceil(\frac{n}{2})} \log  ceil( \frac{n}{2}) \geq \frac{n}{2} \log \frac{n}{2}  \\ = \frac{n}{2} (\log n - \log 2)\approx \frac{1}{2}n\log n$ 
       - $\therefore \exists C = \frac{1}{2}, n_0 = 1 \\ \ni n\geq n_0, \log n \geq \frac{1}{2}n \log n \Rightarrow \log n! = \Omega(n\lg n)$ 
    3. $\therefore \log n! = \Theta(n\lg n)$
- **Ex (96 台大資工)**
  - $T(n) = \sum_{k = 1}^n k^2(\log k)^3 = \Theta(n^d(\log n)^e)$，d = ？、e = ？
  - ＜想法＞：拆開
    $1^2(\log 1)^3 + 2^2(\log 2)^3 + ... +n^2(\log n)^3 \Rightarrow_{\leq} 1^2(\log n)^3 + 2^2(\log n)^3 + ... +n^2(\log n)^3$
    提出 $(\log n)^3 \times \sum_{i = 1}^n i^2  = O(n^3(\log n)^3)$  所以 d = 3, e = 3



- $(\log_an)^b = o(n^k), k > 0$ 
  - Ex：$(\lg n)^{100} = o(n^{0.0000001})$
  - **Ex (96 輔大資工)**
    - Prove $(\log n)^3 = O(n^\frac{1}{16})$
    - (函數**型態複雜**不適合使用定義證明，這裡採取**極值法**) 
      $\lim_{n\rightarrow\infty} \frac{(\log n)^3}{n^{\frac{1}{16}}} \\ =_{同取微分} \lim_{n\rightarrow\infty} \frac{\frac{3}{n (\ln 10)^3}\cdot (\ln n)^2}{\frac{1}{16}\cdot n^{\frac{-15}{16}}} \\ =_{整理} Constant\cdot \lim_{n\rightarrow\infty}\frac{(\ln n)^2}{n^{\frac{1}{16}}} =\ldots \\ \Rightarrow_{趨近於, n \rightarrow \infty} 0 \\ \Rightarrow (\log n)^3 = o(n^\frac{1}{16}) \Rightarrow_{?} (\log n)^3 = O(n^\frac{1}{16})$ 
- **Ex** 比較 $n^{1 + \epsilon}$ 與 $\frac{n^2}{\log n}$ 的等級，其中 $0<\epsilon < 1$。
  - ＜猜測＞：**後者比較大**
  - ＜分析＞：$n^{1+\epsilon} = n^{2-\delta}, 0<\delta<1 \Rightarrow \frac{n^2}{n^\delta} \\ \because \log n  = o(n^\delta) \\ \therefore n^{1 + \epsilon}  = O(\frac{n^2}{\log n})$



# 補充例題



Example（101交通大學資料結構與演算法）

Consider the following three problem. **Assume that the only operations allowed on the data are**

- *comparing the values of two floating-point numbers and identifying the larger value*;
- *comparing the distance between two array entries (the absolute value of the difference between the two array entries) with the distance between two other array entries*;
- *swapping two entries in the array*.

Further assume that **each allowed operation has unit cost**. What are the worst-case optimal asymptotic running times for algorithms that solve these problems?

- （1）**Nearest neighbors**: given an **unsorted array** of n floating-point numbers as input, return two of the numbers that are closest in value to each other.
  - （A）$O(n\log n)$
  - （B）$O(\log n)$
  - （C）$O(n^2)$
  - （D）$O(n^3)$
  - （E）$O(n)$

排序後元素兩兩算出距離 d，再從所有距離裡找出最小值

$\Rightarrow O(n\log n) + O(n) + O(n) = O(n\log n)$

- （2）**Farthest neighbors**: given an **unsorted array** of n floating-point numbers as input, return two of the numbers that are farthest in value from each other.

  - （A）$O(n\log n)$
  - （B）$O(\log n)$
  - （C）$O(n^2)$

  - （D）$O(n^3)$
  - （E）$O(n)$

找最大值、最小值

$\Rightarrow O(n) + O(n) = O(n)$ 

- （3）Given a floating-point number to find a closest value in a **sorted array** of n floating-point numbers.

  - （A）$O(n\log n)$
  - （B）$O(\log n)$
  - （C）$O(n^2)$

  - （D）$O(n^3)$
  - （E）$O(n)​$

使用「Binary search」找到最接近的值

$\Rightarrow O(\log n)$ 